---
name: vanilla-rails-style
description: Use when writing Ruby/Rails code - enforces expanded conditionals over guard clauses, private indentation, invocation-order methods, and bang method restrictions that contradict standard Ruby guides
---

# Vanilla Rails Style

**These conventions CONTRADICT standard Ruby style guides.** They reflect production 37signals code.

## Counter-Intuitive Patterns

| Pattern | 37signals Way | Standard Ruby Way |
|---------|---------------|-------------------|
| Conditionals | `if/else` | Guard clauses |
| Private indent | 2 spaces under `private` | No indentation |
| Bang methods | Only with counterpart | Flag "dangerous" actions |
| Method order | Invocation sequence | Alphabetical |

## Expanded Conditionals (NOT Guard Clauses)

**Prefer `if/else`.** Guard clauses allowed ONLY at method start with 5+ line body.

```ruby
# Good
def todos_for_new_group
  if ids = params.require(:todolist)[:todo_ids]
    @bucket.recordings.todos.find(ids.split(","))
  else
    []
  end
end

# Bad
def todos_for_new_group
  ids = params.require(:todolist)[:todo_ids]
  return [] unless ids
  @bucket.recordings.todos.find(ids.split(","))
end
```

**Multiple guards → nested if/else.** Convert `return unless` chains to nested conditionals that show complete logic flow.

**Allowed exception:**
```ruby
def after_recorded_as_commit(recording)
  return if recording.parent.was_created?  # Guard at START, complex body below

  if recording.was_created?
    broadcast_new_column(recording)
  else
    broadcast_column_change(recording)
  end
end
```

## Private Method Indentation

Indent 2 spaces under `private`. No newline after `private`.

```ruby
class SomeClass
  def public_method
    # ...
  end

  private
    def helper_one
      # indented
    end

    def helper_two
      # indented
    end
end
```

**Exception — module with ONLY private methods:** mark `private` at top, blank line, no indent.

## Method Ordering

Order by invocation sequence — callers before callees:

1. Class methods
2. `initialize` (first among instance methods)
3. Public methods
4. Private methods (ordered by call sequence)

```ruby
class Builder
  def build
    prepare
    format
    output
  end

  private
    def prepare; end   # called first
    def format; end    # called second
    def output; end    # called third
end
```

## Bang Methods

Only use `!` when non-bang counterpart exists: `save/save!`, `update/update!`.

No counterpart → no bang. `close` not `close!`. `archive` not `archive!`.

## Red Flags

| If you're about to... | Do this instead |
|-----------------------|-----------------|
| Add guard clauses | Use if/else |
| Remove private indentation | Indent under `private` |
| Add `!` to method without counterpart | Use plain name |
| Alphabetize private methods | Order by invocation |
| Create a service object | Move logic to model |

## Self-Check

- [ ] if/else instead of guard clauses (except single guard at start with complex body)
- [ ] Private methods indented under `private`
- [ ] Methods ordered by invocation sequence
- [ ] Bang only on methods with non-bang counterpart
