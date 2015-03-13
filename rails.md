# Gems

## Grouping your dependencies

Place gems in their appropriate group environment. Bundler provides a `groups` feature
that allows you to seperate your gem dependencies into environments that
make the most sense.

```ruby
# bad
gem 'byebug'

# good in-line syntax
gem 'byebug', group: :development

# good block syntax
group :development do
  gem 'byebug'
end
```

## Useful gems

### General

* `active_model_serializers`: Brings convention over configuration to
  your JSON generation.
  ([source](https://github.com/rails-api/active_model_serializers))

* `carrierwave`: Proves a simple and extremely flexible way to upload files.
  ([source](https://github.com/carrierwaveuploader/carrierwave))

* `context_validations`: Context based validations for model instances.
  ([source](https://github.com/dockyard/ruby-context_validations))

* `destroyed_at`: Allows you to "destroy" an object without deleting the record or assocaited records.
  ([source](https://github.com/dockyard/ruby-destroyed_at))

* `easy_auth`: Simple drop in authentication for Rails 3.2+
  ([source](https://github.com/dockyard/ruby-easy_auth))

  * To be used in conjunction with other auth gems found
    [here](https://github.com/dockyard/ruby-easy_auth/wiki/Plugins).

* `friendly_id`: Addon to Ruby's Active Record that allows you to
  replace ids in your URLs with strings.
  ([source](https://github.com/norman/friendly_id))

* `geocoder`: Complete geocoding solution for Ruby.
  ([source](https://github.com/alexreisner/geocoder))

* `pages`: Simple dynamic yet static pages.
  ([source](https://github.com/dockyard/pages))

* `party_foul`: Rails exceptions automatically opened as issues on
  GitHub.
  ([source](https://github.com/dockyard/party_foul))

* `pg`: Ruby interface to the PostgreSQL RDBMS.
  ([source](https://github.com/ged/ruby-pg))

  By default, Rails uses the SQLite3 Database. To begin a Rails
  project with Postgres, run the following command:

  * `rails new myproject --database=postgresql`

* `postgres_ext`:  Adds missing native PostgreSQL data types to ActiveRecord and convenient
  querying extensions for ActiveRecord and Arel for Rails 4.x
  ([source](https://github.com/dockyard/postgres_ext))

* `postgres_ext-postgis`: Extends ActiveRecord's data type handling and
  query methods in both Arel and ActiveRecord.
  ([source](https://github.com/dockyard/postgres_ext-postgis))

* `postgres_ext-serializers`: Will take over anytime you try to serialize
  an ActiveRecord::Relation.
  ([source](https://github.com/dockyard/postgres_ext-serializers))

* `will_paginate`: Pagination library.
  ([source](https://github.com/mislav/will_paginate))

### Development

* `bullet`: Helps increase your application's performance by reducing
  the number of queries it makes.
  ([source](https://github.com/flyerhzm/bullet))

* `byebug`: Debugger.
  ([source](https://github.com/deivid-rodriguez/byebug))

* `quiet_assets`: Turns off the Rails asset pipeline log.
  ([source](https://github.com/evrone/quiet_assets))

* `thin`: Tiny, fast, and funny HTTP server.
  ([source](https://github.com/macournoyer/thin/))


### Testing

* `capybara`: Simulates how a real user would interact with your app.
  ([source](https://github.com/jnicklas/capybara))

* `capybara-email`: Easily teset ActionMailer and Mail messages in your
  Capybara integration tests.
  ([source](https://github.com/dockyard/capybara-email))

* `capybara-extensions`: Extends Capybara's finders and matchers with
  additional methods for interacting with tables, lists, list items, and
  many HTML5 elements
  ([source](https://github.com/dockyard/capybara-extensions))

* `database_cleaner`: Set of strategies for cleaning your database;
  ensures clean state during tests.
  ([source](https://github.com/DatabaseCleaner/database_cleaner))

* `m`: Test runner that can run tests by line number.
  ([source](https://github.com/qrush/m))

* `minitest-spec-rails`: Makes it easy to use the MiniTest::Spec DSL.
  ([source](https://github.com/metaskills/minitest-spec-rails))

* `valid_attribute`: Minimalist matcher for validation.
  ([source](https://github.com/bcardarella/valid_attribute))


### Production

* `fog`: Cloud services library.
  ([source](https://github.com/fog/fog))

* `unicorn`: Rack HTTP server for fast clients and Unix.
  ([source](https://github.com/defunkt/unicorn))
