# URL-Based Multi-Tenancy

Reference implementation for path-based multi-tenancy.

## Concept

URLs prefixed with account ID: `/1234567/boards/...`

Middleware moves account slug from `PATH_INFO` to `SCRIPT_NAME`, making Rails think app is "mounted" at that path.

## Benefits

- No subdomain setup needed
- Easy testing (set `script_name`)
- Standard Rails routing unchanged
- Single database with `account_id` foreign keys

## Middleware

```ruby
module AccountSlug
  PATTERN = /(\d{7,})/
  PATH_INFO_MATCH = /\A(\/#{PATTERN})/

  class Extractor
    def initialize(app)
      @app = app
    end

    def call(env)
      request = ActionDispatch::Request.new(env)

      if request.path_info =~ PATH_INFO_MATCH
        request.script_name = $1
        request.path_info = $'.empty? ? "/" : $'
        env["app.account_id"] = $2.to_i
      end

      if env["app.account_id"]
        account = Account.find_by(external_id: env["app.account_id"])
        Current.with_account(account) { @app.call(env) }
      else
        Current.without_account { @app.call(env) }
      end
    end
  end
end

Rails.application.config.middleware.insert_after(
  Rack::TempfileReaper, AccountSlug::Extractor
)
```

## Current Context

```ruby
class Current < ActiveSupport::CurrentAttributes
  attribute :session, :user, :identity, :account

  def with_account(value, &) = with(account: value, &)
  def without_account(&) = with(account: nil, &)
end
```

## Model Scoping

```ruby
class Card < ApplicationRecord
  belongs_to :account, default: -> { board.account }
end
```

## URL Generation

In mailers:

```ruby
class ApplicationMailer < ActionMailer::Base
  private
    def default_url_options
      if Current.account
        super.merge(script_name: Current.account.slug)
      else
        super
      end
    end
end
```

## Jobs

Serialize/restore account context:

```ruby
module AccountJobExtensions
  def initialize(...) = (super; @account = Current.account)
  def serialize = super.merge("account" => @account&.to_gid)
  def deserialize(data) = (super; @account = GlobalID::Locator.locate(data["account"]))
  def perform_now = @account ? Current.with_account(@account) { super } : super
end
```

## Testing

```ruby
# Unit tests
setup { Current.account = accounts("acme") }

# Integration tests
setup do
  integration_session.default_url_options[:script_name] = "/1234567"
end
```

## Key Points

- Never query across accounts
- Always scope through `Current.account`
- Jobs auto-restore context
- Use `script_name: nil` for untenanted paths
