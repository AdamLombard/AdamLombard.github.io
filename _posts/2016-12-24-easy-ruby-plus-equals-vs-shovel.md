---
layout: post
title: "Easy Ruby: Plus-Equals (+=) vs. Shovel (<<)"
image: snow-house.jpg
opacity: 0.8
tags: [Ruby, Code]
---

_Understanding how plus-equals and shovel affect strings in Ruby_

## Confusion at the North Pole

The difference between plus-equals (`+=`) and shovel (`<<`) is a common source of confusion, frustration, and cursing when working with strings in Ruby.

If we say…

{% highlight ruby %}
santa = 'Saint'

claus = santa

santa += ' Nick'
{% endhighlight %}

… then `santa` will yield the string `Saint Nick`, while `claus` will yield the string `Saint`.

Makes sense, right? Changing santa doesn’t affect claus.

Now let’s ruin Christmas.

If we say…
{% highlight ruby %}
santa = 'Saint'

claus = santa

santa << ' Nick'
{% endhighlight %}

…then `santa` will yield the string `Saint Nick`, while `claus` will also yield the string `Saint Nick`.

WTF, Santa?!

## Simple Explanation

To understand how `+=` and `<<` affect strings, you need three simple mental models:

* Variables point to objects
* The `=` (assignment operator) affects variables
* The `<<` (append operator) affects objects

## Detailed Explanation

The expression `Saint` yields the following string object:

{% highlight text %}
Saint
{% endhighlight %}

But this object is largely useless to us, since we can’t reference it in a consistent way.

To do so, we must initialize a variable to point to it. We do this using the assignment operator, `=`. In Ruby, a variable is initialized the first time an object is assigned to it:

{% highlight ruby %}
santa = 'Saint'
{% endhighlight %}

The code above says, “Computer, I need you to store an object for me — the string ‘Saint’. I may need to use this object later, or even change it a little! Things could get crazy! But, no matter how it changes, I want to make sure you and I are always talking about the same object. So, let’s have a codeword for the address in memory where ‘Saint’ is stored. Let’s use the codeword santa. Go team!"

So, if we say…

{% highlight ruby %}
santa = 'Saint'

santa = 'Saint Nick'
{% endhighlight %}

… we are not changing the `Saint` object by adding ` Nick` to it. We’re storing a different object at a different address in memory, and re-pointing the santa variable to this other address.

We aren’t changing the object, even if we concatenate and re-assign. It’s still a re-assignment of the variable:

{% highlight ruby %}
santa = 'Saint'

santa = santa + ' Nick'
{% endhighlight %}

Which means we aren’t changing the object, even if we use some fancy syntactic sugar (or, in Santa’s case, syntactic sugarplums) to concatenate and re-assign :

{% highlight ruby %}
santa = 'Saint'

santa += ' Nick'
{% endhighlight %}

The object didn’t change. The variable changed.

But, what if we want the opposite — to keep the variable pointing to the same spot in memory, but change the string located there?

(Hint: the append operator (`<<`) is about to come through in the clutch and save Christmas.)

{% highlight ruby %}
santa = 'Saint'

santa << ' Nick'
{% endhighlight %}

Here we are saying “Computer, please create a string object ‘Saint’, store it at some address, and we’ll talk about that address later using the codeword santa." Then, using the `<<`, we are saying, “Computer, remember that object you are saving for me at the santa address? Please add ‘ Nick’ to the end of that object. Thanks, computer!”

The object changes, while the variable does not. The variable continues to point to the same address in memory.

This means something quite subtle when writing Ruby.

If we say…

{% highlight ruby %}
santa = 'Saint'

claus = santa

santa << ' Nick'
{% endhighlight %}

… we will have…

* `santa` yielding `Saint Nick`, and
* `claus` also yielding `Saint Nick`!

We pointed `santa` to a string object, and then pointed `claus` to `santa`. Since `santa` is just a reference to a memory address, `claus` is also a reference to the same address, and thus to the exact same object modified by the statement `santa << ‘ Nick’`.

To bring the concept full circle, if we say…

{% highlight ruby %}
santa = 'Saint'

claus = santa

santa += ' Nick'
{% endhighlight %}

… we will have…

* `santa` yielding `Saint Nick`, and
* `claus` still yielding just `Saint`, because although `santa` and `claus` both pointed to the same address when claus was initialized, the `+=` re-assigned santa to a different address.

The `<<` affects objects, while `+=` affects variables.


## A note for future Rubyists

Technically `<<` and `+=` are not operators. `<<` and `+` are methods (in Ruby, `santa << ' Nick'` is the same as `santa.<<(' Nick')`), while `+=` is a shortcut combining assignment and the concatenation method. I used the term operator loosely but intentionally, for the sake of my fellow Ruby newbies trying to Google answers to this issue.

---

(_This post first appeared on [Medium](https://medium.com/@AdamLombard/easy-ruby-plus-equals-vs-shovel-6f030875e366)_.)