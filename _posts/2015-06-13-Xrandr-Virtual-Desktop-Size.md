---
layout: post
title: Xrandr to modify Virtual Desktop Size
---

If you, like me, are using Linux since the 90ies, you might remember the times when you could easily configure a virtual desktop size.
Usually, you would configure a virtual desktop size bigger than the screen size.

I don't know why, but in today's KDE with OpenSUSE or Kubuntu, this no longer seems possible.
This is sad.

However, I've found a workaround.
The workaround is to use the `xrandr` command to configure the size of a virtual desktop.
The following `xrandr` command could be used to have a virtual desktop of 1920x1080 when the screen size in fact is only 1600x900.

{% highlight bash %}
xrandr --fb 1920x1080 --output eDP --mode 1600x900 --panning 1920x1080
{% endhighlight %}

If you want to know the correct values for `--output` and `--mode`, just check the output of a vanilla `xrandr` command.
