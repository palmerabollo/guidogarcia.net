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

{{< highlight python >}}
def foo(arg1, arg2="default"):
    print "arg1:", arg1, "arg2:", arg2
{{< / highlight >}}

The price to pay is that you can not define two methods with the same name in the same class.

{{< highlight python >}}
def sum(a, b):
    return a + b

def sum(a, b, c):
    return a + b + c
{{< / highlight >}}

I am not a Python expert, but it does not seem such a big deal.

### Java. The ugly.

Java is more verbose, but you have strong types and simple refactoring in exchange.

{{< highlight java >}}
public void foo(String arg1) {
    foo(arg1, "default");
}

public void foo(String arg1, String arg2) {
    System.out.printf("arg1: %s arg2: %s", arg1, arg2);
}
{{< / highlight >}}

### Javascript. The bad.

Javascript is a bit more ugly.

{{< highlight js >}}
function foo(arg1, arg2) {
    arg2 = arg2 || 'default';
    console.log('arg1 %s arg2 %s', arg1, arg2);
}
{{< / highlight >}}

This is [real code](https://github.com/joyent/node-smartdc/blob/master/lib/cloudapi.js#L223) we use in Instant Servers, to have an optional first parameter:

{{< highlight js >}}
CloudAPI.prototype.getAccount = function (account, callback, noCache) {
    if (typeof (account) === 'function') {
        callback = account;
        account = this.account;
    }
    if (!callback || typeof (callback) !== 'function')
        throw new TypeError('callback (function) required');
    ...
}
{{< / highlight >}}

It is pure crap.
