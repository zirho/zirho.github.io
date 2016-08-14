---
title: The gist of Redux
date: 2016-07-25 09:00:00
tags: [JavaScript, redux, gist]
categories: 
- JavaScript
---


# Principles


### What is a pure function

the same output with the same input
not modifying input 
focused on one thing

### Reducer functions

input current state, action 
output next state

### Why immutable for store objects

it's all about diff

if it's mutable

var obj1 = {};
var obj2 = obj1;

obj1['a'] = 'some';


obj == obj
=> true

obj === obj
=> true

