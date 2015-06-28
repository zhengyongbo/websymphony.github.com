---
layout: post
title: "How to use UUIDs as Primary Key with ecto"
date: 2015-06-28 13:23
comments: true
excerpt: In this, I explain how to use UUIDs as primary key instead of IDs when using ecto with Phoenix Framework.
---

Recently, I was playing around with [Phoenix Framework (version: _0.13.1_)](http://phoenixframework.org) with Postgres as the database. Instead of using IDs for primary key, I wanted to use UUIDs.
Since Phoenix and associated projects haven't hit 1.0 yet, api is still under flux.

As of this post, there are two resources with instructions on how to achive this and both didn't seem to work. A [stackoverflow post](http://stackoverflow.com/questions/30004008/setting-up-phoenix-framework-and-ecto-to-use-uuids-how-to-insert-the-generated) and a [google group discussion](https://groups.google.com/forum/#!topic/phoenix-talk/OZaL2nSWUTE).

Since ecto version _0.12.1_ using UUIDs as primary key coudn't be any easier.
Here is how to do it:

<!--more-->

The migration that creates the table will need to be aware of the fact that ID column is going to be of UUID type.

{% highlight elixir %}
defmodule Blog.Repo.Migrations.CreatePost do
  use Ecto.Migration

  def up do

    # Here we are specifying to not use default auto-incrementing id as primary key
    create table(:posts, primary_key: false) do
      # Here we explicitly set the type of id column as uuid and assign it as primary key
      add :id, :uuid, primary_key: true
      add :title, :string
      add :body, :text

      timestamps
    end

  end

  def down do
    drop table(:posts)
  end

end
{% endhighlight %}


We will also need to update our model definition so that it can auto-generate UUIDs for primary keys.

{% highlight elixir %}
defmodule Blog.Post do
  use Blog.Web, :model

  # :binary_id is managed by drivers/adapters, it will be UUID for mysql, postgres
  #  but can be ObjectID if later you decide to use mongo
  @primary_key {:id, :binary_id, autogenerate: true}

  schema "posts" do
    field :title, :string
    field :body, :string

    timestamps
  end

  @required_fields ~w(title body)
  @optional_fields ~w()


  @doc """
  Creates a changeset based on the `model` and `params`.

  If `params` are nil, an invalid changeset is returned
  with no validation performed.
  """
  def changeset(model, params \\ :empty) do
    model
    |> cast(params, @required_fields, @optional_fields)
  end
end
{% endhighlight %}

And that is all you need to use UUIDs from primary keys with ecto and phoenix framework.

As an aside, I really like how these projects are shaping up. Also, the community around Elixir and Phoenix is absolutely fantastic. It took me no more than 5 minutes to figure this out by hopping on Elixir IRC and José Valim himself explaining how to solve this problem.