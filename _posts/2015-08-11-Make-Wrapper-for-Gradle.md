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

Of course a more sophisticated wrapper is thinkable, but for most of the cases this tiny wrapper will do the job.
