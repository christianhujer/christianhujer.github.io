---
layout: post
title: A Dispatching Makefile
---

There's a `make` problem that has intrigued me and some colleagues for quite a while.
And while I had already solved it in the past, I wasn't happy with the solution.
The problem is: A Makefile that runs the goals that you want in the directories that you want?

This can be useful in several situations.
Here are two of them:
* You have a project that consists of various modules or components which are built separately.
  You want to build, or test, or clean all of them.
* You have a project that builds software for multiple platforms or configurations.
  You want to build, or test, or clean all of them.

So, here's a Makefile that can do this.
It is surprisingly small, simple and generic.
There's no need to change the Makefile when you add new subdirectories or want to run new make goals.
And of course it works perfectly well with option `-j` for parallelization to utilize your multi-core CPU.

Imagine that you have *n* subdirectories, and for each of these subdirectories you want to run `all`, `clean` or `test`.

{% highlight make linenos %}
TARGETS:=$(or $(MAKECMDGOALS),all)
DIRS:=$(wildcard */)

.PHONY: $(TARGETS)
$(TARGETS): $(DIRS)

.PHONY: $(DIRS)
$(DIRS):
	$(MAKE) -C $@ $(MAKECMDGOALS)
{% endhighlight %}

Now you can simply do this:

    $ ls
    Makefile foo/ bar/ buzz/
    $ make # Runs make in foo/ bar/ and buzz/.
    $ make clean # Runs make clean in foo/ bar/ and buzz/.
    $ make all test $ Runs make all test in foo/ bar/ and buzz/.

## How this works

The variable `TARGETS` contains the make goals that you want to dispatch.
In case an explicit goal was not specified, it is set to `all`.

The variable `DIRS` contains the directories to which you want to dispatch.
You can tweak this to your needs, in this example we're simply using all subdirectories.

The target `$(TARGETS)` performs the first half of the dispatching mechanism.
Whatever goal you have given is dispatched onto the directories.

The target `$(DIRS)` performs the second half of the dispatching mechanism.
Whatever directory is to be executed will be called with the supplied make goals.


Have a lot of fun!
