---
layout: post
title: Depend Upon Abstractions
date: 2012-07-02 16:25:04.000000000 -04:00
categories:
- coding
tags: []
status: publish
type: post
published: true
---
One nice piece of advice for designing flexible programs is <strong>depend upon
  abstractions, not implementations</strong>.

This is the idea behind the <a
  href="http://www.refactoring.com/catalog/extractClass.html">extract class
refactoring</a>. You package up some set of data and functionality, and only
allow clients to interact with it through a public API. The class's internal
workings are intentionally hidden.

This tends to lead to flexible designs because implementation details can be
changed without affecting any of the calling code. Changes are kept to just one
file, which makes them easier, and ease of change is the best test for good
design.

Most programmers are at least somewhat familiar with this idea. However, it's
easy to forget it when you start working with outside libraries or services.

Here's a paraphrased example from a Rails app I reviewed recently. This app
uses the braintree gem to charge users, create subscriptions, and refund money.

{% highlight ruby %}
# Gemfile
gem 'braintree'

# app/models/user.rb
class User
  SUBSCRIPTION_AMOUNT = 10.to_money

  def charge_for_subscription
    Braintree.charge(SUBSCRIPTION_AMOUNT)
  end

  def create_as_customer
    Braintree.create_customer(user.name)
  end
end

# app/models/refund.rb
class Refund
  def process!
    Braintree.refund(order.amount, user.braintree_id)
  end
end
{% endhighlight %}

Just because we're calling methods in another class does not mean we're
programming against an abstraction. It's certainly better than making raw HTTP
calls to Braintree, but our choice of vendor and gem used are implementation
details that have leaked into our business logic. 

With calls to Braintree littered throughout, switching to another vendor will
require the editing and re-testing of many files. We've fallen short of the
ideal we described above, where one change requires edits only in one place.

Fortunately, the fix is quite easy: a simple wrapper.

{% highlight ruby %}
# app/models/user.rb
class User
  def charge_for_subscription
    PaymentGateway.charge_for_subscription
  end

  def create_as_customer
    PaymentGateway.create_customer(user.name)
  end
end


# app/models/refund.rb
class Refund
  def process!
    PaymentGateway.refund(self)
  end
end

# New file: lib/payment_gateway.rb
class PaymentGateway
  def charge_for_subscription
    Braintree.charge(SUBSCRIPTION_AMOUNT)
  end

  def create_customer(customer_name)
    Braintree.create_customer(customer_name)
  end

  def refund(refund)
    Braintree.refund(refund.order_amount, refund.user_braintree_id)
  end
end
{% endhighlight %}

Another good name for the new class would be PaymentGatewayAdapter, as it's an
example of <a href="http://en.wikipedia.org/wiki/Adapter_pattern">the adapter
pattern</a>.

This code is only subtly different, but yields significant benefits:

<ol>
  <li>Switching from Braintree to another vendor now requires edits to only one
  file. If this change is somewhat likely, this benefit alone probably
  justifies the refactoring.</li>

  <li>If Braintree changes its public API, we again have only one class to
  edit.</li>

  <li>Testing has become easier. PaymentGateway gives us what some people call
  'seams': a place where components join together where we can easily stub
  behavior. Before, unit tests required stubbing Braintree's code. Upgrading
  the gem could potentially break many tests. Now, we can stub our own methods,
    which is safer.</li>

  <li>We can use method names that better match our problem domain. A small
  win, but a pleasant one.</li>

  <li>We can change the parameters that PaymentGateway's methods expect, such
  as we did in PaymentGateway#refund.</li>

  <li>We have a better home for constants like SUBSCRIPTION_AMOUNT and similar
  data.</li>
</ol>

The only downside I can think of for this code is that it adds a bit of
indirection. You'll have to jump through one additional file to found out how
customer charging is implemented. However, the wrapper is very thin, so I don't
think this cost is large.

It's easy to accidentally find yourself depending on implementation details.
External libraries and services are frequently sources of this, and warrant
extra suspicion. Implementations that have a decent likelikhood of change
should always be wrapped in sanitary fashion.
