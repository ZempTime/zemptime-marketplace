---
name: vanilla-rails-models
description: Use when writing Rails models - enforces state-as-records not booleans, concerns as adjectives namespaced under model, and concern extraction triggers
---

# Vanilla Rails Models

Rich domain models with concerns. Decompose with concerns, not services.

**For method ordering and private indentation rules, see vanilla-rails-style.**

## State as Separate Records (NOT Booleans)

Don't use boolean columns for state. Create state records that capture who/when.

```ruby
# Bad - boolean
add_column :cards, :starred, :boolean, default: false

# Good - state record
create_table :stars, id: :uuid do |t|
  t.uuid :card_id, null: false
  t.uuid :user_id, null: false
  t.timestamps
end
```

```ruby
class Card < ApplicationRecord
  has_one :star, dependent: :destroy

  def star(user: Current.user)
    create_star!(user: user) unless starred?
  end

  def starred?
    star.present?
  end
end
```

**Binary state (one per item):** `has_one` — `closure`, `triage`, `goldness`

**Multi-user state:** `has_many` — `pins`, `watches`, `assignments`

## Concerns as Adjectives, Namespaced Under Model

Name as adjectives (-able/-ible ONLY). Namespace under the model. File at `app/models/card/closeable.rb`.

| Wrong | Right | Why |
|-------|-------|-----|
| `Card::Closing` | `Card::Closeable` | Verb → adjective |
| `Card::Stars` | `Card::Starrable` | Noun → adjective |
| `Card::Closed` | `Card::Closeable` | Past participle → capability |
| `Starrable` | `Card::Starrable` | Must namespace under model |
| `concerns/starrable.rb` | `card/starrable.rb` | File under model directory |

**Full pattern:**

```ruby
# app/models/card/closeable.rb
module Card::Closeable
  extend ActiveSupport::Concern

  included do
    has_one :closure, dependent: :destroy
    scope :closed, -> { joins(:closure) }
    scope :open, -> { where.missing(:closure) }
  end

  def close(user: Current.user)
    unless closed?
      transaction do
        create_closure!(user: user)
        track_event :closed, creator: user
      end
    end
  end

  def reopen(user: Current.user)
    if closed?
      transaction do
        closure&.destroy
        track_event :reopened, creator: user
      end
    end
  end

  def closed?
    closure.present?
  end
end
```

**Multi-user pattern:**

```ruby
module Card::Pinnable
  extend ActiveSupport::Concern

  included do
    has_many :pins, dependent: :destroy
  end

  def pinned_by?(user)
    pins.exists?(user: user)
  end

  def pin_by(user)
    pins.find_or_create_by!(user: user)
  end

  def unpin_by(user)
    pins.find_by(user: user)&.destroy
  end
end
```

## When to Extract

- Feature adds 3+ methods to model
- Clear adjective name exists (-able/-ible)
- Even if only one model uses it (decomposition, not just reuse)
- Model exceeds ~100 lines

## Red Flags

| Red flag | Fix |
|----------|-----|
| Boolean column for state | State record table (see data-modeling) |
| Concern not ending in -able/-ible | Rename immediately |
| Concern not namespaced under model | Move to `Card::Closeable` |
| Concern in `app/models/concerns/` for single model | Move to `app/models/card/` |
| Service object for domain logic | Rich model method instead |
| "Only extract if reused" | Extract for decomposition at 3+ methods |
