---
title: Private properties for ES6 JavaScript
date: 2016-06-10 14:52:05
tags: [JavaScript, es6, es2015, private property, module]
categories: 
- JavaScript
---

{% asset_img "private.png" "Private Properties in ES6" %}


In ES5 JavaScript, it is relatively easy to have private properties in prototype definition.

It goes like this.

```javascript
// es5 constructor(as class) definition 
// JavaScript is prototype-based language
function Person(firstname, lastname) {

  // public property
  this.firstname = firstname;
  this.lastname = lastname;

  // private property 
  var records = [{type: 'in', amount: 0}];

  // public function
  // it needs to be instance method to access private properties
  this.addTransaction = function(trans) {
    if (trans.hasOwnProperty('type') && trans.hasOwnProperty('amount')) {
      records.push(trans);
    }
  }

  // public function
  this.getBalance = function() {
    var total = 0;

    records.forEach(function(record){
      if (record.type === 'in') {
        total += record.amount;
      } else {
        total -= record.amount;  
      }
    });

    return total;
  }
}

// Prototype function
Person.prototype.getFullName = function() {
  return this.firstname + " " + this.lastname;
};

module.exports = Person;
```

Although, in es6, it is not that easy to achieve and there are many options you can choose.

You can find all of those here, [Managing private data](http://www.2ality.com/2016/01/private-data-classes.html), [JS class definition](https://curiosity-driven.org/private-properties-in-javascript)

In my opinion, weekmap method is the best, if you need perfect privacy. Other than that, you could use conventional approach using underscore(_) in front of private property names.

But I found most cases can be solved by modularity approach which looks something like the code below.

In a file such as **person.js**

```javascript

let records = [{type: 'in', amount: 0}];

export class Person {

  constructor(first, last) {
    this.firstname = first;
    this.lastname = last;
  }
   
  addTransaction(trans) {
    if (trans.hasOwnProperty('type') && trans.hasOwnProperty('amount')) {
      records.push(trans);
    }
  }

  getBalance() {
    let total = 0;

    records.forEach(record => {
      total += record.amount;
    });

    return total;
  }

  getFullName() {
    return `${this.firstname} ${this.lastname}`;
  }

}
```

`record` property is used as a private data storage and can NOT be accessed out of the modular scope.

To use this class, you can just import and use. 
Below are unit tests that I wrote against to the `Person` class.

```javascript
import { Person } from './person.js';

describe('Person es6 class', function () {

  var person;
  beforeEach(function () {
    person = new Person('Andrew', 'Lincoln');
  });

  it('should be initiated with first name and last name', function () {
    expect(person.getFullName()).toEqual('Andrew Lincoln');
  });

  it("should be initiated with 0 balance", function() {
    expect(person.getBalance()).toEqual(0);
  });
});
```


Hope this helps.


