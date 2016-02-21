---
title: A Querulous Query
layout: post
---

When I began my web programming career, `active_record` was already in wide
use.

This was nice for my productivity, but meant I never got around to learning
much SQL.

Turns out, `active_record` was usually enough. I'd type some Ruby and out would
come objects. Speedy enough, most of the time.

But today it wasn't. I had a bunch of podcasts and needed to sort them by the
number of downloads their episodes had gotten in the last week.

Not the hardest task ever, but my naive Ruby had n+1s up the wazoo.

At first, I didn't want to accept that it was time to stop and do some
research. I poked and prodded almost at random, hoping some combination of
`group` and `join` and Stack Overflow snippets would solve it. I spent longer
in this state than I'm proud of.

Finally, I gave in and worked through [this SQL
tutorial](http://sqlzoo.net/wiki/SQL_Tutorial).

After less than an hour, I understood the pieces I needed. Thirty minutes more
and I [had my query](https://gist.github.com/r00k/01340add5cba8dee53df).

It's an interesting impulse, the desire to make forward progress at all costs.
Sometimes it serves me well: shipped is usually better than perfect.

But today, I'm glad I took a breath, stepped back, and [sharpened the
saw](http://www.hanselman.com/blog/SharpenTheSawForDevelopers.aspx).
