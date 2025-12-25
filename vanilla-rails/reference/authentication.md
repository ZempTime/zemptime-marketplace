# Passwordless Authentication

Reference implementation for magic link authentication.

## Architecture

**Identity** - Global user (by email)
- Owns sessions and magic links
- Can exist across multiple accounts

**User** - Account-specific membership
- Belongs to one Account and one Identity
- Has role (owner/admin/member)

**Session** - Authenticated session
- Belongs to Identity (not User)
- Signed cookie stores token

## Schema

```ruby
create_table "identities", id: :uuid do |t|
  t.string "email_address", null: false
  t.index ["email_address"], unique: true
end

create_table "magic_links", id: :uuid do |t|
  t.uuid "identity_id"
  t.string "code", null: false
  t.datetime "expires_at", null: false
  t.integer "purpose", null: false
  t.index ["code"], unique: true
end

create_table "sessions", id: :uuid do |t|
  t.uuid "identity_id", null: false
  t.string "user_agent"
  t.string "ip_address"
end
```

## Identity Model

```ruby
class Identity < ApplicationRecord
  has_many :magic_links, dependent: :destroy
  has_many :sessions, dependent: :destroy
  has_many :users, dependent: :nullify

  validates :email_address, format: { with: URI::MailTo::EMAIL_REGEXP }
  normalizes :email_address, with: ->(v) { v.strip.downcase.presence }

  def send_magic_link(**attrs)
    magic_links.create!(attrs).tap do |ml|
      MagicLinkMailer.sign_in(ml).deliver_later
    end
  end
end
```

## MagicLink Model

```ruby
class MagicLink < ApplicationRecord
  CODE_LENGTH = 6
  EXPIRATION = 15.minutes

  belongs_to :identity
  enum :purpose, %w[sign_in sign_up], prefix: :for

  scope :active, -> { where(expires_at: Time.current...) }

  before_validation :generate_code, :set_expiration, on: :create

  def self.consume(code)
    active.find_by(code: sanitize(code))&.tap(&:destroy)
  end

  private
    def generate_code
      self.code ||= SecureRandom.base32(CODE_LENGTH)
    end

    def set_expiration
      self.expires_at ||= EXPIRATION.from_now
    end
end
```

## Authentication Concern

```ruby
module Authentication
  extend ActiveSupport::Concern

  included do
    before_action :require_authentication
    helper_method :authenticated?
  end

  private
    def authenticated?
      Current.identity.present?
    end

    def require_authentication
      resume_session || request_authentication
    end

    def resume_session
      if session = Session.find_signed(cookies.signed[:session_token])
        Current.session = session
      end
    end

    def start_new_session_for(identity)
      identity.sessions.create!(
        user_agent: request.user_agent,
        ip_address: request.remote_ip
      ).tap do |s|
        Current.session = s
        cookies.signed.permanent[:session_token] = {
          value: s.signed_id, httponly: true, same_site: :lax
        }
      end
    end
end
```

## Flow

1. User enters email
2. `identity.send_magic_link` creates code, sends email
3. User enters 6-char code
4. Code consumed, session created
5. Signed token in permanent cookie
6. Subsequent requests: `resume_session` finds session

## Security

- Timing-safe comparison
- Email enumeration prevention (fake links)
- Code sanitization (O→0, I→1)
- Rate limiting
- 15-minute expiration
- Single-use codes
