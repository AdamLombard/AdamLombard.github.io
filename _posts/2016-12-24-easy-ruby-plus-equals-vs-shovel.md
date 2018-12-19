---
layout: post
title: "Easy Ruby: Plus-Equals (+=) vs. Shovel (<<)"
description: Understanding how plus-equals and shovel affect strings in Ruby
image: snow-house.jpg
opacity: 0.8
tags: [Ruby, Code]
categories: blog
---

The difference between plus-equals (`+=`) and shovel (`<<`) is a common source of confusion and frustration when first working with strings in Ruby.

If we say…

{% highlight ruby %}
santa = 'Saint'
claus = santa
santa += ' Nick'
{% endhighlight %}

…then `santa` will yield the string `Saint Nick`, while `claus` will yield the string `Saint`.

Makes sense, right? Changing `santa` doesn’t affect `claus`.

Now let’s ruin Christmas!

## Confusion at the North Pole

If we say…
{% highlight ruby %}
santa = 'Saint'
claus = santa
santa << ' Nick'
{% endhighlight %}

…then `santa` will yield the string `Saint Nick`, while `claus` will _also yield the string_ `Saint Nick`.

WTF, Santa?!

## Explanation

To understand how `+=` and `<<` affect strings, we need three simple mental models:

* Variables point to objects
* The assignment operator (`=`) affects variables
* The append operator (`<<`) affects objects

In Ruby, a variable is initialized the first time an object is assigned to it:

{% highlight ruby %}
santa = 'Saint'
{% endhighlight %}

The code above says, “Computer, we need you to store an object for us &mdash; the string ‘Saint’. We may need to use this object later, or even change it! Things could get crazy! But, no matter how it changes, we need to make sure we're always talking about the same object. So, let’s have a codeword for the address in memory where ‘Saint’ is stored. Let’s use the codeword `santa`. Go team!"

So, if we say…

{% highlight ruby %}
santa = 'Saint'
santa = 'Saint Nick'
{% endhighlight %}

…we aren't changing the `Saint` object by adding `Nick` to it. We’re storing a _different_ object at (most likely) a _different_ place in memory, and re-pointing the `santa` variable to this other memory address.

We aren’t changing the object, even if we concatenate and re-assign. It’s still just an assignment of a variable:

{% highlight ruby %}
santa = 'Saint'
santa = santa + ' Nick'
{% endhighlight %}

The same is true even if we use some fancy syntactic sugar (or, in Santa’s case, syntactic sugarplums) to concatenate and re-assign:

{% highlight ruby %}
santa = 'Saint'
santa += ' Nick'
{% endhighlight %}

The plus-equals operator (`+=`) doesn't change objects. It changes what variables point to, via concatenation and assignment.

But, what if we want the opposite &mdash;to keep the variable pointing to the same spot in memory, and yet change the string located there?

(Hint: the append operator (`<<`) is about to come through in the clutch and save Christmas!)

{% highlight ruby %}
santa = 'Saint'
santa << ' Nick'
{% endhighlight %}

Here we are saying “Computer, please create a string object ‘Saint’, store it at some address, and we’ll talk about that address later using the codeword `santa`." Then, using the `<<`, we are saying, “Computer, remember that object you're saving for us at the `santa` address? Please add ‘ Nick’ to the end of that object. Thanks, computer!”

The object changes, while the variable does not. The variable continues to point to the same address in memory.

To bring us full circle to our original, confusing example, if we say…

{% highlight ruby %}
santa = 'Saint'
claus = santa
santa << ' Nick'
{% endhighlight %}

…we will have…

* `santa` yielding `Saint Nick`, and
* `claus` _also_ yielding `Saint Nick`!

This is because the `<<` operator affects objects, while `+=` affects variables. We pointed `santa` to a string object, and then pointed `claus` to `santa`&mdash;i.e., to the same address in memory.

## A note for Ruby Newbies

Technically `<<` and `+=` are not operators. `<<` and `+` are methods (in Ruby, `santa << ' Nick'` is the same as `santa.<<(' Nick')`), while `+=` is a shortcut combining assignment and the concatenation method. I used the term "operator" loosely but intentionally, for the sake of my fellow Ruby Newbies trying to Google answers to this issue.

---

_This post first appeared on [Medium](https://medium.com/@AdamLombard/easy-ruby-plus-equals-vs-shovel-6f030875e366). It was edited for clarity on 2018.12.18._
