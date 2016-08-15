---
title: Refactor legacy code today with webpack!
tags:
  - es6
  - es2015
  - babel
  - webpack
  - JavaScript
  - legacy code
  - refactor
categories:
  - JavaScript
date: 2016-08-13 21:59:01
---


&nbsp;
{% asset_img "wtf_reading.jpg" WHYYYY? %}

  **All codes are on github with different branches associated with process for demonstration**

[Github code](https://github.com/zirho/webpack-to-legacy)  [https://github.com/zirho/webpack-to-legacy](https://github.com/zirho/webpack-to-legacy)

# Legacy code

I recently started working at SteelHouse.
Here, we have several different teams working on different projects.
One of the projects is an old and out-dated jquery-based web app backed by php server side process.
Very classic. 🌵

It may be incorrect to call it a legacy code, but for me, it's very hard to read and add more functions to it.
Some of the JavaScript file contain more than 12,000 lines of code.
Mostly, it's not fun working with it.

I decided to start to refactor some of codes to ES6 modular approach.

***
# Objectives

When it comes to refactoring legacy code, one of the most important factors is workflow.
I have a couple of side projects in react, redux, and ES6 with webpack and I really like it.
Especially, HMR(Hot Module Replacement) has blown my mind. Thanks to contributers to webpack and [Dan](https://github.com/gaearon)! 🍻

I knew that this magical thing called HMR can help us make our workflow as smooth as possible.
So I decided to incorporate that in our project.

Here are my goals.

  1. Webpack with HMR
  1. Using ES6
  1. Integrating legacy code with ease

***
# Preparation

  1. setup npm package

      If you have not initialized your project with npm yet, this is the perfect time to do so.
      ```
      $ npm init --y
      ```

      Add few run-scripts to `package.json`

      ```
      "scripts": {
        "dev": "webpack-dev-server --inline --hot --config webpack.config.dev.js",
        "build": "webpack"
      },

      ```

  1. install related packages

      Install webpack and other stuff for workflow and transpiling.
      ```
      $ npm i -D webpack webpack-dev-server path babel-core babel-loader babel-preset-es2015 babel-preset-stage-2
      ```

  1. config webpack

      create `webpack.config.dev.js` file. below is minimal config for webpack-dev-server.
      ```
      var webpack = require('webpack')

      module.exports = {
        entry: {
          'hmr': 'webpack-dev-server/client?http://localhost:8080',
          'hmr2': 'webpack/hot/dev-server',
          'entryname': './js/entry.js',
        },
        output: {
          path:       path.join(__dirname, 'dist'),
          filename:   './bundle.js',
        },
        module: {
          loaders: [
            {
              test:   /\.js/,
              loader: 'babel',
              include: __dirname + '/src'
            },
          ]
        },
      }
      ```
***
# Integrating with legacy code

  The most tricky part is that legacy code messes up global scope. And they reference each other for no reason.
  One of the JS file has been blown up with more than 12,000 lines of plain old JavaScript.

  So one thing that I had to figure out was this:
  > Everything that I create in new files has to be available in global scope **almost** automatically.

  Which can be achieved like the steps below.

  1. Let's say we have a file called `legacy.js` that contains a function called `plus`
  and it is being used everywhere in `legacy.js`

  2. I take that function from `legacy.js` and put it in a newly created file called `utils.js` and have it exported like below.

  ```
  export function add(num1, num2) {
    return num1 + num2;
  }
  ```

  3. Now we want that function in global scope.

  Let's say webpack will generate the output file `bundle.js` with the entry file called `entry.js`.
  It would look like this:

  ```
  // Accept hot module reloading
  // To enable HMR with webpack, you have to make sure to have these lines of code in the entry files to accept hot module reloading.
  if (process.env.NODE_ENV !== 'production') {
    if (module.hot) {
      module.hot.accept()
    }
  }

  // using namespace import to get all exported things from the file
  import * as Utils from './utils'

  // injecting every function exported from utils.js into global scope(window)
  Object.assign(window, Utils)
  ```

  4. And put this output file before the script tag for `legacy.js` in html.

  Then, logacy.js will get to access global function `add`.

  ```
  <script type="text/javascript" src="bundle.js"></script>
  <script type="text/javascript" src="legacy.js"></script>
  ```

### Now all the goodies

  If I have that set in my workflow,

  * I can refactor the extracted function in ES6 syntax like this.

  ```
  export const add = (num1, num2) => {
    return num1 + num2;
  }
  ```

  * I can add as many functions, objects, and variables as I want in `utils.js` file in ES6 ways, and those will be in global scope automatically.

  * No more refreshing pages everytime you change js files. This is huge for me!!


# Result 💯

  I really enjoy coding in ES6 by-passing legacy code these days.
  Also, sometimes along the way, some functions are only used in ES6 codes then I can remove it from global scope.
  That way, it will get cleaner and cleaner.

  Now the biggest file came down to almost 10,000 lines of codes.
  This method is looking very positive.

  Plus, I get to use npm packages and have some unit testability.
  Maybe I will post something about unit tests on ES6 codes later on.

  Happy coding! 🖥  🍺

  **All codes are on github with different branches associated with process for demonstration**

[Github code](https://github.com/zirho/webpack-to-legacy)  [https://github.com/zirho/webpack-to-legacy](https://github.com/zirho/webpack-to-legacy)

