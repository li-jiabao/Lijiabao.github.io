
# Reduction

The section
[Aggregate Operations](../../collections/streams/index.html) describes the following pipeline of operations, which calculates the average age of all male members in the collection `roster`:

```
average</code></a>,
[`sum`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#sum--),
[`min`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#min-java.util.Comparator-),
[`max`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#max-java.util.Comparator-), and
[`count`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#count--)) that return one value by combining the contents of a stream. These operations are called **reduction operations**. The JDK also contains reduction operations that return a collection instead of a single value. Many reduction operations perform a specific task, such as finding the average of values or grouping elements into categories. However, the JDK provides you with the general-purpose reduction operations
[`reduce`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#reduce-T-java.util.function.BinaryOperator-) and
[`collect`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#collect-java.util.function.Supplier-java.util.function.BiConsumer-java.util.function.BiConsumer-), which this section describes in detail.</p>

This section covers the following topics:

<ul>
  - [The Stream.reduce Method](#reduce)
  - [The Stream.collect Method](#collect)
</ul>

<p>You can find the code excerpts described in this section in the example
[`ReductionExamples`](examples/ReductionExamples.java).</p>

<h2><a name="reduce" id="reduce">The Stream.reduce Method</a></h2>

<p>The
[`Stream.reduce`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#reduce-T-java.util.function.BinaryOperator-) method is a general-purpose reduction operation. Consider the following pipeline, which calculates the sum of the male members' ages in the collection `roster`. It uses the
[`Stream.sum`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#sum--) reduction operation:</p>

<pre class="codeblock">Integer totalAge = roster
    .stream()
    .mapToInt(Person::getAge)
    .sum();
```

Compare this with the following pipeline, which uses the `Stream.reduce` operation to calculate the same value:

```
identity</code>: The identity element is both the initial value of the reduction and the default result if there are no elements in the stream. In this example, the identity element is `0`; this is the initial value of the sum of ages and the default value if no members exist in the collection `roster`.</p></li>
  
  <li>`accumulator`: The accumulator function takes two parameters: a partial result of the reduction (in this example, the sum of all processed integers so far) and the next element of the stream (in this example, an integer). It returns a new partial result. In this example, the accumulator function is a lambda expression that adds two `Integer` values and returns an `Integer` value:
  
  <pre class="codeblock">(a, b) -&gt; a + b
```

  - `identity`: The identity element is both the initial value of the reduction and the default result if there are no elements in the stream. In this example, the identity element is `0`; this is the initial value of the sum of ages and the default value if no members exist in the collection `roster`.
  
  <li>`accumulator`: The accumulator function takes two parameters: a partial result of the reduction (in this example, the sum of all processed integers so far) and the next element of the stream (in this example, an integer). It returns a new partial result. In this example, the accumulator function is a lambda expression that adds two `Integer` values and returns an `Integer` value:
  
  <pre class="codeblock">(a, b) -&gt; a + b</code></pre></li>


The `reduce` operation always returns a new value. However, the accumulator function also returns a new value every time it processes an element of a stream. Suppose that you want to reduce the elements of a stream to a more complex object, such as a collection. This might hinder the performance of your application. If your `reduce` operation involves adding elements to a collection, then every time your accumulator function processes an element, it creates a new collection that includes the element, which is inefficient. It would be more efficient for you to update an existing collection instead. You can do this with the
[`Stream.collect`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#collect-java.util.function.Supplier-java.util.function.BiConsumer-java.util.function.BiConsumer-) method, which the next section describes.

## <a name="collect" id="collect">The Stream.collect Method</a>

Unlike the `reduce` method, which always creates a new value when it processes an element, the
[`collect`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#collect-java.util.function.Supplier-java.util.function.BiConsumer-java.util.function.BiConsumer-) method modifies, or mutates, an existing value.

Consider how to find the average of values in a stream. You require two pieces of data: the total number of values and the sum of those values. However, like the `reduce` method and all other reduction methods, the `collect` method returns only one value. You can create a new data type that contains member variables that keep track of the total number of values and the sum of those values, such as the following class,
[`Averager`](examples/Averager.java):

```
supplier</code>: The supplier is a factory function; it constructs new instances. For the `collect` operation, it creates instances of the result container. In this example, it is a new instance of the `Averager` class.</li>
  
  - `accumulator`: The accumulator function incorporates a stream element into a result container. In this example, it modifies the `Averager` result container by incrementing the `count` variable by one and adding to the `total` member variable the value of the stream element, which is an integer representing the age of a male member.
  
  - `combiner`: The combiner function takes two result containers and merges their contents. In this example, it modifies an  `Averager` result container by incrementing the `count` variable by the `count` member variable of the other `Averager` instance and adding to the `total` member variable the value of the other `Averager` instance's `total` member variable.</ul>
  
Note the following:

<ul>
  - The supplier is a lambda expression (or a method reference) as opposed to a value like the identity element in the `reduce` operation.
  
  - The accumulator and combiner functions do not return a value.
  
  <li>You can use the `collect` operations with parallel streams; see the section
[Parallelism](../../collections/streams/parallelism.html) for more information. (If you run the `collect` method with a parallel stream, then the JDK creates a new thread whenever the combiner function creates a new object, such as an `Averager` object in this example. Consequently, you do not have to worry about synchronization.)</li>  
  
</ul>

Although the JDK provides you with the `average` operation to calculate the average value of elements in a stream, you can use the `collect` operation and a custom class if you need to calculate several values from the elements of a stream.

The `collect` operation is best suited for collections. The following example puts the names of the male members in a collection with the `collect` operation:

<pre class="codeblock">List&lt;String&gt; namesOfMaleMembersCollect = roster
    .stream()
    .filter(p -&gt; p.getGender() == Person.Sex.MALE)
    .map(p -&gt; p.getName())
    .collect(Collectors.toList());
```

  - The supplier is a lambda expression (or a method reference) as opposed to a value like the identity element in the `reduce` operation.
  
  - The accumulator and combiner functions do not return a value.
  
  <li>You can use the `collect` operations with parallel streams; see the section
[Parallelism](../../collections/streams/parallelism.html) for more information. (If you run the `collect` method with a parallel stream, then the JDK creates a new thread whenever the combiner function creates a new object, such as an `Averager` object in this example. Consequently, you do not have to worry about synchronization.)</li>  
  

This version of the `collect` operation takes one parameter of type
[`Collector`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html). This class encapsulates the functions used as arguments in the `collect` operation that requires three arguments (supplier, accumulator, and combiner functions).

The
[`Collectors`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html) class contains many useful reduction operations, such as accumulating elements into collections and summarizing elements according to various criteria. These reduction operations return instances of the class `Collector`, so you can use them as a parameter for the `collect` operation.

This example uses the
[`Collectors.toList`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toList--) operation, which accumulates the stream elements into a new instance of `List`. As with most operations in the `Collectors` class, the `toList` operator returns an instance of `Collector`, not a collection.

The following example groups members of the collection `roster` by gender:

```
groupingBy</code></a> operation returns a map whose keys are the values that result from applying the lambda expression specified as its parameter (which is called a **classification function**). In this example, the returned map contains two keys, `Person.Sex.MALE` and `Person.Sex.FEMALE`. The keys' corresponding values are instances of `List` that contain the stream elements that, when processed by the classification function, correspond to the key value. For example, the value that corresponds to key `Person.Sex.MALE` is an instance of `List` that contains all male members.</p>

The following example retrieves the names of each member in the collection `roster` and groups them by gender:

<pre class="codeblock">Map&lt;Person.Sex, List&lt;String&gt;&gt; namesByGender =
    roster
        .stream()
        .collect(
            Collectors.groupingBy(
                Person::getGender,                      
                Collectors.mapping(
                    Person::getName,
                    Collectors.toList())));
```

The
[`groupingBy`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#groupingBy-java.util.function.Function-java.util.stream.Collector-) operation in this example takes two parameters, a classification function and an instance of `Collector`. The `Collector` parameter is called a **downstream collector**. This is a collector that the Java runtime applies to the results of another collector. Consequently, this `groupingBy` operation enables you to apply a `collect` method to the `List` values created by the `groupingBy` operator. This example applies the collector 
[`mapping`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#mapping-java.util.function.Function-java.util.stream.Collector-java.util.stream.Collector-), which applies the mapping function `Person::getName` to each element of the stream. Consequently, the resulting stream consists of only the names of members. A pipeline that contains one or more downstream collectors, like this example, is called a **multilevel reduction**.

The following example retrieves the total age of members of each gender:

```
reducing</code></a> operation takes three parameters:</p>

<ul>
  - `identity`: Like the `Stream.reduce` operation, the identity element is both the initial value of the reduction and the default result if there are no elements in the stream. In this example, the identity element is `0`; this is the initial value of the sum of ages and the default value if no members exist.
  
  - `mapper`: The `reducing` operation applies this mapper function to all stream elements. In this example, the mapper retrieves the age of each member.
  
  - `operation`: The operation function is used to reduce the mapped values. In this example, the operation function adds `Integer` values.
</ul>

The following example retrieves the average age of members of each gender:

<pre class="codeblock">Map&lt;Person.Sex, Double&gt; averageAgeByGender = roster
    .stream()
    .collect(
        Collectors.groupingBy(
            Person::getGender,                      
            Collectors.averagingInt(Person::getAge)));
```
