---
author: admin
date: 2011-10-29 13:37:43+00:00
draft: false
title: 'Java : different ways to filter a Collection'
type: post
url: /blog/2011/10/29/java-different-ways-filter-collection/
categories:
- Software
- Tecnología
tags:
- collections
- java
---

Imagine we have a simple Java class:

{{< highlight java >}}
public class Person {
  private int age;
  private Gender sex;

  // constructor, getters & setters
}

enum Gender { MALE, FEMALE };
{{< / highlight >}}

And that you have a `Collection` of `Person` objects, such as the following one:

{{< highlight java >}}
Person p1 = new Person(35, Gender.MALE);
Person p2 = new Person(30, Gender.MALE);
Person p3 = new Person(25, Gender.FEMALE);
Person p4 = new Person(15, Gender.FEMALE);

List<Person> people = Arrays.asList(p1,p2,p3,p4);
{{< / highlight >}}

How would you create a new `Collection` containing only men over 21?

### Use plain Java, like tough people do

{{< highlight java >}}
List<Person> result = new ArrayList<Person>();
for (Person person : people) {
  if (person.getAge() > 21 && person.getGender() == Gender.MALE) {
    result.add(person);
  }
}

// now result contains the filtered collection
{{< / highlight >}}

### Filtering by predicate

The second option is what [Commons Collections](http://commons.apache.org/collections/) and [Guava](http://code.google.com/p/guava-libraries/) propose, and that I think should have been part of the Java core at least since Java 5.

{{< highlight java >}}
public interface Predicate<T> { boolean apply(T type); }

public static <T> Collection<T> filter(Collection<T> col, Predicate<T> predicate) {
  Collection<T> result = new ArrayList<T>();
  for (T element: col) {
    if (predicate.apply(element)) {
      result.add(element);
    }
  }
  return result;
}
{{< / highlight >}}

It remembers me the visitor pattern. With this approach in mind, you can define a set of predicates:

{{< highlight java >}}
Predicate<Person> validPersonPredicate = new Predicate<Person>() {
  public boolean apply(Person person) {
    return person.getAge() > 21 && person.getGender() == Gender.MALE;
  }
};

Collection<Person> result = filter(people, validPersonPredicate);
{{< / highlight >}}

I find it very similar to Python's `filter()` function, but more verbose due to Java's nature.

This way your code becomes cleaner, and you can combine fine-grained reusable predicates. As a future line of work, it would be interesting to define a [fluent interface](http://martinfowler.com/bliki/FluentInterface.html) that allows chaining of several predicates in a more natural way.

### JSR 335, Project Lambda and lambdaj

[JSR 335](http://jcp.org/en/jsr/detail?id=335) (Lambda Expressions for the Java Programming Language) is part of JSR 337 (Java SE 8), which is scheduled for release in late 2012.

Lambda syntax is [already decided](http://mail.openjdk.java.net/pipermail/lambda-dev/2011-September/003936.html) (it is similar to C# and Scala) and what I hope is to see something similar to this kind of filtering, method chaining included:

{{< highlight java >}}
    Collection<Person> result = people
        .filter(p => { return p.age > 21 })
        .filter(p => { return p.gender == Gender.MALE });
{{< / highlight >}}

If you can not wait and need to start using that kind of syntax today, I recommend the [lambdaj library](http://code.google.com/p/lambdaj/), it is not the same but it is handy in some cases:

{{< highlight java >}}
    Collection<Person> result = with(people).clone()
        .retain(having(on(Person.class).getAge(), greaterThan(21)))
        .retain(having(on(Person.class).getGender(), equals(Gender.MALE)));
{{< / highlight >}}

I have not tried the last snippet but you get the idea. Notice you have to call `clone()` if you want to leave the original collection unchanged.

## A small benchmark

As a second line of work, a comparison between the three alternatives is still to be done. I suppose the use of predicates is cleaner at the cost of a lower performance.

Comments are open.
