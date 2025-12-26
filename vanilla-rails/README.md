# vanilla-rails

This is an experimental take on distilling the ideas and approaches in https://github.com/basecamp/fizzy and turning them into a set of skills which can ship some great rails code.

Inspired by https://github.com/obra/superpowers (and strongly encouraged to use that with this!)

## What is this?

A Claude Code plugin that teaches vanilla Rails conventions:
- **CRUD controllers** - No custom actions, extract resources for state changes
- **Rich models** - Concerns as adjectives, state-as-records pattern
- **Fixtures** - Always fixtures, never factories
- **Thin jobs** - `_later`/`_now` pattern, logic in models
- **Hotwire** - Turbo Streams, focused Stimulus controllers
- **Small PRs** - 2-5 files, one sentence description

These patterns come from analyzing production Basecamp applications and distilling the key conventions.

## Installation

### Option 1: Via Marketplace (Recommended)

```bash
claude marketplace add https://github.com/zemptime/vanilla-rails-marketplace
claude plugin install vanilla-rails
```

### Option 2: Local Development

```bash
git clone https://github.com/zemptime/vanilla-rails.git
cd vanilla-rails
claude --plugin-dir .
```

## Commands

| Command | Purpose |
|---------|---------|
| `/shape` | Apply vanilla-rails patterns to a feature, produce structured handoff for planning |

**Workflow with superpowers:**
```
superpowers:brainstorming → /shape → superpowers:writing-plans
```

## Skills Included

| Skill | When It Activates |
|-------|-------------------|
| `vanilla-rails-controllers` | Writing Rails controllers |
| `vanilla-rails-models` | Writing Rails models or concerns |
| `vanilla-rails-data-modeling` | Designing database schema, writing migrations |
| `vanilla-rails-delegated-types` | 5+ content types in unified feeds/timelines |
| `vanilla-rails-testing` | Writing Rails tests |
| `vanilla-rails-jobs` | Writing background jobs |
| `vanilla-rails-naming` | Naming classes, methods, files |
| `vanilla-rails-hotwire` | Writing Turbo/Stimulus code |
| `vanilla-rails-work-breakdown` | Planning features or PRs |
| `vanilla-rails-style` | Ruby code style questions |
| `vanilla-rails-views` | Writing ERB templates, partials, Turbo Streams |
| `vanilla-rails-writing-affordances` | Extracting PORO wrappers for related operations |
| `shaping` | Applying patterns to features (via `/shape`) |

Skills auto-invoke based on context. No manual activation needed.

## Reference Implementations

The `reference/` directory contains detailed implementations:
- `authentication.md` - Passwordless magic link auth
- `multi-tenancy.md` - URL path-based multi-tenancy
- `events-audit-trail.md` - Polymorphic event system

These are for deep dives, not auto-loaded by Claude.

## AGENTS.md Template

Use `templates/AGENTS.md` as a starting point for your Rails projects. Copy it to your project root and customize.

## Key Patterns

### Resource Extraction
```ruby
# Instead of custom actions
resources :cards { post :close }

# Extract a resource
resources :cards { resource :closure }
```

### State as Records
```ruby
# Instead of boolean columns
class Card < ApplicationRecord
  # closed: boolean
end

# State records with who/when
class Card < ApplicationRecord
  has_one :closure
end
```

### Concerns as Adjectives
```ruby
class Card < ApplicationRecord
  include Closeable    # can be closed
  include Assignable   # can be assigned
  include Taggable     # can be tagged
end
```

### Job Pattern
```ruby
# Model
def notify_later
  NotifyJob.perform_later(self)
end

def notify_now
  # actual logic
end

# Job - just a wrapper
class NotifyJob < ApplicationJob
  def perform(record)
    record.notify_now
  end
end
```

### Data Modeling
```ruby
# All tables use UUIDs
create_table :cards, id: :uuid do |t|
  t.uuid :account_id, null: false  # Multi-tenancy
  # ...
end

# No foreign key constraints (app-level integrity)
# State as separate tables, not booleans
# Lead indexes with account_id
```

### Delegated Types (Recording/Recordable)
```ruby
# For 5+ content types in unified timelines
class Recording < ApplicationRecord
  delegated_type :recordable, types: %w[Message Document Upload Comment]
end

# Query the container, not individual types
project.recordings.includes(:recordable).chronologically
```

## Philosophy

> "Vanilla Rails is plenty"

These patterns prove you don't need:
- Service objects for simple operations
- Factories (fixtures are faster and clearer)
- Custom controller actions (extract resources)
- Complex gems for common patterns

Trust the framework. Keep it simple.

## Contributing

Issues and PRs welcome at https://github.com/zemptime/vanilla-rails

## License

MIT
