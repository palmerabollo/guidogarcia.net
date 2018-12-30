---
author: admin
date: 2011-08-28 18:01:35+00:00
draft: false
title: To ORM or not to ORM
type: post
url: /blog/2011/08/28/to-orm-or-not-to-orm/
categories:
- Software
- Tecnología
tags:
- hibernate
- jdbc
- jdo
- jpa
- mybatis
- orm
- sql
---


In the last days I have had a couple of interesting discussions about wether to include an ORM library (such as Hibernate JPA implementation) in an application is a good idea or not.

In this case, as usual, I think there is not a single best choice for every scenario, so I'll try to list pros (and cons) for each alternative.

## Use an ORM layer

* Easy to develop and maintain. You only annotate your domain model classes with standard JPA/JDO annotations that, at the same time, serve as some kind of documentation. So you can write less code, which means more productivity and probably less bugs.
* Database independence. You can change your DBMS (i.e. from MySQL to Oracle) and keep you code untouched. Sounds interesting, but I have to admit that I've not seen this need in more than 10 years of professional experience.
* State management. Automatic synchronization of the state of your in-memory [domain model](http://martinfowler.com/eaaCatalog/domainModel.html) with a relational database

I could say this would be my default choice in a new project without any special requirements.

## Use plain JDBC

* Greater degree of control, because you write your own SQL sentences by hand
* A shallow learning curve (if you already know SQL)
* More efficient because you can only retrieve the data you need and because you (or a DBA) can fine-tune the precise SQL sentences. I don't fully agree with this argument because an ORM layer also includes many other improvements such as caching, lazy loading, etc.
* At the end, you also have to write sentences if you use an ORM (i.e. JPQL with JPA)

I only recommend this alternative in those few cases where you have extreme performance requirements and/or an experienced DBA in your team.

## Use a mixed model

Some years ago I used iBatis (now [mybatis](http://www.mybatis.org/) as a SQL mapping framework that comes with a great documentation and other niceties such as caching mechanisms, ability to build dynamic queries on the fly, etc.

It was great, but at the end you were forced to maintain a XML file with a bunch of SQL queries and the mappings between your classes and your database structure. With mybatis 3 things seem to have changed: you can stop using an external XML file and use Java annotations instead. They are not standard as far as I know.

I have no experience with it, so I don't really know wether it is a valid alternative or not.

**Extra point**: One of my coworkers said "don't ever let hibernate to create your database schema".  Literally. Do you have any reason to think it is not a good idea?

The post is open to any contribution.
