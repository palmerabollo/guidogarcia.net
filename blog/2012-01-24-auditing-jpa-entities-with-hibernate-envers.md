---
author: admin
date: 2012-01-24 23:06:26+00:00
draft: false
title: Auditing JPA entities with Hibernate Envers
type: post
url: /blog/2012/01/25/auditing-jpa-entities-with-hibernate-envers/
categories:
- Software
- Tecnología
tags:
- auditable
- database
- hibernate
- java
- jpa
---

In a recent project we had the need to keep a record of the changes made in every domain entity. That is, we need to insert a new record on an audit table every time an entity is created/updated/deleted.

This is not the first time this problem arises. I think this is a the case of a [cross-cutting concern](http://en.wikipedia.org/wiki/Cross-cutting_concern), that fits well into the [aspect-oriented programming](http://en.wikipedia.org/wiki/Aspect-oriented_programming) area. I have seen this very same problem in many different projects I have worked on, always solved with one variant of these two approches:




	  * Use a database trigger, that fires with every change in the entities you need to track.
	  * Manage it programmatically, a painful solution that contaminates your code.




### Hibernate Envers to the rescue



The Envers project "aims to enable easy auditing of persistent classes. All that you have to do is annotate your persistent class or some of its properties, that you want to keep track of, with **@Audited**. For each audited entity, a table will be created, which will hold the history of changes made to the entity".

The [Envers project](http://www.jboss.org/envers) is now a [Hibernate](http://hibernate.org/) core module (since 3.5+), so it is really easy to include it in your projects. As promised, you only need to annotate your entities:


    
    
    @Entity
    @Audited
    public class MyEntity {
       ...
    }
    



If you are using Hibernate Hibernate 3.x, add the following properties to your **persistence.xml**, to configure the listeners that need to be called when a persistence related event is fired:


    
    
    <property name="hibernate.ejb.event.post-insert" value="org.hibernate.ejb.event.EJB3PostInsertEventListener,org.hibernate.envers.event.AuditEventListener" />
    <property name="hibernate.ejb.event.post-update" value="org.hibernate.ejb.event.EJB3PostUpdateEventListener,org.hibernate.envers.event.AuditEventListener" />
    <property name="hibernate.ejb.event.post-delete" value="org.hibernate.ejb.event.EJB3PostDeleteEventListener,org.hibernate.envers.event.AuditEventListener" />
    <property name="hibernate.ejb.event.pre-collection-update" value="org.hibernate.envers.event.AuditEventListener" />
    <property name="hibernate.ejb.event.pre-collection-remove" value="org.hibernate.envers.event.AuditEventListener" />
    <property name="hibernate.ejb.event.post-collection-recreate" value="org.hibernate.envers.event.AuditEventListener" />
    



And last, if you are using Maven, include it as a dependency in your pom.xml (the [dependency scope is "provided" if you use JBoss AS 7.0.2](http://planet.jboss.org/view/post.seam?post=envers_bundled_with_jboss_as_7_0_2)):


    
    
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-envers</artifactId>
        <version>${envers.version}</version> <-- i.e. 3.6.8.Final -->
    </dependency>
    



That is all. Every change in an entity will be audited in an automatically generated "player_aud" table. This is the simplest and, most important, cleanest way I currently know to solve the aforementioned problem.

The [official docs](http://docs.jboss.org/envers/docs/index.html) include other features such as logging, querying historical data, advanced configuration and some unsupported features (bags).



### Any critics?



Hibernate Envers is so simple that the only improvement I can think about is to see it standardized as part of the next version of JPA, and to include a single configuration property covering 90% of the cases (or even better, discover if the entities are marked as auditable at runtime):


    
    
    <property name="hibernate.auditable" value="true" /> 
    



Any comment is welcome.

**UPDATE**: With Hibernate 4.x it is no longer necessary to add the event listeners to your Hibernate configuration. All you need is to put hibernate-envers on your classpath and annotate your entities.
