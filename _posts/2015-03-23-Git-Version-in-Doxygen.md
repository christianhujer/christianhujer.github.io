---
layout: post
title: Git Version in Doxygen
---

When generating Doxygen documentation, you might want to tell the reader which version of the source code was used to generate the documentation.
Here's how to do that when you're using git.

The ingredients are:

- A `Makefile` that calls `doxygen`, because that's what you're most likely using when dealing with Doxygen.
- The ability of `make` to `export` variables to the environment.
- The ability of `make` to deal with expressions that are `stdout` of shell commands, by using the `$(shell ...)` built-in function.
- The ability of `doxygen` to refer to environment variables in its configuration file `Doxyfile`, and a `Doxyfile` that makes use of this.
- The `git` plumbing command `rev-parse` to get the version number.
- The `git` plumbing command `diff-index` to find out whether something was modified.

Here's the `Makefile`:

{% highlight make linenos %}
doc: export PROJECT_NUMBER:=$(shell git rev-parse HEAD ; git diff-index --quiet HEAD || echo "(with uncommitted changes)")

.PHONY: doc
doc:
	doxygen
{% endhighlight %}

Here's the relevant section of the `Doxyfile`:

{% highlight make linenos %}
# The PROJECT_NUMBER tag can be used to enter a project or revision number. This
# could be handy for archiving the generated documentation or if some version
# control system is used.

PROJECT_NUMBER         = $(PROJECT_NUMBER)
{% endhighlight %}

In case you need a `bash` script instead of a Makefile, here you go:

{% highlight bash linenos %}
#!/bin/bash
export PROJECT_NUMBER="$(git rev-parse HEAD ; git diff-index --quiet HEAD || echo '(with uncommitted changes)')"
exec doxygen
{% endhighlight %}

There you go!
