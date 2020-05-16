# Phoenix Style Guides

## Table of Contents

1. [Controllers](#controllers)

## Controllers

### Abstract CRUD logic into  separate modules

```elixir
# bad

defmodule PhoenixApp.PostController do
  use PhoenixApp.Web, :controller

  alias PhoenixApp.Post

  def index(conn, %{"page" => page, "per_page" => per_page}) do
    posts =
      Post
      |> offset((page - 1) * per_page)
      |> limit(per_page)
      |> Repo.all()

    render(conn, "index.html", posts: posts)
  end
end

# good

defmodule PhoenixApp.PostController do
  use PhoenixApp.Web, :controller

  alias PhoenixApp.PostManager

  def index(conn, params) do
    posts = PostManager.list_posts(params)
    render(conn, "index.html", posts: posts)
  end
end
```

### Use `with` instead of nested `case`s

```elixir
# bad

defmodule PhoenixApp.PostController do
  use PhoenixApp.Web, :controller

  alias PhoenixApp.{Authorizer,PostManager}

  def show(conn, %{"id" => id}) do
    case PostManager.get_post(id) do
      {:ok, post} ->
        case Authorizer.authorize(post) do
          :ok ->
            render(conn, "show.html", post: post)
          {:error, :unauthorized} -> 
            redirect(conn, to: unauthorized_path(conn))
        end
      {:error, :not_found} ->
        redirect(conn, to: not_found_path(conn))
    end
  end
end

# good

defmodule PhoenixApp.PostController do
  use PhoenixApp.Web, :controller

  def show(conn, params) do
    with {:ok, post} <- PostManager.get_post(id),
         :ok <- Authorizer.authorize(post) do
      render(conn, "show.html", post: post)
    else
      {:error, :unauthorized} -> 
        redirect(conn, to: unauthorized_path(conn))
      {:error, :not_found} ->
        redirect(conn, to: not_found_path(conn))
    end
  end
end
```
