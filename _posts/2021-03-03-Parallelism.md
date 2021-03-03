---
title: Parallelism
author: lijiabao
date: 2020-12-06 01:52:55.339844400 +0800
category: Collections
categories: Collections
tags: Collections
---

# Parallelism

Parallel computing involves dividing a problem into subproblems, solving those problems simultaneously (in parallel, with each subproblem running in a separate thread), and then combining the results of the solutions to the subproblems. Java SE provides the
[fork/join framework](../../essential/concurrency/forkjoin.html), which enables you to more easily implement parallel computing in your applications. However, with this framework, you must specify how the problems are subdivided (partitioned). With aggregate operations, the Java runtime performs this partitioning and combining of solutions for you.

One difficulty in implementing parallelism in applications that use collections is that collections are not thread-safe, which means that multiple threads cannot manipulate a collection without introducing
[thread interference](../../essential/concurrency/interfere.html) or
[memory consistency errors](../../essential/concurrency/memconsist.html). The Collections Framework provides 
[synchronization wrappers](../../collections/implementations/wrapper.html), which add automatic synchronization to an arbitrary collection, making it thread-safe. However, synchronization introduces
[thread contention](../../essential/concurrency/sync.html#thread_contention). You want to avoid thread contention because it prevents threads from running in parallel. Aggregate operations and parallel streams enable you to implement parallelism with non-thread-safe collections provided that you do not modify the collection while you are operating on it.

Note that parallelism is not automatically faster than performing operations serially, although it can be if you have enough data and processor cores. While aggregate operations enable you to more easily implement parallelism, it is still your responsibility to determine if your application is suitable for parallelism.

This section covers the following topics:

  - [Executing Streams in Parallel](#executing_streams_in_parallel)
  - [Concurrent Reduction](#concurrent_reduction)  
  - [Ordering](#ordering)
  <li>[Side Effects](#side_effects)
    <ul>
      - [Laziness](#laziness)
      - [Interference](#interference)
      - [Stateful Lambda Expressions](#stateful_lambda_expressions)
    
You can find the code excerpts described in this section in the example
[`ParallelismExamples`](examples/ParallelismExamples.java).

## <a name="executing_streams_in_parallel" id="executing_streams_in_parallel">Executing Streams in Parallel</a>

You can execute streams in serial or in parallel. When a stream executes in parallel, the Java runtime partitions the stream into multiple substreams. Aggregate operations iterate over and process these substreams in parallel and then combine the results.

When you create a stream, it is always a serial stream unless otherwise specified. To create a parallel stream, invoke the operation
[`Collection.parallelStream`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#parallelStream--). Alternatively, invoke the operation
[`BaseStream.parallel`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#parallel--). For example, the following statement calculates the average age of all male members in parallel:

```
Collector.Characteristics.CONCURRENT</code></a>. To determine the characteristics of a collector, invoke the
[`Collector.characteristics`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.Characteristics.html) method.</li>
  
  <li>Either the stream is unordered, or the collector has the characteristic
[`Collector.Characteristics.UNORDERED`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.Characteristics.html#UNORDERED). To ensure that the stream is unordered, invoke the
[`BaseStream.unordered`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/BaseStream.html#unordered--) operation.</li>
  
</ul>

<p>**Note**: This example returns an instance of
[`ConcurrentMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html) instead of `Map` and invokes the
[`groupingByConcurrent`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#groupingByConcurrent-java.util.function.Function-)  operation instead of `groupingBy`. (See the section
[Concurrent Collections](../../essential/concurrency/collections.html) for more information about `ConcurrentMap`.) Unlike the operation `groupingByConcurrent`, the operation `groupingBy` performs poorly with parallel streams. (This is because it operates by merging two maps by key, which is computationally expensive.) Similarly, the operation
[`Collectors.toConcurrentMap`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toConcurrentMap-java.util.function.Function-java.util.function.Function-) performs better with parallel streams than the operation
[`Collectors.toMap`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toMap-java.util.function.Function-java.util.function.Function-).</p>

<!-- *********************************************************************** -->

<h2><a name="ordering" id="ordering">Ordering</a></h2>

The order in which a pipeline processes the elements of a stream depends on whether the stream is executed in serial or in parallel, the source of the stream, and intermediate operations. For example, consider the following example that prints the elements of an instance of `ArrayList` with the `forEach` operation several times:

<pre class="codeblock">Integer[] intArray = {1, 2, 3, 4, 5, 6, 7, 8 };
List&lt;Integer&gt; listOfIntegers =
    new ArrayList&lt;&gt;(Arrays.asList(intArray));

System.out.println("listOfIntegers:");
listOfIntegers
    .stream()
    .forEach(e -&gt; System.out.print(e + " "));
System.out.println("");

System.out.println("listOfIntegers sorted in reverse order:");
Comparator&lt;Integer&gt; normal = Integer::compare;
Comparator&lt;Integer&gt; reversed = normal.reversed(); 
Collections.sort(listOfIntegers, reversed);  
listOfIntegers
    .stream()
    .forEach(e -&gt; System.out.print(e + " "));
System.out.println("");
     
System.out.println("Parallel stream");
listOfIntegers
    .parallelStream()
    .forEach(e -&gt; System.out.print(e + " "));
System.out.println("");
    
System.out.println("Another parallel stream:");
listOfIntegers
    .parallelStream()
    .forEach(e -&gt; System.out.print(e + " "));
System.out.println("");
     
System.out.println("With forEachOrdered:");
listOfIntegers
    .parallelStream()
    .forEachOrdered(e -&gt; System.out.print(e + " "));
System.out.println("");
```

## <a name="ordering" id="ordering">Ordering</a>

This example consists of five pipelines. It prints output similar to the following:

```
Collections.sort</code></a>.</li>
  
  - The third and fourth pipelines print the elements of the list in an apparently random order. Remember that stream operations use internal iteration when processing elements of a stream. Consequently, when you execute a stream in parallel, the Java compiler and runtime determine the order in which to process the stream's elements to maximize the benefits of parallel computing unless otherwise specified by the stream operation.
  
  <li>The fifth pipeline uses the method
[`forEachOrdered`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#forEachOrdered-java.util.function.Consumer-), which processes the elements of the stream in the order specified by its source, regardless of whether you executed the stream in serial or parallel. Note that you may lose the benefits of parallelism if you use operations like `forEachOrdered` with parallel streams.</li>
</ul>

<!-- *********************************************************************** -->

<h2><a name="side_effects" id="side_effects">Side Effects</a></h2>

<p>A method or an expression has a side effect if, in addition to returning or producing a value, it also modifies the state of the computer. Examples include  mutable reductions (operations that use the `collect` operation; see the section
[Reduction](../../collections/streams/reduction.html) for more information) as well as invoking the `System.out.println` method for debugging. The JDK handles certain side effects in pipelines well. In particular, the `collect` method is designed to perform the most common stream operations that have side effects in a parallel-safe manner. Operations like `forEach` and `peek` are designed for side effects; a lambda expression that returns void, such as one that invokes `System.out.println`, can do nothing but have side effects. Even so, you should use the `forEach` and `peek` operations with care; if you use one of these operations with a parallel stream, then the Java runtime may invoke the lambda expression that you specified as its parameter concurrently from multiple threads. In addition, never pass as parameters lambda expressions that have side effects in operations such as `filter` and `map`. The following sections discuss [interference](#interference) and [stateful lambda expressions](#stateful_lambda_expressions), both of which can be sources of side effects and can return inconsistent or unpredictable results, especially in parallel streams. However, the concept of [laziness](#laziness) is discussed first, because it has a direct effect on interference.</p>

<!-- *********************************************************************** -->

<h3><a name="laziness" id="laziness">Laziness</a></h3>
 
<p>All intermediate operations are **lazy**. An expression, method, or algorithm is lazy if its value is evaluated only when it is required. (An algorithm is **eager** if it is evaluated or processed immediately.) Intermediate operations are lazy because they do not start processing the contents of the stream until the terminal operation commences. Processing streams lazily enables the Java compiler and runtime to optimize how they process streams. For example, in a pipeline such as the `filter`-`mapToInt`-`average` example described in the section
[Aggregate Operations](../../collections/streams/index.html), the `average` operation could obtain the first several integers from the stream created by the `mapToInt` operation, which obtains elements from the `filter` operation. The `average` operation would repeat this process until it had obtained all required elements from the stream, and then it would calculate the average.</p>

<!-- *********************************************************************** -->

<h3><a name="interference" id="interference">Interference</a></h3>

Lambda expressions in stream operations should not **interfere**. Interference occurs when the source of a stream is modified while a pipeline processes the stream.  For example, the following code attempts to concatenate the strings contained in the `List` `listOfStrings`. However, it throws a `ConcurrentModificationException`:

<pre class="codeblock">try {
    List&lt;String&gt; listOfStrings =
        new ArrayList&lt;&gt;(Arrays.asList("one", "two"));
         
    // This will fail as the peek operation will attempt to add the
    // string "three" to the source after the terminal operation has
    // commenced. 
             
    String concatenatedString = listOfStrings
        .stream()
        
        // Don't do this! Interference occurs here.
        .peek(s -&gt; listOfStrings.add("three"))
        
        .reduce((a, b) -&gt; a + " " + b)
        .get();
                 
    System.out.println("Concatenated string: " + concatenatedString);
         
} catch (Exception e) {
    System.out.println("Exception caught: " + e.toString());
}
```

### <a name="laziness" id="laziness">Laziness</a>

This example concatenates the strings contained in `listOfStrings` into an `Optional&lt;String&gt;` value with the `reduce` operation, which is a terminal operation. However, the pipeline here invokes the intermediate operation `peek`, which attempts to add a new element to `listOfStrings`. Remember, all intermediate operations are lazy. This means that the pipeline in this example begins execution when the operation `get` is invoked, and ends execution when the `get` operation completes. The argument of the `peek` operation attempts to modify the stream source during the execution of the pipeline, which causes the Java runtime to throw a `ConcurrentModificationException`.

### <a name="stateful_lambda_expressions" id="stateful_lambda_expressions">Stateful Lambda Expressions</a>

Avoid using **stateful lambda expressions** as parameters in stream operations. A stateful lambda expression is one whose result depends on any state that might change during the execution of a pipeline. The following example adds elements from the `List` `listOfIntegers` to a new `List` instance with the `map` intermediate operation. It does this twice, first with a serial stream and then with a parallel stream:

```
synchronizedList</code></a> so that the `List` `parallelStorage` is thread-safe. Remember that collections are not thread-safe. This means that multiple threads should not access a particular collection at the same time. Suppose that you do not invoke the method `synchronizedList` when creating `parallelStorage`:</p> 

<pre class="codeblock">List&lt;Integer&gt; parallelStorage = new ArrayList&lt;&gt;();
```

The example behaves erratically because multiple threads access and modify `parallelStorage` without a mechanism like synchronization to schedule when a particular thread may access the `List` instance. Consequently, the example could print output similar to the following:
