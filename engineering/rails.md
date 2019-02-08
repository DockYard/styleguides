# Rails Style Conventions

## Table of Contents
1.  [Routing](#routing)
1.  [Controllers](#controllers)
1.  [Models](#models)
    1. [Naming Conventions](#naming-conventions)
    1. [ActiveRecord](#activerecord)
    1. [ActiveRecord Queries](#activerecord-queries)
1.  [Migrations](#migrations)
1.  [Views](#views)
1.  [Testing](#testing)
1.  [Gems](#gems)

## Routing

* You can use `resources` to create routes for the seven default actions (index,
  show, new, create, edit, update, and destroy).

```ruby
resources :subscriptions
```

* However, blindly using `resources` can be seen as a leaky abstraction. Instead, you should use the `:only` and `:except` options to
fine-tune this behavior.

```ruby
resources :photos, only: [:index, :show]

resources :photos, except: :destroy
```

*  When you need to add more actions to a RESTful resource, use `member` and `collection` routes.

```ruby
# bad
get 'subscriptions/:id/unsubscribe'
resources :subscriptions

# good
resources :subscriptions do
  get 'unsubscribe', on: :member
end
```

* Nest route declarations under existing resources.

```ruby
# bad
get 'photos/search'
resources :photos

# good
resources :photos do
  get 'search', on: :collection
end
```

* If you need to define multiple `member/collection` routes use the
  alternative block syntax.

```ruby
resources :subscriptions do
  member do
    get 'unsubscribe'
    # more routes
  end
end

resources :photos do
  collection do
    get 'search'
    # more routes
  end
end
```

* Use namespaced routes to group related actions and API versioning.

```ruby
namespace :admin do
  resources :products
end

namespace :api do
  namespace :v1 do
    resources :users
  end
end
```

## Controllers

*  Keep the controllers light. They shouldn't contain any business logic. All the business
   logic should reside in an external class.

*  Use controller/context based validations rather than lumping them into the model (See the
   [`context_validations`](https://github.com/dockyard/ruby-context_validations) gem in the [gems](#gems) section).

## Models

### ActiveRecord

* Avoid altering ActiveRecord defaults (table names, primary key, etc)
  unless you have a very good reason.

* Group macro-style methods (`has_many`, `default_scope`, etc) in the beginning
   of the class definition.

```ruby
class User < ActiveRecord::Base
  # keep the default scope first (if any)
  default_scope { where(active: true) }

  # constants come up next
  COLORS = %w(red green blue)

  # followed by association macros
  belongs_to :country
  has_many :authentications, dependent: :destroy

  ...
end
```

* Refer to [7 Patterns to Refactor Fat
  Models](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/) for more helpful model guidelines.

### ActiveRecord Queries

* Favor the use of `find` over `where` when you need to retrieve a single
record by id.

```ruby
# bad
User.where(id: id).take

# good
User.find(id)
```

* Favor the use of `find_by` over `where` when you need to retrieve a single
record by some attributes.

```ruby
# bad
User.where(first_name: 'Bruce', last_name: 'Lee').first

# good
User.find_by(first_name: 'Bruce', last_name: 'Lee')
```

* Use `find_each` when you need to process a lot of records.

```ruby
# bad - loads all the records at once
User.all.each do |user|
  NewsMailer.weekly(user).deliver_now
end

# good - records are retrieved in batches
User.find_each do |user|
  NewsMailer.weekly(user).deliver_now
end
```

* Favor the use of `where.not` over SQL.

```ruby
# bad
User.where("id != ?", id)

# good
User.where.not(id: id)
```

* Avoid N+1 queries. For example, given the relationship below:

```ruby
# blog model
class Post < ActiveRecord::Base
  belongs_to :author
end

# authors model
class Author < ActiveRecord::Base
  has_many :posts
end
```

The first line of code below will send 6 (5+1) queries to the database, 1 to fetch the
5 recent posts and then 5 for their corresponding authors. Instead, use
the `includes` method to ensure all the associated data is loaded with
the minimum number of queries.

```ruby
# bad

#controller
@recent_posts = Post.order(published_at: :desc).limit(5)

# good

# controller
@recent_posts = Post.order(published_at: :desc).includes(:authors).limit(5)
```

* Use [Bullet](https://github.com/flyerhzm/bullet), which will watch your queries while you develop your application and
  notify you when you should add eager loading (N+1 queries).

## Migrations

* Set default values in the migrations themselves instead of in the
application layer.

```ruby
# bad
def price
  self[:price] or 0
end

# good
class CreateProducts < ActiveRecord::Migration
  def change
    create_table :products do |t|
      t.string :name
      t.integer :price, default: 0
    end
  end
end
```

* Migrations should be reversible when possible.

* When the logic is simple, use the `change` method. When using `change`,
  Active Record can automatically figure out how to reverse
  your migration.

```ruby
class AddNameToPeople < ActiveRecord::Migration
  def change
    add_column :people, :name, :string
  end
end
```

* However, if you want to do something more complex that Active Record
  can't automatically reverse, you should use the `up` and `down`
  methods.

```ruby
def up
 remove_column :products, :tax_percent
end

def down
 add_column :products, :tax_percent, :decimal, null: false, :precision => 6, :scale => 4
end
```

## Views

* If in need of a view layer, use ERB templating over HAML.

* Use double quotes when dealing with HTML or string interpolation.

* Never make complex formatting in the views, export the formatting to a
method in the view helper or the model.

* Mitigate code duplication by using partial templates and layouts.

```ruby
# index.html.erb
<h1>Products</h1>
<%= render partial: "product", collection: @products %>
```

```ruby
# _product.html.erb
<p>Product Name: <%= product.name %></p>
```

## Testing

* Use `MiniTest::Spec` instead of `RSpec`.

* Unit test controllers for validation/context specific behavior. Unit
  test model methods. And do not test views.

* Rely on unit tests to test the permutations of sad paths rather than
  pushing that complexity to integration tests.

* Test happy paths and one general sad path in integration tests.

* Use `must_match_schema` and `must_match_payload` gems for API integration tests.

```ruby
describe '#index' do
  context 'current user\'s addresses' do
    it 'returns the correct JSON format' do
      @response.must_match_schema({ addresses: [:id, :street,
:city] })
    end

    it 'returns the correct payload' do
      @response.must_match_payload({
        addresses: [ id: 1, street: 'Milk St', city: 'Boston']
      })
    end
  end
```

* Make heavy use of `describe` and `context`. Name the describe blocks in unit tests as follows:

  * use "description" for non-methods
  * use pound "#method" for instance methods
  * use dot ".method" for class methods

```ruby
# model
class Article
  def summary
    #...
  end

  def self.latest
    #...
  end
end

# test
describe Article do
  describe '#summary' do
    #...
  end

  describe '.latest' do
    #...
  end
end
```

## Gems

### Grouping your dependencies

If applicable, place gems in their appropriate group environment; otherwise,
it is OK to keep in global scope. Bundler provides a `groups` feature that
allows you to separate your gem dependencies into environments that make
the most sense.

```ruby
# bad (should be global)
group :development, :test, :production, :staging
  gem 'pg'
  gem 'simple_form'
end

# bad (byebug is for dev only)
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

* [`active_model_serializers`](https://github.com/rails-api/active_model_serializers): Brings
  convention over configuration to your JSON generation.

* [`carrierwave`](https://github.com/carrierwaveuploader/carrierwave): Proves a simple
  and extremely flexible way to upload files.

* [`context_validations`](https://github.com/dockyard/ruby-context_validations): Context
  based validations for model instances.

* [`destroyed_at`](https://github.com/dockyard/ruby-destroyed_at): Allows you to
  "destroy" an object without deleting the record or associated records.

* [`easy_auth`](https://github.com/dockyard/ruby-easy_auth): Simple drop in authentication
  for Rails 3.2+

  * To be used in conjunction with other auth gems found
    [here](https://github.com/dockyard/ruby-easy_auth/wiki/Plugins).

* [`friendly_id`](https://github.com/norman/friendly_id): Addon to Ruby's Active Record
  that allows you to replace ids in your URLs with strings.

* [`geocoder`](https://github.com/alexreisner/geocoder): Complete geocoding solution for Ruby.

* [`pages`](https://github.com/dockyard/pages): Simple dynamic yet static pages.

* [`party_foul`](https://github.com/dockyard/party_foul): Rails exceptions automatically
  opened as issues on GitHub.

* [`pg`](https://github.com/ged/ruby-pg): Ruby interface to the PostgreSQL RDBMS.

  By default, Rails uses the SQLite3 Database. To begin a Rails
  project with Postgres, run the following command:

  * `rails new myproject --database=postgresql`

* [`postgres_ext`](https://github.com/dockyard/postgres_ext): Adds missing native PostgreSQL data
  types to ActiveRecord and convenient querying extensions for ActiveRecord and Arel for Rails 4.x

* [`postgres_ext-postgis`](https://github.com/dockyard/postgres_ext-postgis): Extends
  ActiveRecord's data type handling and query methods in both Arel and ActiveRecord.

* [`will_paginate`](https://github.com/mislav/will_paginate): Pagination library.

### Development

* [`bullet`](https://github.com/flyerhzm/bullet): Helps increase your application's
  performance by reducing the number of queries it makes.

* [`byebug`](https://github.com/deivid-rodriguez/byebug): Debugger.

* [`quiet_assets`](https://github.com/evrone/quiet_assets): Turns off the Rails asset pipeline log.

* [`thin`](https://github.com/macournoyer/thin/): Tiny, fast, and funny HTTP server.

### Testing

* [`capybara`](https://github.com/jnicklas/capybara): Simulates how a real user would
  interact with your app.

* [`capybara-email`](https://github.com/dockyard/capybara-email): Easily test ActionMailer
  and Mail messages in your Capybara integration tests.

* [`capybara-extensions`](https://github.com/dockyard/capybara-extensions): Extends
  Capybara's finders and matchers with additional methods for interacting with
  tables, lists, list items, and many HTML5 elements

* [`database_cleaner`](https://github.com/DatabaseCleaner/database_cleaner): Set of strategies
  for cleaning your database; ensures clean state during tests.

* [`minitest-spec-rails`](https://github.com/metaskills/minitest-spec-rails): Makes it easy
  to use the MiniTest::Spec DSL.

* [`valid_attribute`](https://github.com/bcardarella/valid_attribute): Minimalist matcher for validation.

### Production

* [`fog`](https://github.com/fog/fog): Cloud services library.

* [`unicorn`](https://github.com/defunkt/unicorn): Rack HTTP server for fast clients and Unix.
