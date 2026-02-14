---
name: vanilla-rails-data-modeling
description: Use when designing database schema, writing migrations, or making data storage decisions - enforces UUIDs, account_id multi-tenancy, no foreign keys, and proper index patterns
---

# Vanilla Rails Data Modeling

Database schema conventions from production 37signals patterns.

**For state-as-records pattern details, see vanilla-rails-models.**

## UUID Primary Keys

All tables use UUIDs. No auto-incrementing integers.

```ruby
create_table :cards, id: :uuid do |t|
  t.uuid :account_id, null: false
  t.string :title
  t.timestamps
end
```

UUIDv7 (timestamp-ordered), base36 encoded as 25-character strings. No ID enumeration, merge-safe, no sequence contention.

## Multi-Tenancy via account_id

Every tenant-scoped table has `account_id`. No exceptions for user data.

**Tables WITHOUT account_id** (global/cross-tenant): `identities`, `sessions`, `magic_links`

Scope queries via `Current.account`:

```ruby
class ApplicationRecord < ActiveRecord::Base
  def self.default_scope
    where(account_id: Current.account.id) if Current.account
  end
end
```

Don't forget `account_id` on join tables.

## No Foreign Key Constraints

Use application-level integrity, not database constraints.

```ruby
# Bad
t.references :card, foreign_key: true

# Good
t.uuid :card_id, null: false
add_index :table, :card_id
```

Prevents deadlocks during bulk operations. Maintain integrity via `dependent: :destroy`.

## Index Strategy

| Pattern | Rule |
|---------|------|
| Composite indexes | Lead with `account_id` |
| Polymorphic | Always `[type, id]` |
| Binary state | `unique: true` on parent_id |
| Per-user state | `unique: [parent_id, user_id]` |
| Tenant uniqueness | `[:account_id, :field]` |

```ruby
add_index :cards, [:account_id, :status]
add_index :events, [:eventable_type, :eventable_id]
add_index :closures, :card_id, unique: true
add_index :pins, [:card_id, :user_id], unique: true
```

## Join Table Patterns

| Need | Pattern | Has ID? | Has account_id? |
|------|---------|---------|-----------------|
| Just link two things | HABTM (`id: false`) | No | No |
| Track when/who linked | `has_many :through` | Yes (`id: :uuid`) | Yes |

**HABTM naming:** `plural_plural` alphabetically (`boards_filters`)

**Through naming:** Singular noun (`taggings`, `assignments`)

```ruby
# HABTM - no metadata needed
create_table :boards_filters, id: false do |t|
  t.uuid :board_id, null: false
  t.uuid :filter_id, null: false
end

# Through - timestamps, account scoping
create_table :taggings, id: :uuid do |t|
  t.uuid :account_id, null: false
  t.uuid :card_id, null: false
  t.uuid :tag_id, null: false
  t.timestamps
end
```

## Polymorphic Associations

Use semantic names describing the relationship:

| Name | Meaning |
|------|---------|
| `eventable` | thing the event is about |
| `source` | where it came from |
| `container` | what holds it |
| `searchable` | what is searchable |
| `recordable` | what it's attached to |

## Counter Caches

Manual `increment!`/`decrement!`, not Rails `counter_cache:` option:

```ruby
after_create :increment_account_counter

private
  def increment_account_counter
    account.increment!(:cards_count)
  end
```

## Settings Tables

Polymorphic config with inheritance fallback:

```ruby
class Board < ApplicationRecord
  def auto_postpone_period
    entropy&.auto_postpone_period || account.auto_postpone_period
  end
end
```

## Migration Conventions

| Rule | Example |
|------|---------|
| Prefer `change` | `def change; add_column ...; end` |
| Explicit UUID refs | `t.uuid :card_id` not `t.references :card` |
| Large table indexes | `add_index :table, :col, algorithm: :concurrently` |
| `up/down` only when irreversible | `remove_column` in `up` |

## Quick Reference

| Decision | Pattern |
|----------|---------|
| Primary key | `id: :uuid` always |
| Tenant column | `account_id` on all tenant tables |
| Foreign keys | None — app-level integrity |
| Simple join | `id: false`, no account_id |
| Rich join | `id: :uuid`, with account_id |
| Polymorphic index | `[type, id]` compound |
| Query index | Lead with `account_id` |
| Counter cache | Manual `increment!` |

## Sharding (Advanced)

For large tables, shard by account using CRC32:

```ruby
def shard_for(account_id)
  Zlib.crc32(account_id.to_s) % 16
end
```

16 identical tables, MySQL native fulltext across shards.
