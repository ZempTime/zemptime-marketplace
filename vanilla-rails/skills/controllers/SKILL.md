---
name: vanilla-rails-controllers
description: Use when writing Rails controllers or implementing state changes - enforces resource extraction, thin controllers delegating to models, params.expect, and controller concerns for scoping
---

# Vanilla Rails Controllers

**State changes are resources.** Every state change becomes its own resource controller with CRUD operations.

## Resource Extraction

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

**See vanilla-rails-routing for route structure, nesting, and directory mapping.**

## Common State Resources

| State Change | Resource | create = | destroy = |
|--------------|----------|----------|-----------|
| Close/Reopen | `closure` | close | reopen |
| Archive/Unarchive | `archival` | archive | unarchive |
| Pin/Unpin | `pin` | pin | unpin |
| Publish/Unpublish | `publication` | publish | unpublish |
| Assign/Unassign | `assignment` | assign | unassign |
| Follow/Unfollow | `subscription` | subscribe | unsubscribe |

## Thin Controllers, Rich Models

Controllers delegate to intention-revealing model methods. No business logic in controllers.

```ruby
# Bad - logic in controller
class Cards::ArchivalsController < ApplicationController
  def create
    @card.update(archived: true)
  end
end

# Good - delegate to model
class Cards::ArchivalsController < ApplicationController
  include CardScoped

  def create
    @card.archive
    respond_to do |format|
      format.turbo_stream
      format.json { head :no_content }
    end
  end

  def destroy
    @card.unarchive
    respond_to do |format|
      format.turbo_stream
      format.json { head :no_content }
    end
  end
end
```

## Controller Concerns (Scoping)

Extract parent-finding into concerns. Name describes what's scoped:

```ruby
# app/controllers/concerns/card_scoped.rb
module CardScoped
  extend ActiveSupport::Concern

  included do
    before_action :set_card
  end

  private
    def set_card
      @card = Card.find(params[:card_id])
    end
end

# app/controllers/concerns/board_scoped.rb
module BoardScoped
  extend ActiveSupport::Concern

  included do
    before_action :set_board
  end

  private
    def set_board
      @board = Current.account.boards.find(params[:board_id])
    end
end
```

**Common concerns:**

| Concern | Sets | Used by |
|---------|------|---------|
| `BoardScoped` | `@board` | All controllers under `boards/` |
| `CardScoped` | `@card` | All controllers under `cards/` |
| `Authenticated` | session check | All controllers needing auth |

## params.expect()

Use `params.expect()` instead of `params.require().permit()`:

```ruby
# Bad
def card_params
  params.require(:card).permit(:title, :description)
end

# Good
def card_params
  params.expect(card: [:title, :description])
end
```

## Migration Pattern

State resources need a table tracking who/when:

```ruby
create_table :closures, id: :uuid do |t|
  t.uuid :card_id, null: false
  t.uuid :user_id
  t.timestamps
end
add_index :closures, :card_id, unique: true
```

## Red Flags

| Red flag | Fix |
|----------|-----|
| `post :close`, `patch :activate` | Extract resource |
| Business logic in controller | Move to model |
| `params.require().permit()` | Use `params.expect()` |
| `before_action` duplicated across controllers | Extract scoping concern |
| Controller > 30 lines per action | Delegate more to model |

## Self-Check

- [ ] State changes modeled as resources (create/destroy)
- [ ] Controller actions delegate to model methods
- [ ] Parent-finding extracted to scoping concerns
- [ ] `params.expect()` for strong parameters
