---
layout: post
title: Getting started with Angular
---

My friends at [Equal Experts]<http://www.equalexperts.in/> did this very nice example of how to get started with Angular.js.
Here are the steps:

## 1. Install `nvm`, the Node Version Manager

The simplest way to install `nvm` is to download and run the `install.sh` script of `nvm`, like this:

{% highlight bash linenos %}
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash
{% endhighlight %}

or if you prefer `wget` over `curl`:

{% highlight bash linenos %}
    wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash
{% endhighlight %}

More information: <https://github.com/creationix/nvm>

## 2. Use `nvm` to install *node.js* (`node` and `npm`)

You can do it like this:

{% highlight bash linenos %}
    nvm install node
{% endhighlight %}

This installs *node.js* (`node` and `npm`) in your home directory.

## 3. Clone the nice `ShoppingCart` project from Anita Kadam

{% highlight bash linenos %}
    git clone https://github.com/anitakadam/ShoppingCart
    cd ShoppingCart
{% endhighlight %}

## 4. Use `npm` to install `bower`

{% highlight bash linenos %}
    npm install -g bower
{% endhighlight %}

## 5. Use `bower` to install the required libraries, including *Angular.js*

{% highlight bash linenos %}
    bower install
{% endhighlight %}

## 6. Use `npm` to install required node packages

{% highlight bash linenos %}
    npm install
{% endhighlight %}

## 7. Use `npm` to run the project

{% highlight bash linenos %}
    npm start
{% endhighlight %}

## 8. Open your browser and connect to <http://localhost:3000>

## 9. Enjoy

## 10. All commands in one sequence

{% highlight bash linenos %}
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash
    wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash
    nvm install node
    git clone https://github.com/anitakadam/ShoppingCart
    cd ShoppingCart
    npm install -g bower
    bower install
    npm install
    npm start
{% endhighlight %}
