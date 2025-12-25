# [Project Name]

This file provides guidance to AI coding agents working with this repository.

## What is [Project Name]?

[Brief description - what it does, who uses it, primary purpose]

## Development Commands

### Setup and Server
```bash
bin/setup              # Initial setup
bin/dev                # Start development server
```

Development URL: http://localhost:3000
[Add login instructions if applicable]

### Testing
```bash
bin/rails test                         # Run unit tests
bin/rails test test/path/file_test.rb  # Run single test file
bin/rails test:system                  # Run system tests
bin/ci                                 # Run full CI suite
```

### Database
```bash
bin/rails db:fixtures:load   # Load fixture data
bin/rails db:migrate          # Run migrations
```

## Architecture Overview

### Authentication & Authorization
[Describe how authentication works]
[Describe authorization approach]

### Core Domain Models

[List primary models and relationships]

**[Model]** → [Description]
- Key associations
- Important behaviors

## Coding Style

We follow vanilla Rails conventions. The `vanilla-rails` plugin provides detailed guidance:
- Controllers: CRUD-only, resource extraction
- Models: Concerns, state-as-records
- Testing: Fixtures, integration tests
- Jobs: Thin wrappers, `_later`/`_now` pattern
- Hotwire: Turbo Streams, focused Stimulus controllers

## Key Principles

### 1. CRUD Controllers Only
```ruby
# Bad
resources :tasks { post :complete }

# Good
resources :tasks { resource :completion }
```

### 2. Rich Models, Thin Controllers
```ruby
# Controller
def create
  @task.complete
end

# Model
def complete
  update!(completed_at: Time.current)
  notify_assignees
end
```

### 3. Concerns as Adjectives
```ruby
# app/models/task/completable.rb
module Task::Completable
  def complete
    # ...
  end
end
```

### 4. Fixtures Over Factories
```yaml
# test/fixtures/tasks.yml
urgent_task:
  title: Fix production bug
  project: flagship
```

### 5. Small, Focused PRs
One clear purpose per PR. 2-5 files typical.

## Project-Specific Conventions

[Document conventions unique to this project]

## Common Gotchas

[Document common pitfalls]

---

**Customization:** Replace [bracketed] sections. Remove this line when done.
