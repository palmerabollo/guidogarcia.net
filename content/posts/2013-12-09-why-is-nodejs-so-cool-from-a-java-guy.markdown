---
author: admin
date: 2013-12-09 14:43:51+00:00
draft: false
title: Why is node.js so cool? (from a Java guy)
type: post
url: /blog/2013/12/09/why-is-nodejs-so-cool-from-a-java-guy/
categories:
- Software
- Tecnología
tags:
- asynchronous
- java
- nodejs
- npm
- patterns
---

### I confess: I am a Java guy

At least I used to be. Until I meet node.js. I still think the JVM is one of the greatest pieces of technology ever created by man, and I love the Spring Framework, the hundreds of Apache Java libraries or the over-six-hundred-page books about JEE patterns. It is great for big applications that are created by many developers, or applications that are made to last.

But many applications today are not made to last. Sometimes you just want to test something fast. Fail fast, fail cheap, keep it simple... the "be lean" mantra, you know.

Moreover, open source has completely changed the way we build applications, moving from developing tons of code in monolithic applications to assembling [small programs](http://mkhadikov.com/2012/02/02/programs-should-be-small.html) that use third-party components as middlewares (nosql databases, queues, caches).

### Second confession: I hate(d) Javascipt

Yes, Internet Explorer 4 made me hate Javascript. So the first time I heard about node.js and server-side Javascript I felt a shiver down my spine. It got worse when I started to play with the unfamiliar [continuation-passing style](http://en.wikipedia.org/wiki/Continuation-passing_style), the asynchronous callback hell did not take long to appear.

### A simple pattern: function(err, result) {}

But the absence of rules does not necessarily has to mean chaos. In fact, there is one pattern in node.js: your callbacks will have two arguments; the first argument will be an error object, the second one will be the result. This is your contract with the platform and, more important, with the community. Stick with it and you will be fine.

Using such a **popular programming language** plus this **simple convention** is what makes it so easy to start working with node.js. It makes building small modules that work together with other developers' modules surprisingly easy. This is why we have more than 50K modules in the [npm registry](https://npmjs.org/). Most of them are probably worthless, but natural selection also applies here, and this evolutionary process is much faster than the Java Community Process (JCP).

With node.js I feel like a productive anarchist. I get shit done.

_You should also read "[Broken Promises](http://www.futurealoof.com/posts/broken-promises.html)", "[Why is node.js becoming so popular](http://www.quora.com/Node-js/Why-is-Node-js-becoming-so-popular)" (quora), and watch [Mikeal Rogers' talk on why is node so successful](http://www.youtube.com/watch?v=2RXprpqRsfc) (24 min)._
