---
author: admin
date: 2014-03-27 16:43:59+00:00
draft: false
title: Lazy loading of modules in nodejs
type: post
url: /blog/2014/03/27/lazy-loading-modules-nodejs/
categories:
- Software
- Tecnología
tags:
- javascript
- lazy loading
- nodejs
- patterns
- require
---

This is a pattern I found in [pkgcloud](https://github.com/pkgcloud/pkgcloud/blob/master/lib/pkgcloud.js#L93) to lazy-load nodejs modules. That is, to defer their loading until a module is actually needed.

{{< highlight js >}}
  var providers = [ 'amazon', 'azure', ..., 'joyent' ];
  ...

  //
  // Setup all providers as lazy-loaded getters
  //
  providers.forEach(function (provider) {
    pkgcloud.providers.__defineGetter__(provider, function () {
      return require('./pkgcloud/' + provider);
    });
  });
{{< / highlight >}}

It basically defines a getter, so modules won't be loaded until you do:

{{< highlight js >}}
var provider = pkgcloud.providers.amazon;
{{< / highlight >}}

It might be useful in applications where you have different adapters ("providers" in the example above) offering different implementations of the same API, and you want to let the user choose which one to use at runtime. This is a common requirement in cloud environments but it could be applicable to other scenarios as well (e.g. choose a payment gateway).

This is the first time I see it, so please share your thoughts on it and any other alternative approaches.
