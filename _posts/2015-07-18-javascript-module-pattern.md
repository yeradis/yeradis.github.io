---
layout: post
title: 'Some shit for your javascript: The Module Pattern'
tags: js programming development language javascript design pattern module
comments: true
permalink: javascript-module-pattern
share: true
---

The Module Pattern was originally defined as a way to have software modules in a programming language with incomplete direct support for the concept. 

This pattern in JavaScript is used to mimic the classes concept and focuses on public and private encapsulation in such a way that we're able to include both public/private methods and variables inside a single object. 

The Module Pattern is, the most commonly used design pattern and widely accepted, you can find it in a number of large projects such as jQuery, Dojo, ExtJS, AngularJS and YUI.

To implement this pattern we will use another one know as the [Immediately-invoked function expression](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression)

What? Well, this one:

```js
(function () {
  // the code here is executed once in its own scope
})();
```
It declares a function, which then calls itself immediately. 

So, lets watch a sample of the Module pattern:

```js
var MyFancyModule = (function () {

  var privateMethodOne 		= function () {};
  var privateMethodTwo 		= function () {};
  var privateMethodThree 	= function () {};
  
  return {
    publicMethodOne: function () {
      // You can call `privateMethodX()` you know...
    },
    publicMethodtwo: function () {

    },
    publicMethodThree: function () {

    }
  };

})();

```
Now you can get an idea on how to organize your shitty code.


