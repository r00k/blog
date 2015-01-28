---
layout: post
title: How to use Rails routes in feature specs
date: 2010-11-19 16:22:09.000000000 -05:00
categories:
- general
tags: []
status: publish
type: post
published: true
---
Want to use Rails' routes in your acceptance specs? No problem. Here's what you
want:

{% highlight ruby %}
# In spec_helper.rb

RSpec.configure do |config|
  # Make the rails routes available in all specs
  config.include Rails.application.routes.url_helpers
  # or
  # Make them available only in acceptance specs
  config.include Rails.application.routes.url_helpers, :type => :acceptance
end
{% endhighlight %}

Fair warning: I've seen some folks say that you shouldn't do this, and instead
should define your routes in paths.rb. This way testing your routes is part of
your acceptance suite.

I see where they're coming from, but this just doesn't seem worth it to me. I
don't find routes to be a very bug-prone area in my apps, so keeping duplicate
my routes in two files is too high a price to pay.

I've been using Rails' built-in route helpers in my specs for several months
now, and can't think of a single routing issue that's come up. YMMV.
