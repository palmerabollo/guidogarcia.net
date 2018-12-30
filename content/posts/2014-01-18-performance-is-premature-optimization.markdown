---
author: admin
date: 2014-01-18 19:06:03+00:00
draft: false
title: Performance is premature optimization
type: post
url: /blog/2014/01/18/performance-is-premature-optimization/
categories:
- Software
- Tecnología
tags:
- benchmark
- gtd
- nodejs
- performance
- software
---

I will burn in hell, but **performance is premature optimization nowadays**. Despite it is very interesting from an engineering perspective, from the practical point of view of someone who wants to follow the make-shit-happen startup mantra, my advice is not to worry much about it when it comes to choosing a programming language.

There are things that matter more than the technology stack you choose. In this post I will try to explain why; then you can vent your rage in the comments section.

[![Get Shit Done](/images/get-shit-done.jpg)
](http://www.startupvitamins.com/products/startup-poster-aaron-levie-get-shit-done)

### Your project is not Twitter

It is not Facebook either, and it probably won't. I am sorry.

Chances of your next project being popular are slim. Even if you are so lucky, you app will not be popular from day one. Even if you are popular enough, hardware is so cheap at that point that it could be considered free for all practical purposes (around one dollar per day for a 1CPU/1GB machine; go compare that with our wages).

### Your project will fail

Face it. You are not alone, most projects fail and there is nothing wrong with it. They fail before performance becomes an issue. I do not know a single project that has failed solely due to a bad choice of a programming language.

So I think that, as a rule of thumb, it is a good idea to choose the technology that allows you to try and develop [small components](http://yobriefca.se/blog/2013/04/29/micro-service-architecture/) faster (nodejs, is that you?). You will have time to throw some of those components away and rebuild their ultra-efficient alternatives from scratch in the unlikely case of needing it.

### Conclusion

You are not going to need performance; **stop worrying and get shit done instead**. I always have a Moët Et Chandon Dom Pérignon 1955 on the fridge to celebrate the day I face performance issues due to choosing X over Y.
