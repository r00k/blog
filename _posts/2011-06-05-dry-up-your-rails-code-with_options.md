---
layout: post
title: 'Let''s read some Rails code: with_options'
date: 2011-06-05 00:35:41.000000000 -04:00
categories:
- general
tags: []
status: publish
type: post
published: true
---
Did you know you can refactor this:

{% highlight ruby %}
# Duplicated :dependent => :destroy option.
class Account < ActiveRecord::Base
  has_many :customers, :dependent => :destroy
  has_many :products,  :dependent => :destroy
  has_many :invoices,  :dependent => :destroy
  has_many :expenses,  :dependent => :destroy
end
{% endhighlight %}

into this?

{% highlight ruby %}
# Better, perhaps.
class Account < ActiveRecord::Base
  with_options :dependent => :destroy do |assoc|
    assoc.has_many :customers
    assoc.has_many :products
    assoc.has_many :invoices
    assoc.has_many :expenses
  end
end
{% endhighlight %}

The `with_options` method is a really cool chunk of code that lets you DRY up
duplication that sometimes appear when passing the same options to a series of
methods.

Curious <strong>how it works behind the scenes</strong>? Check out this
11-minute code walkthrough:

<iframe width="740" height="375"
  src="http://www.youtube.com/embed/OBOl9fFuILk?hd=1" allowfullscreen></iframe>
