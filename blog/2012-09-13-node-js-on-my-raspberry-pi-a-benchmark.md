---
author: admin
date: 2012-09-13 19:24:58+00:00
draft: false
title: Node.js running on my Raspberry Pi. A benchmark.
type: post
url: /blog/2012/09/13/node-js-on-my-raspberry-pi-a-benchmark/
categories:
- Software
- Tecnología
tags:
- benchmark
- nodejs
- raspberry pi
- server
---

Few weeks ago I could not resist the temptation to buy a [Raspberry Pi](http://www.raspberrypi.org/), the super-cheap 35$ computer that comes with 256MB of RAM and a ARM CPU running at 700MHz and fits in your pocket (more information in [wikipedia](http://en.wikipedia.org/wiki/Raspberry_Pi)).

![Raspberry Pi (wikipedia)](http://upload.wikimedia.org/wikipedia/commons/thumb/3/3d/RaspberryPi.jpg/300px-RaspberryPi.jpg)


See how nice it looks. I am more of a software guy, so the first thing I did was to install **node.js** (v0.6.19) develop the simplest web server you can create in node (5 lines, it simply returns a 200 HTTP response code without any contents) and put the beast to work.


    
    
    var http = require('http');
    http.createServer(function (req, res) {
      res.writeHead(200);
      res.end(); 
    }).listen(1337);
    





### The benchmark



I was interested in testing the number of requests per second the application was able to handle running on the Raspberry in the most optimistic scenario. After having some problems running [**httperf**](http://code.google.com/p/httperf/) and [**autobench**](https://github.com/menavaur/Autobench) on Mac OS, I finally went with [**apachebench**](httpd.apache.org/docs/2.2/programs/ab.html) (ab), that can be used to do simple load testings.

These are the results of sending 5120 requests to the node web server, at different concurrency levels, using the following command:


    
    
    ab -n 5120 -c <concurrency> http://192.168.1.36:1337/
    



[![raspberry benchmark results](http://guidogarcia.net/blog/wp-content/uploads/2012/09/raspberry_concurrency-300x282.png)
](http://guidogarcia.net/blog/wp-content/uploads/2012/09/raspberry_concurrency.png)

Additional information: Each concurrency level has been executed three times from my laptop and using a wifi connection; the graph shows the average value. The Raspberry Pi was running the Raspbian “wheezy” image ([downloads](http://www.raspberrypi.org/downloads)).



### Open points



Almost **200 requests per second** in this non real world application that does nothing. It is not bad, enough to develop and try the ideas I have in mind. To be honest, I still do not know why the performance drops so much when the concurrency is 512, or which part (my laptop vs the raspberry) is the bottleneck and why. Any ideas?

I have to measure other aspects like CPU and memory usage. In a quick glance, it seems that the CPU quickly goes over 90% usage even with small concurrency leves. I still appreciate this piece of hardware, but in the future I will try to overclock the processor. The memory was under 10%, what is not strange in this simple application.

I am also waiting for the Java Virtual Machine, that is supposed to be included in the default file system in future releases, to repeat the benchmarks (and probably see how it eats the memory).

It seems interesting, from a research point of view, to [build a cluster](http://www.zdnet.com/raspberry-pi-meets-lego-in-supercomputer-like-cluster-photos-7000004209/) and see how it scales. Donations for this purpose are highly appreciated :)
