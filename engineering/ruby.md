# Ruby Style Guide

## Table of Contents
  1.  [Whitespace](#whitespace)
      * [Indentation](#indentation)
      * [Inline](#inline)
      * [Newlines](#newlines)
  1.  [Commenting](#commenting)
      * [Commented-out code](#commented-out-code)
  1.  [Methods](#methods)
      * [Method definitions](#method-definitions)
      * [Method calls](#method-calls)
  1.  [Conditional Expressions](#conditional-expressions)
      * [Conditional keywords](#conditional-keywords)
      * [Ternary operator](#ternary-operator)
  1.  [Syntax](#syntax)
  1.  [Naming](#naming)
  1.  [Classes](#classes)
  1.  [Exceptions](#exceptions)
  1.  [Collections](#collections)
  1. [Strings](#strings)
  1. [Be Consistent](#be-consistent)

## Whitespace

### Indentation

* Use soft-tabs set to 2 spaces.

```ruby
class Pokemon
∙∙def weakness
∙∙∙∙# Handles weakness
  end
end
```

* Indent the `public`, `protected`, and `private` methods as deep as the method
  definitions they apply to. Leave a blank line above and below the
  modifier to emphasize that it applies to all methods below it.

```ruby
class Pokemon
  def public_method
    # [...]
  end

  private

  def private_method
    #[...]
  end

  def another_private_method
    #[...]
  end
```

* Indent `when` as deep as `case`

* If the parameters of a method call span more than one line, align them with
  one level of indentation relative to the start of the line.

```ruby
def send_mail(source)
  Mailer.deliver(
    to: 'thor@asgard.com',
    from: 'pquill@guardians.net',
    subject: 'I am the captain of this ship!',
    body: source.text
  )
end
```

* In multi-boolean expressions, indent succeeding lines

```ruby
def process_job_contacts?
  is_in_progress? &&
    stage_type.email_ready? &&
    contacts_completed?
end
```

### Inline

* Never leave trailing whitespace.

* Use spaces around operators; after commas, colons, and semicolons; and around
  `{` and before `}`.

```ruby
sum = 1 + 1
a, b = 1, 2
a > sum ? '2 is now greater than 1' : 'You cannot math'
[1, 2, 3].each { |e| puts e }
```

* No spaces after `(`, `[` or before `]`, `)`.

```ruby
some(arg).other
[1, 2, 3].length
```

* Do not include a space before a comma.

* Do not include spaces inside block parameter pipes.

```ruby
# bad
ACCEPTED_TYPES.each_with_index do | accepted_type, i |

# good
ACCEPTED_TYPES.each_with_index do |accepted_type, i|
```

* Avoid whitespace during string interpolation.

```ruby
# bad
location = "#{ folder_name }/files/downloads"

# good
location = "#{folder_name}/files/downloads"
```

### Newlines

* Add a new line after conditionals, blocks, case statements, etc.

```ruby
if wall.needs_paint?
  paint_the_wall
end

wall.paint(:favorite_color)
```

* Add a new line between method definitions.

* Do not include new lines between areas of different indentation
  (such as around class or module bodies).

```ruby
# bad
class MyClass

  include ActiveModel::Model

# good
class MyClass
  include ActiveModel::Model
```

* Add a new line after a return statement.

```ruby
# bad
def my_method(user)
  return unless user.admin?
  do_something
end

# good
def my_method(user)
  return unless user.admin?

  do_something
end
```

## Commenting

> Though a pain to write, comments are absolutely vital to keeping our code
> readable. The following rules describe what you should comment and where. But
> remember: while comments are very important, the best code is
> self-documenting. Giving sensible names to types and variables is much better
> than using obscure names that you must then explain through comments.

[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)

* Write comments for your audience; other developers.

* Please use comment keywords such as `TODO:`, `NOTE:`, `OPTIMIZE:`,  etc.
  If applicable include links to relevant information (such as stack overflow
  links, blog links, links to PRs, etc).

* Don’t use block comments

* Never leave commented out code in the codebase.

## Methods

### Method definitions

* Use `def` with parentheses when there are arguments. Omit the
  parentheses when the method doesn't accept any arguments.

```ruby
def some_method
  # body omitted
end

def some_method_with_arguments(arg1, arg2)
  # body omitted
end
```

* Do not use default arguments. Use keyword arguments instead. Keyword arguments
  allow us to switch the order of the arguments, without affecting the
  behavior of the method
  * [Ruby 2.0 Keyword Arguments](https://thoughtbot.com/blog/ruby-2-keyword-arguments)

```ruby
# bad
def bake(pie, temp = 400, at = Time.now)
  # Bake stuff
end

# good
def bake(pie, temp: 400, at: Time.now)
  # Bake stuff
end
```

## Conditional Expressions

### Conditional keywords

* Never use `then` for multi-line `if/unless`.

```ruby
# bad
if some_condition then
  ...
end

# good
if some_condition
  ...
end
```

* The `and`, `or`, and `not` keywords are banned. They have a lower order of
  precedence and will cause unexpected side effects. Always use `&&`,  `||`, and
  `!` instead.

* Modifier `if/unless` usage is okay when the body is simple, the
  condition is simple, and the whole thing fits on one line. Otherwise,
  avoid modifier `if/unless`. `return if object.nil?` is ok.

* `unless` with `else` is confusing. Please use the positive case first.

```ruby
# bad
unless success?
  puts 'failure'
else
  puts 'success'
end

# good
if success?
  puts 'success'
else
  puts 'failure'
end
```

* Avoid `unless` with multiple conditions.

```ruby
# bad
unless foo? && bar?
  ...
end

# okay
if !(foo? && bar?)
  ...
end
```

* Don't use parentheses around the condition of an `if/unless/while`,
  unless the condition contains an assignment (see [Using the return
  value of `=`](#syntax) below).

```ruby
# bad
if (x > 10)
  ...
end

# good
if x > 10
  ...
end

# ok
if (x = self.next_value)
  ...
end
```

### Guard Clauses

* Prefer a guard clause when you can assert invalid data. A guard clause is a
  conditional statement at the top of a function that bails out as soon as
  it can.

```ruby
# bad
def method(object)
  if object.name.present?
    do_something
  end
end

# good
def method(object)
  return unless object.name.present?

  do_something
end
```

### Ternary operator

* Avoid the ternary operator (`?:`) except in cases where all expressions are
  extremely trivial. However, do use the ternary operator(`?:`) over
  `if/then/else/end` constructs for single line conditionals.

```ruby
# bad
result = if some_condition then something else something_else end

# good
result = some_condition ? something : something_else
```

* Use one expression per branch in a ternary operator. This
  also means that ternary operators must not be nested. Prefer
  `if/else` constructs in these cases.

```ruby
# bad
some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

# good
if some_condition
  nested_condition ? nested_something : nested_something_else
else
  something_else
end
```

* Avoid multi-line `?:` (the ternary operator), use `if/then/else/end` instead.

### Nested Conditionals

* Avoid using nested conditionals - instead, use a guard clause when you can
  assert valid data.

* Prefer `next` in loops instead of using a conditional block.

```ruby
# bad
heroes.each do |hero|
  if hero.superpower == 'Teleportation'
    puts 'Nightcrawler'
  end
end

# good
heroes.each do |hero|
  next unless hero.superpower == 'Teleportation'
  puts 'Nightcrawler'
end
```

## Syntax

* Never use `for`, unless you know exactly why. Most of the time iterators
  should be used instead. `for` is implemented in terms of `each` (so
  you're adding a level of indirection), but with a twist - `for`
  doesn't introduce a new scope (unlike `each`) and variables defined
  in its block will be visible outside it.

```ruby
arr = [1, 2, 3]

# bad
for elem in arr do
  puts elem
end

# good
arr.each { |elem| puts elem }
```

* Use `{...}` over `do...end` for single-line blocks.  Do not use
  `{...}` for multi-line blocks (multiline chaining is always
  ugly). Always use `do...end` for "control flow" and "method
  definitions" (e.g. in Rakefiles and certain DSLs).  Avoid `do...end`
  when chaining.

```ruby
names = ["Bozhidar", "Steve", "Sarah"]

# bad
names.each do |name|
  puts name
end

# good
names.each { |name| puts name }

# bad
names.select do |name|
  name.start_with?("S")
end.map { |name| name.upcase }

# good
names.select { |name| name.start_with?("S") }.map { |name| name.upcase }
```

* Utilize Ruby's implicit return, only use `return` statements when
  necessary (returning early from a function)

```ruby
# bad
def some_method(some_arr)
  return some_arr.size
end

# good
def some_method(some_arr)
  some_arr.size
end
```

* Use `||=` freely to initialize variables.

```ruby
# set name to Ash, only if it's nil or false
name ||= 'Ash'
```

* Don't use `||=` to initialize boolean variables. (Consider what
  would happen if the current value happened to be `false`.)

```ruby
# bad - would set enabled to true even if it was false
enabled ||= true

# good
enabled = true if enabled.nil?
```

* When a method block takes only one argument, and the body consists solely of
  reading an attribute or calling one method with no arguments, use the `&:`
  shorthand.

```ruby
# bad
bluths.map { |bluth| bluth.occupation }
bluths.select { |bluth| bluth.blue_self? }

# good
bluths.map(&:occupation)
bluths.select(&:blue_self?)
```
* Avoid `self` where not required

## Naming

* Use `snake_case` for methods and variables.

* Use `CamelCase` for classes and modules (Keep acronyms like HTTP,
  RFC, XML uppercase).

* Use `SCREAMING_SNAKE_CASE` for other constants.

* The names of predicate methods (methods that return a boolean value)
  should end in a question mark. (i.e. `Array#empty?`).

* The names of potentially "dangerous" methods (i.e. methods that modify `self`
  or the arguments, `exit!`, etc.) should end with an exclamation mark. Bang
  methods should only exist if a non-bang method exists
  ([More on this][ruby-naming-bang]).

## Classes

* Avoid the usage of class (`@@`) variables due to their "nasty" behavior
in inheritance.

```ruby
class Parent
  @@class_var = 'parent'

  def self.print_class_var
    puts @@class_var
  end
end

class Child < Parent
  @@class_var = 'child'
end

Parent.print_class_var # => will print "child"
```

As you can see all the classes in a class hierarchy actually share one
class variable. Class instance variables should usually be preferred
over class variables.

* Use `def self.method` to define singleton methods. This makes the methods
  more resistant to refactoring changes.

```ruby
class TestClass
  # bad
  def TestClass.some_method
    ...
  end

  # good
  def self.some_other_method
    ...
  end
```

## Exceptions

* Don't use exceptions for flow of control.

```ruby
# bad
begin
  n / d
rescue ZeroDivisionError
  puts "Cannot divide by 0!"
end

# good
if d.zero?
  puts "Cannot divide by 0!"
else
  n / d
end
```

* Do not rescuing the `Exception` class, be explicit in what you are
  rescuing from

```ruby
# bad
begin
  # an exception occurs here
rescue Exception
  # exception handling
end

# bad
begin
  # an exception occurs here
rescue
  # exception handling
end

# good
begin
  # an exception occurs here
rescue ActiveRecord::RecordNotFound
  # exception handling
end
```

## Collections

* Use `Set` instead of `Array` when dealing with unique elements. `Set`
  implements a collection of unordered values with no duplicates. This
  is a hybrid of `Array`'s intuitive inter-operation facilities and
  `Hash`'s fast lookup.

```Ruby
Set.new([1,1,2,3]) # => #<Set: {1, 2, 3}>
```

* Use symbols instead of strings as hash keys.

```ruby
# bad
hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

# good
hash = { one: 1, two: 2, three: 3 }
```

* Use multi-line hashes when it makes the code more readable, and use
  trailing commas to ensure that parameter changes don't cause
  extraneous diff lines when the logic has not otherwise changed.

```ruby
hash = {
  protocol: 'https',
  only_path: false,
  controller: :users,
  action: :set_password,
  redirect: @redirect_url,
  secret: @secret
}
```

## Strings

* Use single quotes, unless using string interpolation.

* Prefer string interpolation instead of string concatenation:

```ruby
# bad
email_with_name = user.name + ' <' + user.email + '>'

# good
email_with_name = "#{user.name} <#{user.email}>"
```

* Avoid using `String#+` when you need to construct large data chunks.
  Instead, use `String#<<`. Concatenation mutates the string instance in-place
  and is always faster than `String#+`, which creates a bunch of new string objects.

```ruby
# good and also fast
html = ''
html << '<h1>Page title</h1>'

paragraphs.each do |paragraph|
  html << "<p>#{paragraph}</p>"
end
```

## Be Consistent

> If you are editing code, take a few minutes to look at the code around you and
> determine its style. If they use spaces around their if clauses, you should,
> too. If their comments have little boxes of stars around them, make your
> comments have little boxes of stars around them too.
>
> The point of having style guidelines is to have a common vocabulary of coding so
> people can concentrate on what you are saying, rather than on how you are saying
> it. We present global style rules here so people know the vocabulary. But local
> style is also important. If code you add to a file looks drastically different
> from the existing code around it, the discontinuity throws readers out of their
> rhythm when they go to read it. Try to avoid this.

[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)

Thanks to these kind folks!

[airbnb-ruby](https://github.com/airbnb/ruby)
[bbatsov-ruby](https://github.com/bbatsov/ruby-style-guide)
[github-ruby](https://github.com/styleguide/ruby)
