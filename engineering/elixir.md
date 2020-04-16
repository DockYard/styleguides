# Elixir Style Guides

## Table of Contents
1.  [Whitespace](#whitespace)
1.  [Syntax](#syntax)
    1. [Functions](#functions)
    1. [Conditional Expressions](#conditional-expressions)
    1. [Pipelines](#pipelines)
    1. [Modules](#modules)
1.  [Naming](#naming)

## Whitespace

* Use two-space indentation and spaces around operators, after commas, colons and semicolons.

```elixir
# bad
answer=1-3

def square(a) when a>0,
  do: a*a

Enum.map(["cat","dog"], fn(value) -> IO.puts(value) end)

# good
answer = 1 - 3

def square(a) when a > 0,
  do: a * a

Enum.map(["cat", "dog"], fn(value) -> IO.puts(value) end)
```

* **Do not** put spaces around matched pairs like brackets, parentheses, etc.

```elixir
# bad
{ :ok, result } = { :ok, 13 }

[ a, b, [ c ] ] = [ 1, 2, [ 3 ] ]

%{ favorite: color } = %{ favorite: "red" }

def iterate( [ head | tail ] ), do: iterate(tail)

# good
{:ok, result} = {:ok, 13}

[a, b, [c]] = [1, 2, [3]]

%{favorite: color} = %{favorite: "red"}

def iterate([head | tail]), do: iterate(tail)
```

* Use empty lines between function `def`s...

```elixir
def mark_as_read(email) do
  # ...
end

def mark_as_unread(email) do
  # ...
end

def send, do: # ...
```

* ...but put function `def`s that match for the same function together.

```elixir
def sum(nil),
  do: {:error, "No Value"}
def sum([]), do: 0
def sum([head | tail]) do
  head + sum(tail)
end
```

## Syntax

### Functions

* Use parentheses when there are arguments in a function definition. **Do not** use parentheses when there
  are no arguments.

```elixir
# bad
def greet name message do
  ...
end

# bad
def respond() do
  ...
end

# good
def greet(name, message) do
  ...
end

# good
def respond do
  ...
end
```
...however, favor parenthesis for null arity function calls, when the function is in the local scope.


```elixir
# bad
result = operation

# good
result = operation()
```

* Favor `do:` for single line function definitions.

```elixir
# okay
def iterate([]) do
 :ok
end

# better
def iterate([]), do: :ok
```

* Favor `do:` on its own line for singline lines expressed in more than a single word

```elixir
# okay
def multiply(val), do: val * 20

# better
def multiply(val),
  do: val * 20
  
# bad
def traverse([]),
  do: []
  
# good
def traverse([]), do: []
```

* Use parentheses in function calls when passing parameters, especially inside a pipeline.

```elixir
# bad
convert_to_celsius 32

# REALLY bad
# parses as convert_to_celsius(32 |> format)
convert_to_celsius 32 |> format

# good
convert_to_celsius(32)

# good
convert_to_celsius(32) |> format
```

### Conditional Expressions

* Never use `do:` for multi-line `if/unless` statements. Instead, use
  the `do ... end` format.

```elixir
# bad
if price < 100 do:
  # multiple
  # lines
  # of code

# good
if price > 100 do
  # multiple
  # lines
  # of code
end
```

* Use `do:` for single line `if/unless` statements.

```elixir
if price == 100, do: # do stuff
```

* Use `true` or an atom as the last (catch-all) condition of a `cond` statement
(An atom evaluates to a truthy value). Suggested atoms are `:else`, `:default` or `:otherwise`.

```elixir
cond do
  1 + 1 == 4 ->
    "Close, but no"
  1 + 1 == 3 ->
    "Getting warmer"
  true ->
    "Sure"
end

cond do
  1 + 1 == 4 ->
    "Close, but no"
  1 + 1 == 3 ->
    "Getting warmer"
  :else ->
    "Sure"
end
```

* Prefer `with` ([read more about it here](https://gist.github.com/josevalim/8130b19eb62706e1ab37)) over using deeply nested `case` statements. In this example, `response` is a `HTTPoison` response struct:

```elixir
# bad
case response do
  {:ok, resp} ->
    case Map.fetch(resp, :body) do
      {:ok, body} ->
        case Poison.decode(body) do
          {:ok, body} -> {:ok, body}
          _ -> {:error}
        end
      :error -> {:error, "No body"}
    end
  _ -> {:error}
end

# good
with {:ok, %{body: body}} <- response,
     {:ok, body} <- Poison.decode(body),
     do: {:ok, body}
```

### Pipelines

* Use the pipeline operator, `|>`, to chain **multiple** functions together.

```elixir
# bad
some_string |> String.downcase

# good
String.downcase(some_string)
```

* *always* use parenthesis when piping, including the last expression.

```elixir
# bad
conn
|> assign(:foo, bar)
|> render "index.html"

# good
conn
|> assign(:foo, bar)
|> render("index.html")
```

* Use a single level of indentation for multi-line pipelines.

```elixir
# bad
some_string
  |> String.downcase
  |> String.strip

# good
some_string
|> String.downcase
|> String.strip
```

* Use **bare** variables in the first part of a function chain.

```elixir
# bad
String.strip(some_string)
|> String.downcase
|> String.codepoints

# good
some_string
|> String.strip
|> String.downcase
|> String.codepoints
```

* Always start a new line for each pipeline in a function chain.

```elixir
# bad
some_string |> String.strip |> String.downcase

# good
some_string
|> String.strip
|> String.downcase
|> String.codepoints
```

* When assigning the result of a pipeline to a variable, the `variable =` should stand on its own, with an indented pipeline below.

```elixir
# bad
result = some_string
|> String.strip
|> String.downcase
|> String.codepoints

# good
result =
  some_string
  |> String.strip
  |> String.downcase
  |> String.codepoints
```

### Modules

* Use one module per file unless the module is only used internally by
another module (such as a test).

* No newline after defmodule, starting with `@moduledoc`, followed by `use`'s, `import`'s, and `alias`'s

* Organize multiple `alias`'s and `import`'s alphabetically, with increased nesting.

```elixir
defmodule ArtEducation.Exam do
  @moduledoc """
  Short one line summary, ending in period.

  More details as necessary after newline.
  """

  use GenServer

  import Mogrify
  import SweetXml

  alias ArtEducation.Images
  alias ExAws.S3
  alias ExAws.S3.Upload

  def start_link do
    ...
  end
end
```

## Naming

* Use `snake_case` for atoms, functions and variables.

* Use `CamelCase` for modules.

* Use a question mark rather than a leading `is_` when naming a function
  that returns a boolean value.

```elixir
# bad
def is_acid?(ph) do
  # ph > 7
end

# good
def acid?(ph) do
  # ph < 7
end
```
