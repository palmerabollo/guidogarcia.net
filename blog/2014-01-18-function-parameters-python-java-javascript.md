---
author: admin
date: 2014-01-18 11:31:44+00:00
draft: false
title: Function parameters in Python, Java and Javascript
type: post
url: /blog/2014/01/18/function-parameters-python-java-javascript/
categories:
- Software
tags:
- function
- java
- javascript
- nodejs
- parameters
- python
- verbosity
---

This is a short post about how these programming languages compare with each other when it comes to declaring functions with optional parameters and default values. Feel free to leave alternatives in other languages in the comments.



### Python. The good.



Python is my favorite. Use your parameters in any order and define their default values as part of the function signature itself.


    
    
    def foo(arg1, arg2="default"):
        print "arg1:", arg1, "arg2:", arg2
    



The price to pay is that you can not define two methods with the same name in the same class.


    
    
    def sum(a, b):
        return a + b
    
    def sum(a, b, c):
        return a + b + c
    



I am not a Python expert, but it does not seem such a big deal.



### Java. The ugly.



Java is more verbose, but you have strong types and simple refactoring in exchange.


    
    
    public void foo(String arg1) {
        foo(arg1, "default");
    }
    
    public void foo(String arg1, String arg2) {
        System.out.printf("arg1: %s arg2: %s", arg1, arg2);
    }
    






### Javascript. The bad.



Javascript is a little more ugly.


    
    
    function foo(arg1, arg2) {
        arg2 = arg2 || 'default';
        console.log('arg1 %s arg2 %s', arg1, arg2);
    }
    



This is [real code](https://github.com/joyent/node-smartdc/blob/master/lib/cloudapi.js#L223) we use in [Instant Servers](http://cloud.telefonica.com/), to have an optional first parameter:


    
    
    CloudAPI.prototype.getAccount = function (account, callback, noCache) {
        if (typeof (account) === 'function') {
            callback = account;
            account = this.account;
        }
        if (!callback || typeof (callback) !== 'function')
            throw new TypeError('callback (function) required');
        ...
    }
    



It is pure crap.
