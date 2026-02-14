---
name: vanilla-rails-hotwire
description: Use when writing Hotwire (Turbo/Stimulus) code in Rails - enforces dom_id helpers, morph updates, focused Stimulus controllers, and JavaScript private methods
---

# Vanilla Rails Hotwire

37signals Hotwire conventions beyond the official docs.

## Turbo Streams

**Always `dom_id`, never string interpolation:**

```erb
<%# Wrong %>
<%= turbo_stream.replace "card_#{@card.id}" do %>

<%# Right %>
<%= turbo_stream.replace dom_id(@card) do %>
<%= turbo_stream.replace [ @card ] do %>
```

**Prefixed dom_id for granular updates:**

```ruby
dom_id(@card)                # "card_abc123"
dom_id(@card, :header)       # "header_card_abc123"
dom_id(@card, :status_badge) # "status_badge_card_abc123"
```

**Always `method: :morph` for replacements** (avoids layout shift, preserves scroll):

```erb
<%= turbo_stream.replace dom_id(@card, :status), method: :morph do %>
  <%= render "cards/status", card: @card %>
<% end %>
```

Morph for updates. `append`/`prepend` for new items. `remove` for deletions.

## Stimulus Controllers

**One purpose per controller.** Split large controllers.

**Private methods with `#` prefix** — only methods called from `data-action` are public:

```javascript
export default class extends Controller {
  #debounceTimer = null  // Private field

  copy() {  // Public - called from data-action
    navigator.clipboard.writeText(this.sourceTarget.value)
    this.#showNotification()
  }

  #showNotification() {  // Private - internal only
    this.element.classList.add('success')
  }
}
```

**Public methods:** Those in `data-action="controller#method"` + lifecycle (`connect`, `disconnect`, `*ValueChanged`, `*TargetConnected`)

**Private methods:** Everything else — helpers, callbacks, utilities. Add `#`.

**No business logic in Stimulus.** Controllers coordinate UI only. Validations and data transforms go in Rails.

## View Containers

Structure partials with prefixed dom_id for targeted updates:

```erb
<article id="<%= dom_id(card) %>" class="card">
  <div id="<%= dom_id(card, :status) %>">
    <%= render "cards/status", card: card %>
  </div>
  <div id="<%= dom_id(card, :header) %>">
    <%= render "cards/header", card: card %>
  </div>
</article>
```

## Red Flags

| Red flag | Fix |
|----------|-----|
| `"card_#{@card.id}"` | `dom_id(@card)` |
| `turbo_stream.replace` without `method: :morph` | Add `method: :morph` |
| Helper method without `#` | Add `#` prefix |
| One Stimulus controller doing 5+ things | Split into focused controllers |
| Validations in JavaScript | Move to Rails model |
| Animation logic in Stimulus | Use CSS transitions |
