---
title: Defining Simple Generics
author: lijiabao
date: 2020-12-06 01:38:09.409993900 +0800
category: Bonus
categories: Bonus
tags: Bonus
---

# Generics and Subtyping

Let's test your understanding of generics. Is the following code snippet legal?

```

List&lt;String&gt; ls = new ArrayList&lt;String&gt;(); // 1
List&lt;Object&gt; lo = ls; // 2 

```

Line 1 is certainly legal. The trickier part of the question is line 2. This boils down to the question: is a `List` of `String` a `List` of `Object`. Most people instinctively answer, "Sure!"

Well, take a look at the next few lines:

```

lo.add(new Object()); // 3
String s = ls.get(0); // 4: Attempts to assign an Object to a String!

```

Here we've aliased `ls` and `lo`. Accessing `ls`, a list of `String`, through the alias `lo`, we can insert arbitrary objects into it. As a result `ls` does not hold just `String`s anymore, and when we try and get something out of it, we get a rude surprise.

The Java compiler will prevent this from happening of course. Line 2 will cause a compile time error.

In general, if `Foo` is a subtype (subclass or subinterface) of `Bar`, and `G` is some generic type declaration, it is **not** the case that `G&lt;Foo&gt;` is a subtype of `G&lt;Bar&gt;`. This is probably the hardest thing you need to learn about generics, because it goes against our deeply held intuitions.

We should not assume that collections don't change. Our instinct may lead us to think of these things as immutable.

For example, if the department of motor vehicles supplies a list of drivers to the census bureau, this seems reasonable. We think that a `List&lt;Driver&gt;` is a `List&lt;Person&gt;`, assuming that `Driver` is a subtype of `Person`. In fact, what is being passed is a **copy** of the registry of drivers. Otherwise, the census bureau could add new people who are not drivers into the list, corrupting the DMV's records.

To cope with this sort of situation, it's useful to consider more flexible generic types. The rules we've seen so far are quite restrictive.
