---
author: admin
date: 2014-03-18 06:56:37+00:00
draft: false
title: Cloud is not cheap
type: post
url: /blog/2014/03/18/cloud-is-not-cheap/
categories:
- Cloud
- Tecnología
tags:
- cloud
- iaas
---

There is a myth about cloud computing. Many people think they will save money moving their services to the cloud, but the reality is that the cloud is not cheap.

Virtualization, one of the core parts of cloud computing, tries to meet the promise of elastic capacity and pay-as-you-go policies. Despite of this promise, the true story is that today we are running virtual machines that don't do much because, **most part of the time, our applications are not doing anything**. Their processors are underutilized. While this is an opportunity for cloud providers to oversubscribe their data centers, it also means we are overpaying for it. There is still much untapped potential for applications running on the cloud.

### Services in the 21st century

In the last few years we have seen many improvements in the way applications are packaged and deployed to the cloud, how to automate these processes, and we have learnt that we have to build applications for failure (see "[There will be no reliable cloud](http://blog.hendrikvolkmer.de/2013/04/03/there-will-be-no-reliable-cloud-part-1/)").

But what I have not seen yet is anything about services communicating to each other to share its health status. I think **services in the cloud should be able to [expose their status](https://github.com/palmerabollo/express-ping) in real time**. This way they could talk to others and say "hey, I'm struggling to handle this load, who can help me out with 2 extra GB of RAM for [less than 10 cents/hour](http://aws.amazon.com/ec2/purchasing-options/spot-instances/)?".

How do you think cloud will change apps in the next 5-10 years? Ryan Dahl - How do you see the future of PaaS (see 4:38)

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/L_JKb61EalQ?start=277" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
