--- 
wordpress_id: 276
layout: post
title: Javascript's new Operator
date: 2011-01-23 11:54:58 -07:00
wordpress_url: http://www.bwigg.com/?p=276
---

What is it and how can you use it to create new objects from constructor functions?

### What's a <em>constructor</em> function.

There's really no such thing as a <em>constructor</em> function. There are just functions, that's it, no different than any other function, except that you the programmer has made it so that it initializes a keyword called `this` with methods and properties. Here's the <em>constructor</em> function we will use for the rest of this post.

{% highlight JavaScript %}
function Computer(manufacturer, model) {
  this.manufacturer = manufacturer;
  this.model = model;
}
{% endhighlight %}

### What does `new` do?

The new operator creates new objects using our constructor function. Let's start by seeing what happens when we DON'T use the `new` operator.


{% highlight JavaScript %}
var dell_inspiron = Computer('Dell', 'Inspiron');
alert(dell_inspiron); // undefined!</pre>
{% endhighlight %}

So what happened. Our Computer <em>constructor</em> function ran, it applied the `manufacturer` and `model` properties to the `this` keyword, which should have then populated our `dell_inspiron` variable. So what the hell was `this` pointing to? Let's see what's happening inside our <em>constructor</em> function:

{% highlight JavaScript linenum %}
function Computer(manufacturer, model) {
  alert(this);
  this.manufacturer = manufacturer;
  this.model = model;
}
var dell_inspiron = Computer('Dell', 'Inspiron');
alert(dell_inspiron); // undefined
// notice we now have two additional global objects manufacturer and model... not good
alert(manufacturer); // Dell
alert(model); // Inspiron</pre>
So as you can see calling a <em>constructor</em> function without using the `new` operator, causes our `this` keyword to point to the global object. Now let's try using our <em>constructor</em> function with the `new` operator.
<pre lang="javascript">var mac_book_pro = new Computer('Apple', 'Mac Book Pro');
alert(mac_book_pro); // [object OBJECT]
alert(mac_book_pro.manufacturer + ' ' + mac_book_pro.model); // Apple Mac Book Pro</pre>
{% endhighlight %}


This works as expected, and here's how. Using the `new` operator first creates a new blank object. Then, it invokes our <em>constructor</em> function, passing the newly created blank object as the `this` keyword. Your <em>constructor</em> function then populated the object pointed to by the `this` keyword with the `manufacturer` and `model` properties. When the <em>constructor</em> function ends, it automagically returns the object referenced by the `this` keyword to the `mac_book_pro` variable.
### Additional Resources

Here's the example code in jsfiddle you can play with.

[JSFiddle Example](http://jsfiddle.net/bawigga/aQb5B/10/)

If you want to read more about how the `new` operator works, I highly recommend the following books:

- [Professional JavaScript for Web Developers (Wrox Programmer to Programmer)](http://www.amazon.com/gp/product/047022780X?ie=UTF8&amp;tag=brianwiggi-20&amp;linkCode=as2&amp;camp=1789&amp;creative=390957&amp;creativeASIN=047022780X) Chapter 18 - Advanced Functions
- [JavaScript: The Definitive Guide](http://www.amazon.com/gp/product/0596101996?ie=UTF8&amp;tag=brianwiggi-20&amp;linkCode=as2&amp;camp=1789&amp;creative=390957&amp;creativeASIN=0596101996) Chapter 9 - Classes, Constructors, and Prototypes
