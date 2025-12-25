# Events & Audit Trail

Reference implementation for event-driven architecture.

## Concept

Every significant action creates an Event record. Events drive:
- Activity timelines
- Notifications
- Webhooks
- Audit logging

## Schema

```ruby
create_table "events", id: :uuid do |t|
  t.uuid "account_id", null: false
  t.uuid "eventable_id"
  t.string "eventable_type"
  t.uuid "user_id"
  t.string "action", null: false
  t.json "particulars", default: {}
  t.datetime "created_at", null: false
  t.index ["eventable_type", "eventable_id"]
end
```

## Event Model

```ruby
class Event < ApplicationRecord
  belongs_to :account
  belongs_to :eventable, polymorphic: true, optional: true
  belongs_to :user, optional: true

  after_create_commit :relay_later

  def relay_later
    Event::RelayJob.perform_later(self)
  end

  def relay_now
    notify_watchers
    trigger_webhooks
  end
end
```

## Eventable Concern

```ruby
module Eventable
  extend ActiveSupport::Concern

  included do
    has_many :events, as: :eventable, dependent: :destroy
  end

  def track_event(action, particulars: {}, user: Current.user)
    events.create!(
      account: account,
      user: user,
      action: action,
      particulars: particulars
    )
  end
end
```

## Usage

```ruby
class Card < ApplicationRecord
  include Eventable

  def close(user: Current.user)
    transaction do
      create_closure!(user: user)
      track_event :closed, user: user
    end
  end

  def move_to(column)
    old_column = self.column
    update!(column: column)
    track_event :moved, particulars: {
      from_column_id: old_column.id,
      to_column_id: column.id
    }
  end
end
```

## Common Actions

- `created`, `updated`, `destroyed`
- `closed`, `reopened`
- `moved`, `assigned`, `unassigned`
- `commented`, `mentioned`
- `tagged`, `untagged`

## Particulars

Action-specific data in JSON:

```ruby
# Move
{ from_column_id: "abc", to_column_id: "xyz" }

# Assignment
{ assignee_id: "123" }

# Comment
{ comment_id: "456", body_preview: "First 100..." }
```

## Relay Job

```ruby
class Event::RelayJob < ApplicationJob
  def perform(event)
    event.relay_now
  end
end
```

## Activity Timeline

```ruby
@card.events.includes(:user).order(created_at: :desc)
```

## Benefits

- Complete audit trail
- Activity feeds for free
- Decoupled notifications
- Decoupled webhooks
- Easy to add event types
