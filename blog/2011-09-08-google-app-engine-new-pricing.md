---
author: admin
date: 2011-09-08 22:37:16+00:00
draft: false
title: 'Google App Engine: Disturbances in the Force'
type: post
url: /blog/2011/09/08/google-app-engine-new-pricing/
categories:
- Tecnología
tags:
- cloud
- gae
- google
- paas
---

### A New Hope. The pay as you go promise.





I've been a big fan of [Google App Engine](http://appspot.com) (GAE) since it first appeared back in 2008. For those of you who don't know what am I talking about, GAE is a Platform as a Service (PaaS) cloud computing platform provided by Google where you can deploy your Python, Java and Go web applications.







You only have to deploy your application (using a very simple toolkit) and Google takes care of launching new application instances when necessary. This way you can focus on your application instead of the underlying infrastructure, that automagically scales to the infinity and beyond. In that sense, it really is much simpler than Amazon EC2.







Sounds great, but with great power comes great responsibility. All that comes at the cost of certain limitations and some degree of **vendor lock-in**, first because Google offers a mix of standard (JSP/servlets, JPA, JDO, JCache/JSR-107 if you choose Java) and proprietary APIs, and second because you are required to adapt your relational mind and design your application to cope with the pros and cons of their non-relational storage system (the Datastore), from where it is really hard to escape.







One of the most appealing features is that GAE offers a real **pay-as-you-go model** because you pay for the computational resources you use (not for running instances), even with reasonably free quotas. 






### The Empire Strikes Back





Yes, GAE is free up to a certain level of used resources. This makes it a perfect fit for personal projects or small projects that plan to grow (i.e. startups). In fact we use GAE in Gather Estudios to offer a [software as a service survey manager](http://precision.gatherestudios.es/), where GAE capacity to handle bursting has proven itself to work great.







That was until last week. On Aug 31, Google communicated (see [blog post](http://googleappengine.blogspot.com/2011/08/50-credit-for-new-billing-signups-and.html)) that the GAE "preview" is over. This includes lowering the free quotas for all applications, and assuming that "almost all applications will be billed more under the new pricing", mainly because from now we are going to pay for every running instance, no matters wether you use it or not. You start to worry when you compare the existing cost to the new costs and you realize the new pricing means a **5-10X increase in your monthly bill**. Well... at least they kindly give us 2 weeks to adapt to the new pricing model.







Google, this is not serious. This time you are setting a very dangerous precedent, I've learned a valuable lesson and I am definitely going to start looking into other **alternatives** such as [CloudFoundry](http://www.cloudfoundry.com/), [Openshift](https://openshift.redhat.com) or [Amazon Elastic Beanstalk](http://aws.amazon.com/elasticbeanstalk/) or even Heroku.






### My advice to Google





Don't screw with developers. You'll regret.

