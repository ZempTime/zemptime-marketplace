# Implementing Affordances

## Table of Contents

- [Core Principles](#core-principles)
- [Concerns and Affordances](#concerns-and-affordances)
- [Entry Point Patterns](#entry-point-patterns)
- [PORO Affordance Class](#poro-affordance-class)
- [Naming & Organization](#naming--organization)
- [Common Patterns](#common-patterns)
- [Memoization Rules](#memoization-rules)
- [Anti-Patterns](#anti-patterns)
- [Verification](#verification)

## Core Principles

- **Focused Interface:** 3+ operations around single noun/concept
- **Composable:** Return first-class object you can pass around
- **Discoverable:** Class docs with usage examples
- **Encapsulated:** Private delegation to wrapped objects

## Concerns and Affordances

**Not competing patterns - they work together:**

**Concerns:** Associations, callbacks, validations, shared setup
**Affordances:** Explicit operations, composable interfaces

**Three valid patterns:**

1. **Concern only** - Pure infrastructure (e.g., `Searchable` with callbacks)
2. **Affordance only** - Entry in model, no concern needed
3. **Concern + Affordance** - Concern provides infrastructure, affordance provides API

## Entry Point Patterns

### Pattern 1: Entry via Concern (With Infrastructure)

Use when: Affordance needs associations, callbacks, or validations

```ruby
# app/models/card/entropic.rb
module Card::Entropic
  extend ActiveSupport::Concern

  included do
    has_one :activity_spike, dependent: :destroy  # Infrastructure
    after_save :check_entropy_status
  end

  def entropy  # Entry to affordance
    Card::Entropy.for(self)
  end

  def entropic?
    entropy.present?
  end
end
```

### Pattern 2: Entry Directly in Model

Use when: No concern infrastructure needed

```ruby
# app/models/event.rb
class Event < ApplicationRecord
  def description
    @description ||= Event::Description.new(self, Current.user)
  end
end
```

**Don't create concern just for entry point.**

### Pattern 3: Factory Method Pattern

Use when: Affordance may not apply to all instances

```ruby
# Entry returns nil when not applicable
def entropy
  Card::Entropy.for(self)  # Returns nil if card.last_active_at.nil?
end

# Class factory method
class Card::Entropy
  class << self
    def for(card)
      return unless card.last_active_at
      new(card, card.auto_postpone_period)
    end
  end
end
```

## PORO Affordance Class

**Always:** Plain Ruby object, no ActiveRecord inheritance

```ruby
# app/models/card/entropy.rb

# Calculates entropy timing for auto-postponement
#
# Usage:
#   card.entropy.auto_clean_at
#   card.entropy.days_before_reminder
#   card.entropy.reminder_due?
#
class Card::Entropy
  attr_reader :card, :auto_clean_period

  class << self
    def for(card)
      return unless card.last_active_at
      new(card, card.auto_postpone_period)
    end
  end

  def initialize(card, auto_clean_period)
    @card = card
    @auto_clean_period = auto_clean_period
  end

  def auto_clean_at
    card.last_active_at + auto_clean_period
  end

  def days_before_reminder
    (auto_clean_period * 0.25).seconds.in_days.round
  end

  def reminder_due?
    Time.current >= reminder_at
  end

  private

    def reminder_at
      auto_clean_at - days_before_reminder.days
    end
end
```

**Always add class-level usage examples.**

## Naming & Organization

### Class Names

**Pattern:** `Parent::Concept` or `ParentConcept`

| Entry Method | Class Name |
|--------------|------------|
| `card.entropy` | `Card::Entropy` |
| `event.description` | `Event::Description` |
| `notifier.for(source)` | `Notifier` (factory) |

### File Organization

```
app/models/
├── card.rb
├── card/
│   ├── entropy.rb              # Affordance PORO
│   ├── entropic.rb             # Concern with entry point
│   ├── closeable.rb            # Concern (no affordance needed)
│   └── activity_spike/
│       └── detector.rb         # Nested affordance
├── event.rb
└── event/
    └── description.rb          # Affordance PORO
```

### Module Namespacing

```ruby
# app/models/card/entropy.rb
class Card::Entropy  # Namespaced under parent
  # ...
end

# app/models/event/description.rb
class Event::Description
  # ...
end
```

## Common Patterns

### Multiple Output Formats

```ruby
class Event::Description
  include ActionView::Helpers::TagHelper

  def initialize(event, user)
    @event = event
    @user = user
  end

  def to_html
    to_sentence(creator_tag, card_title_tag).html_safe
  end

  def to_plain_text
    to_sentence(creator_name, card.title)
  end

  private

    attr_reader :event, :user

    delegate :card, :creator, to: :event

    def to_sentence(*parts)
      parts.compact.join(" ")
    end
end
```

### Factory Dispatching

```ruby
class Notifier
  class << self
    def for(source)
      case source
      when Event
        notifier_for_event(source)
      when Mention
        MentionNotifier.new(source)
      end
    end

    private

      def notifier_for_event(event)
        "Notifier::#{event.eventable.class}EventNotifier"
          .safe_constantize
          &.new(event)
      end
  end

  def notify
    recipients.map { |r| create_notification(r) } if should_notify?
  end
end
```

### Coordinating Detection Logic

```ruby
class Card::ActivitySpike::Detector
  def initialize(card)
    @card = card
  end

  def detect
    if has_activity_spike?
      register_activity_spike
      true
    else
      false
    end
  end

  private

    attr_reader :card

    def has_activity_spike?
      card.entropic? && (
        multiple_people_commented? ||
        card_was_just_assigned? ||
        card_was_just_reopened?
      )
    end

    def multiple_people_commented?
      recent_commenters.count > 1
    end

    def recent_commenters
      card.comments.where("created_at > ?", 1.day.ago).distinct(:creator_id)
    end
end
```

### Chainable Affordances

```ruby
class Board::Cards
  def initialize(board)
    @board = board
  end

  def for_user(user)
    Board::UserCards.new(self, user)  # More granular
  end

  def active
    board.cards.active
  end
end

# Usage
board.cards.for_user(user).visible
```

### Passing as Dependencies

```ruby
# Narrow interface instead of full model
def render_notification_preview(description)
  description.to_plain_text.truncate(100)
end

render_notification_preview(event.description)  # Clear dependencies
```

## Memoization Rules

**Memoize:** Stateless wrapper (no parameters beyond parent)
```ruby
def entropy
  @entropy ||= Card::Entropy.for(self)
end

def description
  @description ||= Event::Description.new(self, Current.user)
end
```

**Don't memoize:** Parameterized affordances
```ruby
def entropy(as_of: Time.current)
  Card::Entropy.new(self, as_of: as_of)  # Different params = different instances
end
```

## Anti-Patterns

**Single-method wrapper:**
```ruby
class Permissions
  def can_edit?  # Only method - use plain method instead
  end
end
```

**ActiveRecord inheritance:**
```ruby
class Card::Entropy < ApplicationRecord  # WRONG - use PORO
end

class Card::Entropy  # Correct
  def initialize(card, period)
    @card = card
    @period = period
  end
end
```

**Memoizing parameterized:**
```ruby
def entropy(as_of: Time.current)
  @entropy ||= Card::Entropy.new(self, as_of: as_of)  # WRONG - ignores param changes
end
```

**Creating concern just for entry point:**
```ruby
# WRONG - unnecessary concern
module Event::Describable
  def description
    Event::Description.new(self)
  end
end

# RIGHT - put entry directly in model
class Event < ApplicationRecord
  def description
    @description ||= Event::Description.new(self)
  end
end
```

## Verification

- [ ] PORO (no ActiveRecord inheritance)
- [ ] 3+ methods (not single-method wrapper)
- [ ] Class docs with usage examples
- [ ] Memoization only for stateless
- [ ] Private delegation to wrapped objects
- [ ] File: `app/models/<parent>/<affordance>.rb`
- [ ] Named: `Parent::Concept` pattern
