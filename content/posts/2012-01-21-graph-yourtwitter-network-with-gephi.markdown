---
author: admin
date: 2012-01-21 09:37:14+00:00
draft: false
title: Graph your Twitter network with Gephi
type: post
url: /blog/2012/01/21/graph-yourtwitter-network-with-gephi/
categories:
- Software
- Tecnología
tags:
- gephi
- gephi toolkit
- graph
- java
- neo4j
- twitter
---

[Gephi](http://gephi.org/) is a really cool open-source (GPL) project for visualizing and analyzing network graphs.

### Getting started

If you want to start using Gephi you have two choices:

* The blue pill, a simple **GUI**, pretty easy to use, that offers many network metrics, statistical algorithms (clustering, etc) to analyze your own graphs. The story ends.
* The red pill, the **[Gephi Toolkit](http://wiki.gephi.org/index.php/Toolkit_portal)**. The toolkit is a standard Java library that can be integrated with your own code if you need to analyze graphs. That is what we need.

Both of them are really modular, and can also be extended with big variety of available [plugins](https://gephi.org/plugins/), contributed by third party developers.

### Crawling your Twitter network

Twitter offers a [REST API](https://dev.twitter.com/docs/api). In this case I have used [Spring Social](http://www.springsource.org/spring-social), an extension of the Spring Framework that simplifies the connection with social networks such as Linkedin, Facebook or Twitter ([docs](http://static.springsource.org/spring-social-twitter/docs/1.0.x/reference/html/)).

For example, obtaining the list of followers of a given user is something as simple as:

{{< highlight java >}}
TwitterTemplate twitter = new TwitterTemplate();
List<TwitterProfile> result =
		twitter.friendOperations().getFollowers("palmerabollo");
{{< / highlight >}}

The bad news is that the API is [rate limited](https://dev.twitter.com/docs/rate-limiting) to 150 requests/hour, and you can increase it if you use OAuth to authenticate your requests. This limit is too low (I think Twitter is trying to protect their data from being extracted) and forced me to introduce a basic cache layer (just a `Map`) to save some requests. The cache is coupled with the application code, it could be certainly improved.

### Final result

You can find the [code at github](https://github.com/palmerabollo/test-twitter-graph). Remember that this is a proof of concept, so it can be improved a lot. I am waiting for your pull requests.

![Gephi Twitter](/images/gephi_twitter-300x247.png)

### Lessons learned and future work
* Twitter API is very restrictive. A cache tries to solve this problem but it is still not possible to retrieve the deepest levels of your network. It would be fun to use a NOSQL graph database such as [neo4j](http://neo4j.org/), that even has a [Gephi plugin](https://gephi.org/plugins/neo4j-graph-database-support/) available, to store the data. [This question](http://forum.gephi.org/viewtopic.php?t=1606) I asked in the Gephi Forum is a good starting point if you are interested on it.
* Gephi Toolkit and its plugins are not available from a Maven repository, so I included them as system libraries in my [pom.xml](https://github.com/palmerabollo/test-twitter-graph/blob/master/pom.xml). This is the first time I do it, and it is probably a bad practice. I would like to hear your opinion about this.
* Gephi is a really interesting and powerful project, but the documentation and examples could be improved.
* It would be very interesting to play with other Gephi advanced features such as filtering, clustering. For example, to detect "communities" or to detect the most influential nodes in your network.
* Spring Social simplifies your life, offering a common interface to work with different social networks, saving you the pain and hassle of dealing with every different API out there.

By the way, if anyone else is interested in offering this service through a web interface, please let me know.
