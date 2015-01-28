---
layout: post
title: Bundler's got bash-fu
date: 2011-03-15 21:13:34.000000000 -04:00
categories:
- general
tags: []
status: publish
type: post
published: true
---
Well, technically got it's got POSIX-compliant-fu. But that's even better; so
no worries.

Check out this line from bundler's <a
href="https://github.com/carlhuda/bundler/blob/1-0-stable/bundler.gemspec">gemspec</a>.
The goal of this line is to set s.test_files to an array of file located in
testing-related directories:

{% highlight ruby %}
s.test_files = `git ls-files -- {test,spec,features}/*`.split("\n")
{% endhighlight %}

There are a couple interesting things going on in this line:

- <strong>git ls-files</strong>: by default, this command outputs all the files
  in the index, but we're going to constrain it with a path supplied by...

- <strong>{test,spec,features}</strong>: this is a "curly brace expansion". In
  bash (and other POSIX shells), this will expand to "test spec features",
  however notice that we've got a trailing...

- <strong>/\*</strong>: when you've got leading or trailing characters around
  expansions, they are included in the expansion. So the full command of
  `{test,spec,features}/*` expands to `test/* spec/* features/*`

- Finally, git ls-files outputs the names of all the files in those directories, and we split them on newlines to get our array.

Finally, if you like the idea of curly brace expansions, but prefer to stay in Ruby-land, you can have the best of both worlds:

{% highlight ruby %}
Dir.glob("{test,spec,features}/*") # This also works
{% endhighlight %}

