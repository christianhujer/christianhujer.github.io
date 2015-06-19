---
layout: post
title: Google Code Prettify for XHTML
---

I've been using Google Code Prettify for a while to have syntax highlighting done in the web browser.
Now I'm working on a project that uses XML/XSLT/XHTML, and Google Code Prettify didn't work with that.
Here's a description of how I analyzed and solved this problem.

Google Code Prettify doesn't work with XML/XSLT/XHTML out-of-the-box because of a combination of the following things:
- When you use XSLT to transform XML to XHTML "properly", the resulting document tree is namespaced with the XHTML namespace `'http://www.w3.org/1999/xhtml'`.
- When you use `document.createElement("span")` in a DOM with Namespaces, the resulting element will be in the empty namespace `''`.
  To create elements in the XHTML namespace, you'd have to use `'document.createElementNS("http://www.w3.org/1999/xhtml", "span")'` instead.
- When you style an XHTML document with CSS, the CSS applies to elements from the XHTML namespace, but not the empty namespace.
- Google Code Prettify uses `document.createElement()`, not `document.createElementNS()`.

This analysis already points to the solution.
I patched Google Code Prettify to use `document.createElementNS()` instead of `document.createElement()`.

You could also patch it yourself with the following sed script:
    $ sed -i -e "1 i\\NS_XHTML='http://www.w3.org/1999/xhtml';" -e 's/createElement(/createElementNS(NS_XHTML,/g' prettify.js

A git clone of a patched version (currently only `prettify.js` is patched) is available here: <https://github.com/christianhujer/google-code-prettify>
