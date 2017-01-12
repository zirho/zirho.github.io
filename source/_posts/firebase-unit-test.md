---
title: Firebase Unit Testing
tags:
  - JavaScript
  - firebase
  - unit test
  - mocha
  - es2015
categories:
  - Technology
  - Firebase
  - Unit testing
date: 2017-01-11 10:34:32
---


{% asset_img "firebase-title-image.png" Firebase with Mocha %}

[Github: Firebase Unit Test](https://github.com/zirho/firebase-unit-test)

# Motivation

Firebase is a very powerful tool.

When you build apps on firebase, you want to make sure to set up accessibility rules for database correctly.

That's why firebase itself has a small UI portion of it for testing it out.

{% asset_img "firebase-simulator.png" Firebase simulator ui %}

A lot of changes may occur in the early phase of project, so you want a way to run many tests automatically to ensure rules are properly set.

Also, these tests can be used for changes in the future.

Yeah, BDD or TDD on firebase can be enabled on database scope with this approach.

My github repo demonstrates how you can accomplish this.

[Firebase Unit Test](https://github.com/zirho/firebase-unit-test)

# Features

### Authenticated user interactions

[Authenticated user test](https://github.com/zirho/firebase-unit-test/blob/master/src/auth.spec.js)

Firebase does not run any server side scripts. Instead it has a very interesting way to authorize a certain path based on the user session or any way you like.

You can find more details here.
[Firebase security rules](https://firebase.google.com/docs/database/security/)

So when you run tests, you want the test runner to act like it's been logged in as a certain user.

To achieve that, you want a tool built by firebase team, called firebase-admin.

You can login by UID with this tool and run tests with the user session.

### Unauthenticated user interactions

[Unauthenticated user test](https://github.com/zirho/firebase-unit-test/blob/master/src/unauth.spec.js)

Your app can have multiple instances of firebase.

Creating one with another config is very easy.

Newly created instance will be unauthenticated, so you can use it to test unauth behaviors.

[Creating another firebase instance without a user session](https://github.com/zirho/firebase-unit-test/blob/master/src/firebase.js#L8-L12)

# Conclusion

I wrote this repo for testing database rules. 

But this can be used to solve other various problems, such as database CRUD.

I hope this helps anyone interested in running tests on firebase.

Enjoy!

[Github: Firebase Unit Test](https://github.com/zirho/firebase-unit-test)

