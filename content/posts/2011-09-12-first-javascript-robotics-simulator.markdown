---
author: admin
date: 2011-09-12 23:02:22+00:00
draft: false
title: My first Javascript robotics simulator (I)
type: post
url: /blog/2011/09/13/first-javascript-robotics-simulator/
categories:
- Software
- Tecnología
tags:
- javascript
- robotics
- rvo2
- simulator
---


I must admit I have always been fascinated about robotics, especially about how a swarm of robots can cooperate, following a set of simple rules that lead to complex [emergent behaviours](http://en.wikipedia.org/wiki/Emergence), like those you can see in the exciting motion of a flock of birds.







Whereas I have some elctronics background, the bad news is that I am a software developer at heart. This is why I find a big obstacle when it comes to the hardware layer. My last experiment in this area is a software multi-agent simulator, where multiple independent agents avoid collisions with each other without communication among them while moving in a common arena.







It seems like a hard task, but it is only an experimental port from the C# version of the already [existing RVO2 Library](http://gamma.cs.unc.edu/RVO2/) (based on the [ORCA algorithm](http://gamma.cs.unc.edu/ORCA/)) to Javascript. The whole **[code is shared in github](https://github.com/palmerabollo/rvo2-js)** along with a README file containing basic usage instructions. I am not really proud of the current code, mostly because of my lack of extensive and in-depth experience with Javascript. Please feel free to contribute to improve the project.





As an experimental project, it is not intended to be the most efficient solution. Instead, I took the opportunity to try different technologies: **HTML5**, **Javascript & TDD** and **Git**. In the next post I will write about what I have learnt with this project from this technical point of view, and about the future lines of work.





## One demo is worth than one thousand words






I have set up a **couple of demos** ([radial demo](http://guidogarcia.net/demos/rvo2-js/index.html), [linear demo](http://guidogarcia.net/demos/rvo2-js/index_linear.html)) where you can adjust some parameters (number of agents, agent size, presence of obstacles) and run a simulation. Remember that you need a modern browser with HTML5 support (i.e. Chrome or Safari) and, depending on the number of agents, a decent CPU if you want to smoothly run it.




[![RVO2 simulator](http://guidogarcia.net/blog/wp-content/uploads/2011/09/rvo2js.png)
](http://guidogarcia.net/demos/rvo2-js)




What I find most interesting here is that the **agents are completely autonomous**, a kind of "collective intelligence" emerges, but the agents do not communicate any information among them to coordinate their movements. You should be impressed at this point. Please use the comments sections if you have any idea or any suggestion to improve the simulator.






## Real life applications






I can imagine using this algorithm for several applications, all of them are already very well documented in the Internet: 



  * **Robotics**. Coordinate robots in a factory to avoid collisions ([example](http://gamma.cs.unc.edu/CA/)).
  * **Games development**. See [RVO2 Library integration with Unreal Development Kit](http://gamma.cs.unc.edu/RVO2-UDK/). If this rvo2-js library evolves it could be used in javascript games.
  * **Human behaviour**. Model human behaviour in crowded supermarkets, shopping malls and sports stadiums, or even during emergency situations.


I am sure there are many other applications you can think about. Use the comments below to share your feedback and questions.

