---
author: admin
date: 2015-01-15 08:22:35+00:00
draft: false
title: Simple moving average with bacon.js
type: post
url: /blog/2015/01/15/moving-average-with-baconjs/
categories:
- Software
- Tecnología
---

[Bacon](https://baconjs.github.io/) is a _small functional reactive programming lib for JavaScript_. Sometimes it is easier to handle data as a stream and react to changes in the stream instead of processing individual events. In the example below (nodejs), a simple moving average.


    
    
    var Bacon = require('baconjs')
    
    function avg(array) {
      var sum = array.reduce(function(a, b) { return a + b; }, 0);
      return sum / array.length;
    }
    
    var bus = new Bacon.Bus();
    bus.slidingWindow(3).map(avg).onValue(console.log);
    bus.push(1); // output = 1
    bus.push(2); // output = 1.5
    bus.push(3); // output = 2
    



You can see [another example](http://guidogarcia.net/demos/bacon/) ([github](https://github.com/palmerabollo/test-bacon)) where an event stream is created and populated with your mouse positions. The "y" position is represented along with the moving average.
