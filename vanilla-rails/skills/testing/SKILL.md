---
name: vanilla-rails-testing
description: Use when writing Rails tests - enforces fixtures-only, integration-style controller tests, Current.session setup, simple naming, and Minitest syntax (NO FactoryBot, NO RSpec)
---

# Vanilla Rails Testing

**NO FACTORYBOT. NO RSPEC. NO DATA CREATION IN TESTS.**

## Banned → Required

| Never use | Use instead |
|-----------|-------------|
| `create(:model)`, `build(:model)`, FactoryBot | Fixtures: `cards(:logo)` |
| `let`, `describe`, `it`, `context`, `subject` | `test "name"`, `setup do` |
| `expect(...).to`, `.should` | `assert`, `assert_equal`, `assert_not` |
| `refute` | `assert_not` |
| `ActionController::TestCase` | `ActionDispatch::IntegrationTest` |
| `before_each` | `setup do` |
| RSpec mocks (`allow`, `expect`) | Mocha (`.stubs`) |

If any banned pattern appears, delete it and rewrite.

## Fixtures Only

```ruby
test "close card" do
  cards(:logo).close
  assert cards(:logo).closed?
end
```

Fixture missing? Add it to the fixtures file. Never create data in tests.

## Controller Tests = Integration Tests

```ruby
class Cards::ClosuresControllerTest < ActionDispatch::IntegrationTest
  setup do
    sign_in_as :kevin
  end

  test "create" do
    post card_closure_path(cards(:logo)), as: :turbo_stream
    assert_response :success
  end
end
```

## Model Tests Require Current.session

Always set `Current.session` in setup, even if you think it's not needed:

```ruby
class Card::CloseableTest < ActiveSupport::TestCase
  setup do
    Current.session = sessions(:david)
  end

  test "close records user" do
    cards(:logo).close(user: users(:kevin))
    assert_equal users(:kevin), cards(:logo).closed_by
  end
end
```

## Assertion Patterns

**State changes:** `assert_difference` with lambda syntax:

```ruby
assert_difference -> { Card.count }, +1 do
  post board_cards_path(boards(:writebook))
end

assert_difference({
  -> { cards(:logo).events.count } => +1,
  -> { Event.count } => +1
}) do
  cards(:logo).close(user: users(:kevin))
end
```

**Boolean toggles:** `assert_changes`:

```ruby
assert_changes -> { cards(:logo).reload.closed? }, from: false, to: true do
  post card_closure_path(cards(:logo)), as: :turbo_stream
end
```

**Exceptions:** `assert_raises`:

```ruby
assert_raises ActiveRecord::RecordNotFound do
  Card.find("nonexistent")
end
```

## Test Names

Concise. File + method name provide context.

```ruby
# Good
test "create"
test "close records user"

# Bad
test "should create a new card when given valid parameters"
```

## System Tests

Same rules — fixtures, no data creation:

```ruby
class SmokeTest < ApplicationSystemTestCase
  test "create a card" do
    sign_in_as(users(:david))
    visit board_url(boards(:writebook))
    click_on "Add a card"
  end
end
```

## Stubbing

Use mocha for stubs, webmock for HTTP:

```ruby
TestMailer.stubs(:goes_boom).raises(Net::SMTPSyntaxError)
stub_request(:post, webhook.url).to_return(status: 200)
```

## Red Flags

| Red flag | Action |
|----------|--------|
| `create(:model)` or `FactoryBot` | Delete. Use fixtures. |
| `describe`, `it`, `context` | Delete. Use `test`. |
| `expect(...).to` | Delete. Use `assert`. |
| Model test without `Current.session` | Add `Current.session = sessions(:fixture)` |
| Test names starting with "should" | Shorten. |
| Data creation in tests | Add fixture instead. |
