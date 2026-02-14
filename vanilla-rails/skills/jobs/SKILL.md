---
name: vanilla-rails-jobs
description: Use when writing background jobs or async operations - enforces thin job wrappers (3-5 lines) that delegate to models using _later/_now naming pattern
---

# Vanilla Rails Jobs

**Jobs are thin wrappers (3-5 lines). ALL business logic lives in models.**

## The Pattern

```ruby
# Model concern - WHERE THE LOGIC LIVES
module Card::ClosureNotifications
  extend ActiveSupport::Concern

  included do
    after_update :notify_watchers_later, if: :just_closed?
  end

  def notify_watchers_later
    Card::ClosureNotificationJob.perform_later(self)
  end

  def notify_watchers_now
    watchers.each do |watcher|
      CardMailer.closure_notification(watcher, self).deliver_now
      Notification.create!(user: watcher, card: self, action: 'closed')
    end
  end
end

# Job - ONLY delegates (3 lines)
class Card::ClosureNotificationJob < ApplicationJob
  def perform(card)
    card.notify_watchers_now
  end
end
```

**Flow:** Callback → `_later` → enqueue job → job calls `_now` → logic executes

## Why Thin Jobs

- **Test `_now` synchronously** — no job infrastructure needed
- **Reusable** — call `_now` in console, tests, anywhere
- **Debuggable** — stack traces point to model, not job framework

## Edge Cases

**Multi-model** — primary model orchestrates:
```ruby
class User::DigestJob < ApplicationJob
  def perform(user); user.send_digest_now; end
end
```

**Utility/cleanup** — use class methods:
```ruby
class Session::CleanupJob < ApplicationJob
  def perform; Session.cleanup_expired_now; end
end
```

**Error handling** — ActiveJob retries + model errors:
```ruby
class Card::SyncJob < ApplicationJob
  retry_on ExternalAPI::Error, wait: 5.minutes
  def perform(card); card.sync_to_external_system_now; end
end
```

## Red Flags

| Red flag | Fix |
|----------|-----|
| Job > 5 lines (excluding `retry_on`) | Move logic to model |
| Business logic in job | Move to `_now` method on model |
| `perform(card_id)` then `Card.find` | `perform(card)` — let ActiveJob serialize |
| No `_later`/`_now` naming | Add suffixes |
| Missing `_now` method | Always create — needed for testing |
| Job sends emails directly | Model orchestrates, mailer delivers |
| Job has conditionals/loops | Domain logic goes in model |

## Quick Check

- [ ] Job is 3-5 lines, calls one model method
- [ ] Receives model instance (not ID)
- [ ] No queries, conditionals, or loops in job
- [ ] Model has `_later` method (enqueues)
- [ ] Model has `_now` method (logic)
