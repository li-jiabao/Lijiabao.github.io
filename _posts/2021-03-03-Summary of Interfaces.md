---
title: Summary of Interfaces
author: lijiabao
date: 2020-12-06 01:52:47.569930300 +0800
category: Collections
categories: Collections
tags: Collections
---

# Summary of Interfaces

The core collection interfaces are the foundation of the Java Collections Framework.

The Java Collections Framework hierarchy consists of two distinct interface trees:

- The first tree starts with the `Collection` interface, which provides for the basic functionality used by all collections, such as `add` and `remove` methods. Its subinterfaces &#151; `Set`, `List`, and `Queue` &#151; provide for more specialized collections.
- The `Set` interface does not allow duplicate elements. This can be useful for storing collections such as a deck of cards or student records. The `Set` interface has a subinterface, `SortedSet`, that provides for ordering of elements in the set.
- The `List` interface provides for an ordered collection, for situations in which you need precise control over where each element is inserted. You can retrieve elements from a `List` by their exact position.
- The `Queue` interface enables additional insertion, extraction, and inspection operations. Elements in a `Queue` are typically ordered in on a FIFO basis.
- The `Deque` interface enables insertion, deletion, and inspection operations at both the ends. Elements in a `Deque` can be used in both LIFO and FIFO.
- The second tree starts with the `Map` interface, which maps keys and values similar to a `Hashtable`.
<li>`Map`'s subinterface, `SortedMap`, maintains its key-value pairs in ascending order or in an order specified by a `Comparator`.
</li>

These interfaces allow collections to be manipulated independently of the details of their representation.
