---
author: admin
date: 2011-12-05 14:52:22+00:00
draft: false
title: Getting started with Spring Roo
type: post
url: /blog/2011/12/05/getting-started-with-spring-roo/
categories:
- Software
- Tecnología
tags:
- java
- rad
- spring framework
- spring roo
- web development
---

For those who are not familiar with it, Spring Roo is an open source tool that provides rapid application development of Java applications, using standard Java technologies underneath.

Last week I gave a presentation ([slides](http://www.slideshare.net/palmerabollo/spring-roo-demo)) about how to start using [Spring Roo](http://www.springsource.org/spring-roo) to some co-workers in Telefónica Digital. I have recorded a video with captions for those who couldn't attend (use fullscreen mode and 720p).





The video does not cover some interesting aspects about Spring such as [Spring Data](http://www.springsource.org/spring-data) (I will write a post about it), or how to completely remove Spring Roo from your project (right button > Refactor > Push In and that is all).

If you are planning to give it a try, I strongly recommend to use **Spring Roo 1.2.x** in order to create layered architectures (controller + service + repository), instead of using the Active Record pattern (in words of Martin Fowler, _"An object that wraps a row in a database table or view, encapsulates the database access, and adds domain logic on that data"_)



## When would I use Spring Roo



Spring Roo is a relativly young project (Q4 2009), it is evolving really fast, and I think it is especially useful:




	  * To create your Spring project structure. Roo generates a typical Maven + Spring project. I think it is a good idea to use a proven structure and keep it consistent in every new project you start. Remember that you can remove Roo at any time, so you are not tied to it.
	  * To develop domain-intensive applications, that fit well with a CRUD user interface. Even administrative web interfaces to your existing applications seem good candidates to be developed with the help of Spring Roo.


Comments are open. Where are now the Ruby on Rails guys?
