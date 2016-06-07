---
title: Unit testing JavaScript ES6 code with Karma
date: 2016-06-06 14:55:52
tags: [karma, es6, es2015, babel, webpack, JavaScript, Unit Testing]
categories: 
- JavaScript
- Node.js
---

This posting is referenced to [this post](http://www.syntaxsuccess.com/viewarticle/writing-jasmine-unit-tests-in-es6).

{% asset_img "karma.png" ECMAScript2015 Unit testing %}

# Unit Testing ES6 codes #

To run unit tests against ES6 codes, we need a way to transpile the codes before the unit test runner(karma) processes them.

Here we use webpack to transpile ES6 codes with babel loader using babel es2015 preset.

But the original posting is outdated since babel is now split in serveral modules. We need few of them to get transpiling working.

The complete code is on a github repository.  
[Karma for es6 respository](https://github.com/zirho/karma-es6-babel-webpack)

## Prerequisites

<h3 id="Install-Node-js" class="article-heading"><a href="#Install-Node-js" class="headerlink" title="Install Node.js"></a>Install Node.js<a class="article-anchor" href="#Install-Node-js" aria-hidden="true"></a></h3><p>The best way to install Node.js is with <a href="https://github.com/creationix/nvm" target="_blank" rel="external">nvm</a>.</p>
<p>cURL:</p>
<figure class="highlight bash"><table><tbody><tr><td class="code"><pre><span class="line">$ curl https://raw.github.com/creationix/nvm/master/install.sh | sh</span><br></pre></td></tr></tbody></table></figure>
<p>Wget:</p>
<figure class="highlight bash"><table><tbody><tr><td class="code"><pre><span class="line">$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh</span><br></pre></td></tr></tbody></table></figure>
<p>Once nvm is installed, restart the terminal and run the following command to install Node.js.</p>
<figure class="highlight bash"><table><tbody><tr><td class="code"><pre><span class="line">$ nvm install 4</span><br></pre></td></tr></tbody></table></figure>
<p>Alternatively, download and run <a href="http://nodejs.org/" target="_blank" rel="external">the installer</a>.</p>


## Create project directory ##

Create a directory.

```bash
$ mkdir karma-testing; cd karma-testing
```

## Install related modules ##

Install node modules.

```bash
$ npm init --y
$ npm i -D karma karma-phantomjs-launcher phantomjs-prebuilt karma-jasmine jasmine-core babel-core babel-loader babel-preset-es2015 webpack karma-webpack
$ npm i -g karma-cli
```

Add or change npm script attribute to package.json

```json
  "scripts": {
    "tests": "karma start"
  },
```

## Generate karma config file ##

karma.conf.js

```bash
$ vi karma.conf.js
```

```javascript
module.exports = function(config) {
    config.set({
        browsers: ['PhantomJS'],
        files: [
            { pattern: 'test-context.js', watched: false }
        ],
        frameworks: ['jasmine'],
        preprocessors: {
            'test-context.js': ['webpack']
        },
        webpack: {
            module: {
                loaders: [
                    { test: /\.js$/, exclude: /node_modules/, loader: 'babel?presets[]=es2015' }

                ]
            },
            watch: true
        },
        webpackServer: {
            noInfo: true
        }
    });
};
```

## Provide context including testing files ##

* **The codes below are from [this post](http://www.syntaxsuccess.com/viewarticle/writing-jasmine-unit-tests-in-es6).**

test-context.js

```bash
$ vi test-context.js
```

```javascript
var context = require.context('./source', true, /-spec\.js$/);
context.keys().forEach(context);
```

source/calculator.js

```bash
$ vi source/calculator.js
```

```javascript
export class Calculator{
    add(op1,op2){
        return op1 + op2;
    }
    subtract(op1,op2){
        return op1 - op2;
    }
}
```

source/calculator-spec.js

```bash
$ vi source/calculator-spec.js
```
```javascript
import {Calculator} from './calculator';
describe('Calculator', () => {
   it('should add two numbers', () => {
       let calculator = new Calculator();
       let sum = calculator.add(1,4);
       expect(sum).toBe(5);
   });
    it('should subtract two numbers', () => {
        let calculator = new Calculator();
        let sum = calculator.subtract(4,1);
        expect(sum).toBe(3);
    });
});
```

## Kicking off unit testing ##

```bash
$ npm run tests
```

Enjoy testing ES6 codes!

The complete code is on a github repository.  
[Karma for es6 respository](https://github.com/zirho/karma-es6-babel-webpack)

