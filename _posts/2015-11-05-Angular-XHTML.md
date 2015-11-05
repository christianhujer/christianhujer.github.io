---
layout: post
title: Angular JS with XHTML
---

(Work in Progress)
I'm one of those guys who prefer XHTML over HTML.
You get all the power of HTML, and on top of it you can use all the XML tools.
That's neat.
However, some things work differently.
In this article I cover what works differently regarding Angular JS.

# -1 How to read this

This is a set of annotations for the [Angular JS Tutorial](https://docs.angularjs.org/tutorial).
Each of the steps in this Tutorial corresponds to a step in the Angular JS tutorial and covers the differences and specialities about using Angular JS for XHTML instead of HTML.
Therefore all the headlines are identical to those of the Angular JS tutorial.

# 0 Bootstrapping

In order to use XHTML instead of HTML, we need to

* Rename `index.html` to `start.xhtml`
* Tell the Node http-server to serve from `start.xhtml` as directory index instead of `index.htm` or `index.html`
* Change `start.xhtml` to be an XHTML file instead of a HTML file.

## Renaming index.html to start.xhtml
This one is simple.

    .../angular-phonecat $ mv app/{index.html,start.xhtml}

## Telling Node http-server to serve from `start.xhtml` instead of `index.htm[l]` as directory index.
TODO

## Changing `start.xhtml` to be XHTML instead of HTML
The changes we do for that are:

* Add an XML declaration (not mandaatory but useful for clarity).
* Use an uppercase `<!DOCTYPE>` XML directive.
* Add the XHTML namespace to the `<html/>` element with a corresponding `xmlns` attribute, i.e. `xmlns="http://www.w3.org/1999/xhtml"`.
* Replace the minified `ng-app` attribute with a non-minified `data` attribute `data-ng-app=""`.
* Turn Start Tags into Empty Element Tags for empty elements by replacing their `>` with `/>`.

The resulting `start.xhtml` file looks like this:

{% highlight xhtml linenos %}
<?xml version="1.0"?>
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" data-ng-app="">
<head>
  <meta charset="utf-8"/>
  <title>My HTML File</title>
  <link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap.css"/>
  <link rel="stylesheet" href="css/app.css"/>
  <script src="bower_components/angular/angular.js"></script>
</head>
<body>

  <p>Nothing here {{'yet' + '!'}}</p>

</body>
</html>
{% endhighlight %}
