---
layout: post
title: "How Logical AND and OR Operators Actually Work in Javascript."
date: 2011-11-02 21:44
excerpt: In this I explaing working of Logical AND and OR Operators in Javascript.
comments: true
---

Last night, I watched *[Douglas Crockford: The JavaScript Programming Language](http://www.youtube.com/watch?v=v2ifWcnQs6M "Douglas Crockford: The JavaScript Programming Language")* and learned something interesting about logical ***AND*** and ***OR*** operators in javascript. Thought, I should share it with everyone.

<!--more-->
Lets look at the logical ***AND*** operator first.

{% highlight javascript %}
 if(expr1 && expr2){ //do something }
{% endhighlight %}


As per my earlier understanding of this operator, the code snippet above will execute anything inside the ***if block*** if operands "expr1" and "expr2" are both *truthy*. Which is absolutely correct and that is exactly how it works. Only catch is, what actually happens behind the scenes.
According to Mozilla's javascript documentation for [logical operators](https://developer.mozilla.org/en/JavaScript/Guide/Expressions_and_Operators#Logical_Operators).

> (Logical AND) Returns expr1 if it can be converted to false; otherwise, returns expr2. Thus, when used with Boolean values, && returns true if both operands are true; otherwise, returns false.

So, in other words that means ***expr1 && expr2*** will return *expr2* if *expr1* is *truthy* otherwise *expr1* will be returned.

NOTE: It is not returning "true" or "false", instead it is returning either of the expressions themselves when used with non Boolean Values.

Okay cool, so with this information lets see what actually happened inside the ***if block***. So if *expr1* was *truthy*, *expr1 && expr2* returns *expr2*. Now, the ***if block*** gets executed only when *expr2* is also *truthy*.

Hence, in the end both expressions need to be *truthy* for the ***if block*** to get executed, which is what we initially started with.

Lets see how we can use this for refactoring a common scenario where we want to call a method on an object only upon the existance of object.

**Initial code:**

{% highlight javascript %}
	if(a){
	 return a.method();
	}else{
	 return a;
	}
{% endhighlight %}

**Refactored Code:**

{% highlight javascript %}
 return a && a.method();
{% endhighlight %}

Refactored code does exactly the same thing, but is more expressive and faster.

Lets look at logical ***OR*** now.


By definition:
>(Logical OR) Returns expr1 if it can be converted to true; otherwise, returns expr2. Thus, when used with Boolean values, || returns true if either operand is true; if both are false, returns false.

Or in other words, ***expr1 || expr2*** will return *expr1* if *expr1* is *truthy* otherwise *expr2*.

Again NOTE: It is not returning "true" or "false", instead it is returning either of the expressions themselves when used with non Boolean Values.

This can be used to define default values for variables in an expressive fashion.

{% highlight javascript %}
 var myVar = input || default;
{% endhighlight %}

Here *myVar* will be assigned *input* if it is truthy other wise *default*.

And that's it for now. If you have any questions or suggestions, feel free to hit me up on [twitter](http://twitter.com/websymphony) or leave a comment below and I'll do my best to help!
