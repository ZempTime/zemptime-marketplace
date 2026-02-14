---
name: vanilla-rails-naming
description: Use when naming classes, methods, routes in vanilla Rails codebases - enforces concern adjectives (-able/-ible only), no bangs for state, resource naming cascade, and commit message format
---

# Vanilla Rails Naming

37signals naming conventions from production Basecamp codebases.

## The Naming Cascade

Every state change flows through this cascade:

| Layer | Pattern | Example |
|-------|---------|---------|
| Concern | Adjective (-able/-ible) | `Card::Closeable` |
| State model | Noun | `Closure` |
| Controller | Plural noun | `ClosuresController` |
| Route | Singular resource | `resource :closure` |
| Method | Plain verb (no !) | `card.close` |
| Async enqueue | `*_later` | `notify_watchers_later` |
| Sync execute | `*_now` or plain | `notify_watchers_now` |

**If the cascade doesn't flow, the naming is wrong.**

## Concern Names: -able/-ible ONLY

```ruby
# Wrong
Card::Closing        # verb
Card::PinManager     # Manager/Handler/Service
Card::Closed         # past participle

# Right
Card::Closeable
Card::Pinnable
Card::Assignable
```

**If concern name doesn't end in -able/-ible, STOP and rename immediately.** No -ing, -er, -ish, Logic, Management, Handler, Service.

## State Methods: No Bangs

Plain verbs for state changes. `!` only when non-bang counterpart exists.

```ruby
# Wrong                    # Right
card.close!                card.close
card.pin!                  card.pin_by(user)
card.archive!              card.archive
```

## Async: _later/_now

`_later` always at the end of compound verbs:

```ruby
notify_watchers_later      # enqueues job
notify_watchers_now        # actual logic (called by job)
pin_and_notify_later       # compound verb
```

## File Locations

```
app/models/card/closeable.rb           # Model-specific concern
app/models/concerns/eventable.rb       # Shared concern
app/models/closure.rb                  # State record
app/controllers/cards/closures_controller.rb
```

## Commit Messages

Present tense, lowercase, no type prefixes:

```
add pin feature to cards
extract closeable concern
fix closure validation
```

Not: `feat:`, `fix:`, `[FEATURE]`, past tense, title case.

## Red Flags

| Red flag | Fix |
|----------|-----|
| Concern not -able/-ible | Rename immediately |
| State method with `!` | Remove bang |
| `post :close` in routes | `resource :closure` |
| Async without `_later` | Add suffix |
| `ClosureController` (singular) | `ClosuresController` (plural) |
| Commit with `feat:` prefix | Present tense, no prefix |
