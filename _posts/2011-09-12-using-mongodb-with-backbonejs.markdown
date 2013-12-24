---
layout: post
title: "Using MongoDB With BackboneJS"
date: 2011-09-12 22:28
excerpt: How to use MongoDB with BackboneJS.
comments: true
---

So I ran into a small issue while working on a Rails 3.1 app which uses MongoDB as a backend and BackboneJS for bits and pieces of interactive ui. Changes in the _Collections_, i.e. addition or removal of _Model_ objects to the _Collection_ were not triggering appropriately binded _View_ functions.

<!--more-->
In MongoDB to identify a document uniquely we have "_id" attribute, which is similar to "id" column in relational database, which identifies a row uniquely.

And since by default, BackboneJS looks for "id" attribute to uniquely identify a _Model_ object. It wasn't able to know if a unique _Model_ object had been added or removed from the _Collection_. The fix to the problem turns out to be pretty simple and is clearly mentioned in the [comments](http://documentcloud.github.com/backbone/docs/backbone.html#section-22) of the BackboneJS library itself.

All you have to do is to explicitly set the idAttribute to "_id" in BackboneJS Model.
{% highlight javascript %}
 idAttribute : '_id'
{% endhighlight %}

And everything should work fine after that as far as MongoDB with BackboneJS is concerned.
