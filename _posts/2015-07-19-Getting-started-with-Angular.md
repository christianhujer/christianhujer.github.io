---
layout: post
title: Getting started with Angular
---

My friends at Equal Experts did this very nice example of how to get started with Angular.js.
Here are the steps:

# 1. Install `nvm`, the Node Version Manager

The simplest way to install `nvm` is to download and run the `install.sh` script of `nvm`, like this:

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash

or if you prefer `wget` over `curl`:

    wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash

More information: <https://github.com/creationix/nvm>

# 2. Use `nvm` to install *node.js* (`node` and `npm`)

You can do it like this:

    nvm install node

This installs *node.js* (`node` and `npm`) in your home directory.

# 3. Clone the nice `ShoppingCart` project from Anita Kadam

    git clone https://github.com/anitakadam/ShoppingCart
    cd ShoppingCart

# 4. Use `npm` to install `bower`

    npm install -g bower

# 5. Use `bower` to install the required libraries, including *Angular.js*

    bower install

# 6. Use `npm` to install required node packages

    npm install

# 7. Use `npm` to run the project

    npm start

# 8. Open your browser and connect to <http://localhost:3000>

# 9. Enjoy
