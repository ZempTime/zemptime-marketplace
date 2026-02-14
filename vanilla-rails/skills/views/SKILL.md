---
name: vanilla-rails-views
description: Use when writing ERB templates, partials, or view helpers - covers partial organization, optional locals, CSS class patterns, and collection rendering with caching
---

# Views & Templates

ERB conventions for vanilla Rails applications.

## Partial Organization

**Lowest common ancestor** — place partials at the highest shared directory:

```
app/views/cards/_card.html.erb          # Shared by cards/show and cards/index
app/views/application/_flash.html.erb   # Shared across controllers
app/views/cards/display/_compact.html.erb  # Display variants
```

Never create deeply nested partials only used in one place.

## Optional Locals

Use `local_assigns.fetch` with explicit defaults:

```erb
<% pinned = local_assigns.fetch(:pinned, false) %>
<% card = local_assigns.fetch(:card) %>  <%# raises if missing %>
```

Not `local_assigns[:x] || default` (silent nil, ambiguous intent).

## CSS Class Helper

Build classes with array + compact + join:

```erb
<div class="<%= [
  'card',
  ('card--pinned' if card.pinned?),
  ('card--closed' if card.closed?)
].compact.join(' ') %>">
```

For complex cases: `<div class="<%= token_list('card', 'card--pinned': card.pinned?) %>">`

## Collection Rendering

Always cache, always specify `as:`:

```erb
<%= render partial: 'cards/card', collection: @cards, as: :card, cached: true %>
```

## Turbo Streams in Views

Prepend/append with update for empty states:

```erb
<%= turbo_stream.before :cards, @card %>
<%= turbo_stream.update :cards_count, @cards.count %>
<%= turbo_stream.remove @card %>
```

**See vanilla-rails-hotwire for dom_id, morph, and Stimulus patterns.**

## Stimulus in Views

Layer controllers on existing elements, don't add wrapper divs:

```erb
<body data-controller="keyboard shortcuts dropdown">
```

## Quick Reference

| Pattern | Use |
|---------|-----|
| `local_assigns.fetch(:x, default)` | Optional locals |
| `[...].compact.join(' ')` | Conditional CSS classes |
| `cached: true, as: :item` | Collection rendering |
| `turbo_stream.before` + `.update` | Add + refresh related |

## Red Flags

| Wrong | Right |
|-------|-------|
| `local_assigns[:x] \|\| default` | `local_assigns.fetch(:x, default)` |
| Wrapper div for Stimulus | Add controller to existing element |
| `render @cards` without cache | `render collection:, cached: true` |
| Partial in deep nested path | Lowest common ancestor |
