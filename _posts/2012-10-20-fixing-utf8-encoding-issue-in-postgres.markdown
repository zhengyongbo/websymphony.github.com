---
layout: post
title: "Fixing utf8 encoding issue in Postgres"
date: 2012-10-20 12:05
excerpt: In this post, I setup Postgres for rails on an Ubuntu machine. And fix utf8 encoding issue.
comments: true
---

I was trying to setup Postgres for rails on an Ubuntu machine. And on running

{% highlight ruby %}
rake db:create
{% endhighlight %}

I ran into the following error:

{% highlight sql %}
PGError: ERROR:  new encoding (UTF8) is incompatible with the encoding of the template database (SQL_ASCII)
HINT:  Use the same encoding as in the template database, or use template0 as template.
: CREATE DATABASE "app_development" ENCODING = 'unicode'
{% endhighlight %}

<!--more-->

The database.yml file before:
{% highlight yaml %}
development:
  adapter: postgresql
  encoding: unicode
  database: app_development
  pool: 5
  username: amit
  password:
  template: template0
{% endhighlight %}

Where it will use template0 from the database.yml and will ask me to change the default encoding to something else.

The solution turned out to be changing the default Postgres `template1` to use `utf8 encoding` by default. By following the following gist:

{% gist 3923995 %}

And changing database.yml to use template1 instead.

The database.yml file after:
{% highlight yaml %}
development:
  adapter: postgresql
  encoding: unicode
  database: app_development
  pool: 5
  username: amit
  password:
  template: template1
{% endhighlight %}

I hope this helps someone who runs into the same issue as I did.