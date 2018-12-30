---
author: admin
date: 2014-01-02 14:00:00+00:00
draft: false
title: Give your configuration some REST
type: post
url: /blog/2014/01/02/a-rest-configuration-server/
categories:
- Software
- Tecnología
tags:
- ab testing
- confidence
- configuration
- micro services
- nodejs
- rest
- server
- service directory
---

I have built a simple **configuration server** to expose your app's configuration as a REST service. Its name is **rest-confidence** ([github](https://github.com/palmerabollo/rest-confidence)). In this post I will try to explain its basics and three use cases where it could be useful:

1. To configure distributed services.
2. As a foundation for A/B testing.
3. As a simple service directory.

### Install and run a basic rest-confidence configuration server

The first step is installing the configuration server:

    git clone https://github.com/palmerabollo/rest-confidence.git
    cd rest-confidence
    npm install

After that, you are ready to edit your _`config.json`_ configuration file. For example:

{{< highlight json >}}
{
  "mongodb": {
    "host": "localhost",
    "user": "root"
  },
  "redis": {
    "host": "redis-server",
    "port": 6379
  },
  "logging": {
    "appender": {
      "type": "file",
      "filename": "log_file.log",
      "maxSize": 10240
    }
  }
}
{{< / highlight >}}



Launch the configuration server (`npm start`) and you are done. You are now ready to start retrieving the values associated with any key, in a **hierarchical way**:

    # curl http://localhost:8000/logging/appender
    {"type":"file","filename":"log_file.log","maxSize":10240}

or

    # curl http://localhost:8000/logging/appender/maxSize
    10240

### Use case #1: Configure distributed services

In my last post I wrote about [why I like nodejs](http://guidogarcia.net/blog/2013/12/09/why-is-nodejs-so-cool-from-a-java-guy/), a great platform for building [micro-service-based architectures](http://yobriefca.se/blog/2013/04/28/micro-service-architecture/). However, these kind of architectures also come with their own drawbacks. One of them is that they are **more difficult to deploy and configure**.

With a centralized configuration server such as rest-confidence everything becomes easier. Instead of configuring hundreds of settings on each component, you only need to configure the URL of your configuration server. Your service will go there to look up any configuration property it needs.

### Use case #2: A/B testing

[A/B testing](http://en.wikipedia.org/wiki/A/B_testing) is a simple way to test different changes to your application and determine which ones produce positive results.

As a simplistic example, imagine you want to test an alternative color for your blue sign-up button, and check how it affects the conversion rate. You can define a `$filter` with a `$range` limit in your configuration:

{{< highlight json >}}
{
  "color": {
    "$filter": "random",
    "$range": [
      { "limit": 10, "value": "red" }
    ],
    "$default": "blue"
  }
}
{{< / highlight >}}

So when you retrieve the "color" property value using a random filtering criteria, you'll get different colors depending on the ranges.

    # curl http://localhost:8000/?random=5
    {"color":"red"}

And with a different filtering value out of the range you will get the default value.

    # curl http://localhost:8000/?random=15
    {"color":"blue"}

### Use case #3: Simple service directory

You can use [rest-confidence](https://github.com/palmerabollo/rest-confidence) as a simple **service directory**,  that is, a centralized server that facilitates dynamic location of other services' endpoints, based on different criteria.

{{< highlight json >}}
{
  "myservice": {
    "$filter": "env",
    "production": {
      "url": {
        "$filter": "country",
        "ES": "http://myservice-production.es",
        "UK": "http://myservice-production.co.uk",
        "$default": "http://myservice-production.co.uk"
      },
    },
    "development": {
      "url": "http://myservice-production.com"
    }
  }
}
{{< / highlight >}}

With some criteria applied (for example, env=production and country=ES) you will get the proper service endpoint, or any other information you need:

    # curl http://localhost:8000/myservice?country=ES&env=production
    {"url":"http://myservice-production.es"}

I hope you find it useful. There is also a [nodejs client](https://github.com/palmerabollo/rest-confidence-client). Contributions are welcome.
