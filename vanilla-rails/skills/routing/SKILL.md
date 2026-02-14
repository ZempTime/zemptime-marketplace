---
name: vanilla-rails-routing
description: Use when defining Rails routes, organizing controller directories, or adding resources - enforces resource extraction, shallow nesting, resolve helpers, and module scoping
---

# Vanilla Rails Routing

**Routes mirror controller directory structure.** Every route points to a controller in the matching namespace.

## Resource Extraction (State as Resources)

State changes become singular nested resources, not custom actions:

```ruby
# Bad - custom actions
resources :cards do
  post :close
  post :archive
end

# Good - state as resource
resources :cards do
  resource :closure, only: [:create, :destroy]
  resource :archival, only: [:create, :destroy]
end
```

`resource` (singular) = one state per parent. `create` = enter state. `destroy` = leave state.

## Controller Directory Mapping

Route nesting mirrors `app/controllers/` directory:

```ruby
# routes.rb
resources :boards do
  resources :cards do
    resource :closure
  end
end

# Maps to:
# app/controllers/boards_controller.rb
# app/controllers/boards/cards_controller.rb
# app/controllers/boards/cards/closures_controller.rb
```

**Rule:** If a route is nested under `boards`, the controller lives in `boards/` directory.

## Shallow Nesting

Avoid deeply nested URLs. Use `shallow:` or flatten when resource has its own identity:

```ruby
# Deep (avoid for >2 levels)
/boards/1/cards/2/comments/3/reactions/4

# Shallow
resources :boards do
  resources :cards, shallow: true
end
# Produces:
# /boards/:board_id/cards     (index, new, create)
# /cards/:id                  (show, edit, update, destroy)
```

**Rule:** If you can look up the resource by its own ID, it doesn't need parent in URL.

## Module Scoping

Group related controllers under modules for admin/API/etc:

```ruby
namespace :admin do
  resources :accounts    # Admin::AccountsController
end

scope module: :api do
  resources :cards       # Api::CardsController (no /api prefix in URL)
end

scope "/api" do
  resources :cards       # CardsController (with /api prefix, no module)
end
```

| Method | URL prefix | Module prefix | Use when |
|--------|-----------|---------------|----------|
| `namespace` | Yes | Yes | Admin panel, API versioning |
| `scope module:` | No | Yes | Alternate controller, same URL |
| `scope path:` | Yes | No | URL prefix, same controller |

## Resolve Helpers

Use `resolve` to control polymorphic URL generation:

```ruby
# Without resolve: url_for(@card) might generate wrong path
resolve("Card") { |card| [card.board, card] }

# Now polymorphic_path(@card) generates /boards/:board_id/cards/:id
```

Use when polymorphic routing needs explicit parent context.

## Direct Routes

Name custom routes for URL helpers:

```ruby
direct(:homepage) { "/" }
direct(:dashboard) { |account| "/#{account.to_param}/dashboard" }

# Usage: homepage_url, dashboard_url(@account)
```

## Quick Reference

| Pattern | Use | Avoid |
|---------|-----|-------|
| `resource :closure` | Singular state resource | `post :close` |
| `resources :cards, shallow: true` | 2+ nesting levels | Deep nesting >3 levels |
| `namespace :admin` | Admin/API sections | Manual path + module |
| `resolve("Card")` | Polymorphic URL control | String interpolation in paths |
| Controller at `boards/cards_controller.rb` | Nested route under boards | Controller location mismatch |

## Red Flags

- Custom member/collection actions for state changes → extract resource
- Controller file not matching route nesting → move controller
- 3+ levels of URL nesting → use `shallow:` or flatten
- `url_for` generating wrong paths → add `resolve`
- Route with no corresponding controller directory → create directory
