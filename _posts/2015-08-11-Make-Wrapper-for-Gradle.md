---
layout: post
title: Make Wrapper for Gradle
---

If you are like me, you want to build everything with `make`.
Even when it is, actually built by something else.
In my current project, the build is actually performed with `gradle`.
But I keep typing `make`, and I do not want to break with that habit.

So here's what I did:
I came up with a small wrapper that runs `gradle`, or in this case, `./gradlew`, from `make`.

{% highlight make linenos %}
.PHONY: -rungradle-
-rungradle-:
	./gradlew $(MAKECMDGOALS)

$(MAKECMDGOALS): -rungradle-
{% endhighlight %}

This wrapper will call `./gradlew` from `make` with whatever non-option arguments were given to `make`.
For example, you can call it like `make check` and it will run `./gradlew check`.

Of course a more sophisticated wrapper is thinkable, but for most of the cases this tiny wrapper will do the job.
Another possibility would be to turn `make` into a shell function which checks whether a `Makefile` is available, and if not, looks for alternatives like a `build.gradle`, and if it finds an alternative, it runs the suitable alternative build tool, like, in that case, `gradlew`, but that's a story for another time.
