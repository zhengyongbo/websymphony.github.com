---
layout: post
title: "How 'this' in javascript is similar to 'self' in ruby"
date: 2012-04-21 13:23
comments: true
excerpt: In this, I show similarities between 'this' in javacript and 'self' in ruby.
---

When you read or learn something new, you always try to find a correlation of new knowledge to something you already knew. So when I learned how 'self' changes in a ruby program, I couldn't help but notice similarity to 'this' in javascript. In both ruby and javascript, 'self' and 'this' refer to the current object. Both 'self' and 'this' are transient and change depending upon the context.

<!--more-->

Lets consider an example of 'self' in ruby first.

{% highlight ruby %}
class MyExample
 def say_hello
    puts self
 	puts "Hello there."
 end
end

#calling the function
my_example_obj = MyExample.new
my_example_obj.say_hello
{% endhighlight %}

Here we have defined a simple class MyExample, which has an instance method _say_hello_. Then we create an instance of the class and call _say_hello_ on it.
Upon invocation of instance method, 'self' is changed to _my_example_obj_ and therefore it prints signature of _my_example_obj_ followed by "Hello there."

{% highlight ruby %}
#OUTPUT:
#<MyExample:0x00000101082e68>
"Hello there."
{% endhighlight %}

***


Now lets see how an equivalent javascript handles 'this'.

{% highlight javascript %}
var MyExample = function(){
}

MyExample.prototype = {
	'say_hello' : function(){
		console.log(this);
		console.log("Hello there.");
	}
}

var my_example_obj = new MyExample();
my_example_obj.say_hello();
{% endhighlight %}

Here we have defined a simple class MyExample just like our ruby program and have defined _say_hello_ method which gives us insight about 'this'. And similar to 'self' in our ruby program, in javascript 'this' changes to the object, the method was called on and hence we get the following output.

{% highlight javascript %}
/*OUTPUT*/
MyExample /*the value of 'this'*/
"Hello there."
{% endhighlight %}

It is interesting to see how different languages solve certain problems. And due to fundamental similarities in concept, i.e. dynamic nature of javascript, ruby. They have similar concept of 'this' and 'self' in method invocation on objects.
