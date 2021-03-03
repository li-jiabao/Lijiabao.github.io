---
title: The Deque Interface
author: lijiabao
date: 2020-12-06 01:52:38.237256500 +0800
category: Collections
categories: Collections
tags: Collections
---

# The Deque Interface

Usually pronounced as `deck`, a deque is a double-ended-queue. A double-ended-queue is a linear collection 
of elements that supports the insertion and removal of elements at both end points.

The `Deque` interface is a richer abstract data type than both `Stack` and `Queue` because it implements both stacks and queues at the same time.

The 
[`Deque`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html) interface, defines methods to access the elements at both ends of the `Deque` instance. Methods are provided to insert, remove, 
and examine the elements. Predefined classes like 
[`ArrayDeque`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayDeque.html) and 
[` LinkedList`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) implement the `Deque` interface. 


Note that the `Deque` interface can be used both as last-in-first-out stacks and first-in-first-out queues.
The methods given in the `Deque` interface are divided into three parts:

## Insert

The `addfirst` and `offerFirst` methods insert elements at the beginning of the `Deque` instance.
 The methods `addLast` and `offerLast` insert elements at the end of the `Deque` instance.
 When the capacity of the `Deque` instance is restricted, the preferred methods are `offerFirst` and `offerLast` because `addFirst` might fail to throw an exception if it is full.


## Remove

The `removeFirst` and `pollFirst` methods remove elements 
from the beginning of the `Deque` instance. The `removeLast` and `pollLast` methods 
remove elements from the end. The methods `pollFirst` 
and `pollLast` return `null` if the `Deque` is empty whereas the methods 
`removeFirst` and `removeLast` throw an exception if the `Deque` instance is empty. 

## Retrieve

The methods `getFirst` and `peekFirst` retrieve the first element of the `Deque` instance.
These methods dont remove the value from the `Deque` instance. Similarly, the methods `getLast`
and `peekLast` retrieve the last element.
The methods `getFirst` and `getLast` throw an exception if the
`deque` instance is empty whereas the methods `peekFirst` and `peekLast` 
return `NULL`.


The 12 methods for insertion, removal and retieval of Deque elements are summarized in the following table:

    <th id="h1">Type of Operation</th>    <th id="h2">First Element (Beginning of the `Deque` instance)</th>    <th id="h3">Last Element (End of the `Deque` instance)</th>     
    <td headers="h1">**Insert**</td>    <td headers="h2">`addFirst(e)`<br />`offerFirst(e)`</td>    <td headers="h3">`addLast(e)`<br />`offerLast(e)`</td>     
    <td headers="h1">**Remove**</td>    <td headers="h2">`removeFirst()`<br />`pollFirst()`</td>    <td headers="h3">`removeLast()`<br />`pollLast()`</td>  
    <td headers="h1">**Examine**</td>    <td headers="h2">`getFirst()`<br />`peekFirst()`</td>    <td headers="h3">`getLast()`<br />`peekLast()`</td>  

In addition to these basic methods to insert,remove and examine a `Deque` instance, the `Deque` interface also has
some more predefined methods. One of these is `removeFirstOccurence`, this method removes the 
first occurence of the specified element if it exists in the `Deque` instance. If the element does not exist then the `Deque` instance remains unchanged.
Another similar method is `removeLastOccurence`; this method removes the last occurence of the specified element in the `Deque` instance.
The return type of these methods is `boolean`, and they return `true` if the element exists in the `Deque` instance.
