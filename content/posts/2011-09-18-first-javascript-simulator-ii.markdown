---
author: admin
date: 2011-09-18 17:03:47+00:00
draft: false
title: My first Javascript robotics simulator (and II)
type: post
url: /blog/2011/09/18/first-javascript-simulator-ii/
categories:
- Software
- Tecnología
tags:
- javascript
- robotics
- simulator
---


A week ago I wrote [a post about a Javascript robotics simulator](http://guidogarcia.net/blog/2011/09/13/first-javascript-robotics-simulator/). As an experimental project, I took the opportunity to try different technologies: **HTML5**, **Javascript** and **Git**. In this post I am going to write about what I have learnt with this project from a technical point of view, and what the future lines of work could be.






### HTML5






I learnt how the new **slider** input works, and how to use a **canvas** to display graphical information into a web browser. The canvas element is not intended to keep track of a set of moving objects because it only knows about pixels ([read more](http://stackoverflow.com/questions/2720477/html5-move-canvas-object/2723412#2723412) about this). For this reason SVG could have been a good alternative in this case.







I also tried to use **Web Workers** to run the simulation in the background, avoiding to block the UI, but I had some problems sending information back and forth to the workers, so this feature will have to wait (the code is shared at [github](https://github.com/palmerabollo/rvo2-js), so contributions are welcome)







HTML5 support is already pretty good and, if in one of my last posts I blamed Google, today I have to say they are making a great effort to take the Internet forward.






### Javascript & TDD






I mostly work with Java, so my mind tend to think in an object-oriented way. I tried to discover good practices about how to organize the code, but I must admit I still have to learn a lot about Javascript. The fact that my code is a port from C# did not help at all. To compensate for it, I had the chance to install and try [MonoDevelop](http://monodevelop.com/).







I also tried to learn how to adopt [TDD](http://en.wikipedia.org/wiki/Test-driven_development) practices in Javascript. I finally did a proof of concept with the `console` object. This is an example:


    
    
    // Code to be tested
    function Vector2(x, y) {
        this.x = x;
        this.y = y;
    
        this.plus = function(vector) {
            return new Vector2(x + vector.x, y + vector.y);
        };
    }
    
    // Tests
    console.group("Coordenates are set");
        v = new Vector2(3, 2);
        console.assert(v.x === 3, "x coordinate is not set");
        console.assert(v.y === 2, "y coordinate is not set");
    console.groupEnd();
    
    console.group("Sum two vectors");
        v1 = new Vector2(3, 2);
        v2 = new Vector2(5, 7);
        res = v2.plus(v1);
        console.assert(res.x === 5 + 3, "added x coordinate is wrong");
        console.assert(res.y === 7 + 2, "added y coordinate is wrong");
    console.groupEnd();
    



It is probably the easiest way to write a unit test in Javascript, but it is very limited as well. The second point to be done is to try a complete testing framework such as [QUnit](http://docs.jquery.com/Qunit) or [Jasmine](http://pivotal.github.com/jasmine/), as recommended by [Alejandro](http://twitter.com/fuzzyalej).






### GIT. Sourceforge who?






I shared [the code in github](https://github.com/palmerabollo/rvo2-js) (please feel free to contribute) and I tried the new [EGit plugin](http://eclipse.org/egit/) to work with github (or any other git repository) from Eclipse.







It works fine, but despite I can see many of the advantages of git, I don't think it is easier or more intuitive to use than SVN. I am still not used to the whole git workflow, but I will keep trying because what is clear is that github is amazing and has changed the way people cooperate in the open-source world.






## Other lines of work






	  * Fix existing bugs when obstacles are present.
	  * Compare the ORCA algorithm with other ones, specially those related with [genetic algorithms](http://en.wikipedia.org/wiki/Genetic_algorithm). Apply this kind of algorithms to model human behaviour, more effective placement of emergency exits in buildings, etc.
	  * Integrate this algorithm into a piece of hardware, such as an Arduino board. I can also think about several Roomba robots sweeping my floor in less time without collisions.

