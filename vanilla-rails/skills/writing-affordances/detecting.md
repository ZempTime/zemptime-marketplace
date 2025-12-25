# Detecting Affordance Opportunities

## Table of Contents

- [Primary Indicators](#primary-indicators)
- [Decision Framework](#decision-framework)
- [Clarifying Questions](#clarifying-questions)
- [Example: Multiple Valid Designs](#example-multiple-valid-designs)
- [Anti-Indicators](#anti-indicators)
- [Quick Reference](#quick-reference)

## Primary Indicators

**1. Repetitive Method Prefixes** (3+ methods)
```ruby
entropy_auto_clean_at, entropy_days_before_reminder, entropy_reminder_sent?
# Prefix "entropy" = affordance noun
```

**2. Multi-Association Coordination** (2+ `has_many`)
```ruby
def assign(user)
  # Touches assignments AND watches AND events
  transaction do
    assignments.create!(user: user)
    watch_by(user)
    track_event :assigned
  end
end
# Coordinating affordance provides unified interface
```

**3. Unclear Relationship Clusters**
```ruby
# Related but scattered, connection non-obvious
auto_clean_at, days_before_reminder, reminder_sent?
# Group under explicit "entropy" concept
```

**4. Fat Model with Mixed Domains** (50+ methods across concepts)
```ruby
# Closeable (4 methods) + Assignable (6) + Entropic (5) + Watchable (4)
# Extract each domain to focused concern + affordance
```

## Decision Framework

```
Methods share prefix (3+)?
  |-- Yes --> Extract affordance (prefix = noun)
  |-- No --> Continue

Coordinate 2+ associations?
  |-- Yes --> Likely affordance
  |-- No --> Continue

Implicit behavior only (callbacks/validations)?
  |-- Yes --> Concern, not affordance
  |-- No --> Continue

Single association only?
  |-- Yes --> Association extension
  |-- No --> Affordance or concern + affordance
```

## Clarifying Questions

When decision unclear, ask:

### 1. Call-Site Clarity (The North Star)
"Would someone understand what this does from the call site?"

- `card.entropy.auto_clean_at` - Self-evident
- `card.entropy_auto_clean_at` - Prefix smell
- `include Searchable` - Clear trait

**Use the clearest option. Don't force a pattern.**

### 2. Explicit vs Implicit
"Do I call this, or does Rails call it?"

**Explicit (you call):**
```ruby
card.entropy.auto_clean_at
event.description.to_html
# --> Affordance
```

**Implicit (Rails calls):**
```ruby
after_save :update_search_index
validates :title, presence: true
# --> Concern
```

**Both:** Concern provides infrastructure, affordance provides API

### 3. Composition Test
"Can I pass just this piece to a method?"

```ruby
# Makes sense to pass around --> Affordance
def render_notification(description)
  description.to_html
end

render_notification(event.description)

# Doesn't make sense --> Concern
include Searchable  # Not a passable object
```

### 4. Prefix Clustering
"Do 3+ methods share a prefix that reveals a noun?"

```ruby
entropy_calculate, entropy_remind, entropy_cleanup
# Prefix "entropy" IS the affordance
```

**Pattern:** Extract noun, remove prefix

### 5. Infrastructure vs Operations
"Need both associations/callbacks AND explicit methods?"

**Both --> Concern + Affordance:**
```ruby
module Card::Entropic
  extend ActiveSupport::Concern

  included do
    has_one :activity_spike     # Infrastructure
    after_save :check_entropy   # Callbacks
  end

  def entropy                   # Entry to affordance
    Card::Entropy.for(self)
  end
end
```

**Just infrastructure --> Concern only:**
```ruby
module Searchable
  included do
    after_save :reindex  # Automatic only
  end
end
```

**Just operations --> Affordance (entry in model):**
```ruby
def description
  @description ||= Event::Description.new(self, user)
end
```

## Example: Multiple Valid Designs

**Event rendering can be implemented three ways:**

**A. Concern only (automatic):**
```ruby
module Event::Describable
  included do
    after_create :cache_description
  end
end
```

**B. Affordance only (explicit):**
```ruby
event.description.to_html
event.description.to_plain_text
```

**C. Both (automatic + explicit):**
```ruby
module Event::Describable
  included do
    after_create :cache_description  # Automatic
  end

  def description
    Event::Description.new(self, user)  # Explicit when needed
  end
end
```

**Let domain guide the choice.**

## Anti-Indicators

**Use Concern:** Pure implicit behavior (callbacks, validations only)

**Use Association Extension:** Operations on single `has_many` only

**Use Plain Method:** Only 1-2 related methods

## Quick Reference

| What You See | Pattern |
|--------------|---------|
| `entropy_*` prefix (3+ methods) | Extract `Entropy` affordance |
| Coordinates `assignments` + `watches` | Coordinating affordance |
| 50+ methods, multiple domains | Extract by domain to affordances |
| Callbacks/validations only | Concern, not affordance |
| Explicit operations called | Affordance (possibly via concern) |
| Single `has_many` operations | Association extension |
| 1-2 methods | Plain method |
