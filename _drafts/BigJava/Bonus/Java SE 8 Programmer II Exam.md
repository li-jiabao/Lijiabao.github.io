
# Java SE 8 Upgrade Exam

This page maps sections in the Java Tutorials to topics covered in the Upgrade to Java SE 8 OCP (Oracle Certified Professional) (Java SE 6 and all prior versions) and Upgrade Java SE 7 to Java SE 8 OCP exams. These exams are associated with the Oracle Certified Professional, Java SE 8 Programmer certificate.

## Upgrade to Java SE 8 OCP (Java SE 6 and all prior versions)

The topics covered in this exam are:

1. [Language Enhancements](#language6)
1. [Concurrency](#concurrency6)
1. [Localization](#localization6)
1. [Java File I/O (NIO.2)](#nio26)
1. [Lambda](#lambda6)
1. [Java Collections](#collections6)
1. [Java Streams](#streams6)

### <a name="language6" id="language6">Section 1: Language
Enhancements</a>

**Item 1:** Develop code that uses <tt>String</tt> objects in the
<tt>switch</tt> statement, binary literals, and numeric literals, including
underscores in literals

<li>
[The switch Statement](../../java/nutsandbolts/switch.html)</li>
<li>
[Primitive Data Types](../../java/nutsandbolts/datatypes.html)</li>
<li>
[Primitive Data Types](../../java/nutsandbolts/datatypes.html)</li>

**Item 2:** Develop code that uses <tt>try</tt>-with-resources
statements, including using classes that implement the
<tt>AutoCloseable</tt> interface

<li>
[The try-with-resources Statement](../../essential/exceptions/tryResourceClose.html)</li>

**Item 3:** Develop code that handles multiple <tt>Exception</tt>
types in a single <tt>catch</tt> block

<li>
[The catch Blocks](../../essential/exceptions/catch.html)</li>

**Item 4:** Use static and default methods of an interface
including inheritance rules for a default method

<li>
[Default Methods](../../java/IandI/defaultmethods.html)</li>

### <a name="concurrency6" id="concurrency6">Section 2: Concurrency</a>

**Item 1:** Use classes from the <tt>java.util.concurrent</tt> package
including <tt>CyclicBarrier</tt> and <tt>CopyOnWriteArrayList</tt> with a focus on
the advantages over and differences from the traditional <tt>java.util</tt>
collections

**Item 2:** Use <tt>Lock</tt>, <tt>ReadWriteLock</tt>, and <tt>ReentrantLock</tt>
classes in the <tt>java.util.concurrent.locks</tt> and
<tt>java.util.concurrent.atomic</tt> packages to support lock-free
thread-safe programming on single variables

<li>
[Atomic Variables](../../essential/concurrency/atomicvars.html)</li>

**Item 3:** Use <tt>Executor</tt>, <tt>ExecutorService</tt>, <tt>Executors</tt>,
<tt>Callable</tt>, and <tt>Future</tt> to execute tasks using thread pools

<li>
[Executor Interfaces](../../essential/concurrency/exinter.html)</li>

**Item 4:** Use the parallel Fork/Join Framework

<li>
[Fork/Join](../../essential/concurrency/forkjoin.html)</li>

### <a name="localization6" id="localization6">Section 3: Localization</a>

**Item 1:** Describe the advantages of localizing an
application and developing code that defines, reads, and sets the
locale with a <tt>Locale</tt> object

<li>
[Introduction](../../i18n/intro/index.html)</li>

<li>
[Creating a Locale](../../i18n/locale/create.html)</li>

**Item 2:** Build a resource bundle for a locale and call a
resource bundle from an application

<li>
[Isolating Locale-Specific Data](../../i18n/resbundle/index.html)</li>

**Item 3:** Create and manage date- and time-based events by
using <tt>LocalDate</tt>, <tt>LocalTime</tt>, <tt>LocalDateTime</tt>, <tt>Instant</tt>, <tt>Period</tt>, and
<tt>Duration</tt>, including a combination of date and time in a single
object

<li>
[Date Classes](../../datetime/iso/date.html)</li>
<li>
[Date and Time Classes](../../datetime/iso/datetime.html)</li>
<li>
[Instant Class](../../datetime/iso/instant.html)</li>
<li>
[Period and Duration](../../datetime/iso/period.html)</li>

**Item 4:** Format dates, numbers, and currency values for
localization with the <tt>NumberFormat</tt> and <tt>DateFormat</tt> classes,
including number and date format patterns

<li>
[Numbers and Currencies](../../i18n/format/numberintro.html)</li>
<li>
[Dates and Times](../../i18n/format/dateintro.html)</li>

**Item 5:** Work with dates and times across time zones and
manage changes resulting from daylight savings

<li>
[Time Zone and Offset Classes](../../datetime/iso/timezones.html)</li>

### <a name="nio26" id="nio26">Section 4: Java File I/O (NIO.2)</a>

**Item 1:** Operate on file and directory paths by using the
<tt>java.nio.Path</tt> class

<li>
[Path Operations](../../essential/io/pathOps.html)</li>

**Item 2:** Check, delete, copy, or move a file or directory
by using the <tt>java.nio.Files</tt> class

<li>
[Checking a File or Directory](../../essential/io/check.html)</li>
<li>
[Deleting a File or Directory](../../essential/io/delete.html)</li>
<li>
[Copying a File or Directory](../../essential/io/copy.html)</li>
<li>
[Moving a File or Directory](../../essential/io/move.html)</li>

**Item 3:** Recursively access a directory tree by using the
<tt>DirectoryStream</tt> and <tt>FileVisitor</tt> interfaces

<li>
[Creating and Reading Directories](../../essential/io/dirs.html)</li>
<li>
[Walking the File Tree](../../essential/io/walk.html)</li>

**Item 4:** Find a file by using the PathMatcher interface,
and use Java SE 8 I/O improvements, including <tt>Files.find</tt>,
<tt>Files.walk</tt>, and <tt>Files.lines methods</tt>

<li>
[Finding Files](../../essential/io/find.html)</li>
<li>
[Walking the File Tree](../../essential/io/walk.html)</li>

**Item 5:** Observe the changes in a directory by using the
<tt>WatchService</tt> interface

<li>
[Watching a Directory for Changes](../../essential/io/notification.html)</li>

### <a name="lambda6" id="lambda6">Section 5: Lambda</a>

The sections  
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html) and
[Aggregate Operations](../../collections/streams/index.html) cover some of the following items:

**Item 1:** Define and write functional interfaces and
describe the interfaces of the <tt>java.util.function</tt> package

**Item 2:** Describe a lambda expression; refactor the code
that uses an anonymous inner class to use a lambda expression;
describe type inference and target typing

**Item 3:** Develop code that uses the built-in interfaces
included in the <tt>java.util.function</tt> package, such as <tt>Function</tt>,
<tt>Consumer</tt>, <tt>Supplier</tt>, <tt>UnaryOperator</tt>, <tt>Predicate</tt>, and <tt>Optional</tt> APIs,
including the primitive and binary variations of the interfaces

**Item 4:** Develop code that uses a method reference,
including refactoring a lambda expression to a method reference

### <a name="collections6" id="collections6">Section 6: Java
Collections</a>

The sections  
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html) and
[Aggregate Operations](../../collections/streams/index.html) cover some of the following items:

**Item 1:** Develop code that uses diamond with generic
declarations

<li>
[The Diamond](../../java/generics/types.html#diamond)</li>

**Item 2:** Develop code that iterates a collection, filters
a collection, and sorts a collection by using lambda
expressions

**Item 3:** Search for data by using methods, such as
<tt>findFirst</tt>, <tt>findAny</tt>, <tt>anyMatch</tt>, <tt>allMatch</tt>, and <tt>noneMatch</tt>

**Item 4:** Perform calculations on Java streams by using
<tt>count</tt>, <tt>max</tt>, <tt>min</tt>, <tt>average</tt>, and <tt>sum</tt> methods and save results to a
collection by using the <tt>collect</tt> method and <tt>Collector</tt> class,
including the <tt>averagingDouble</tt>, <tt>groupingBy</tt>, <tt>joining</tt>, and <tt>partitioningBy</tt>
methods

**Item 5:** Develop code that uses Java SE 8 collection
improvements, including the <tt>Collection.removeIf</tt>,
<tt>List.replaceAll</tt>, <tt>Map.computeIfAbsent</tt>, and
<tt>Map.computeIfPresent</tt> methods

**Item 6:** Develop code that uses the <tt>merge</tt>, <tt>flatMap</tt>,
and <tt>map</tt> methods on Java streams

### <a name="streams6" id="streams6">Section 7: Java Streams</a>

The sections  
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html) and
[Aggregate Operations](../../collections/streams/index.html) cover some of the following items:

**Item 1:** Describe the <tt>Stream</tt> interface and pipelines;
create a stream by using the <tt>Arrays.stream</tt> and <tt>IntStream.range</tt>
methods; identify the lambda operations that are lazy

**Item 2:** Develop code that uses parallel streams,
including decomposition operation and reduction operation in
streams

## Upgrade Java SE 7 to Java SE 8 OCP Programmer

The topics covered in this exam are:

1. [Lambda Expressions](#lambda7)
1. [Using Built-in Lambda Types](#builtinlambda7)
<li><a href="#collectionslambda7">Java Collections and Streams with
Lambdas</a></li>
<li><a href="#collectionoperations7">Collection Operations with
Lambda</a></li>
1. [Parallel Streams](#parallel7)
1. [Lambda Cookbook](#lambdacookbook7)
1. [Method Enhancements](#methodenhancements)
1. [Use Java SE 8 Date/Time API](#datetime7)

### <a name="lambda7" id="lambda7">Section 1: Lambda Expressions</a>

**Item 1:** Describe and develop code that uses Java inner
classes, including nested class, static class, local class, and
anonymous classes

<li>
[Nested Classes](../../java/javaOO/nested.html)</li>
<li>
[Local Classes](../../java/javaOO/localclasses.html)</li>
<li>
[Anonymous Classes](../../java/javaOO/anonymousclasses.html)</li>
<li>
[When to Use Nested Classes, Local Classes, Anonymous Classes, and Lambda Expressions ](../../java/javaOO/whentouse.html)</li>

**Item 2:** Describe and write functional interfaces

**Item 3:** Describe a lambda expression; refactor the code
that uses an anonymous inner class to use a lambda expression;
describe type inference and target typing

### <a name="builtinlambda7" id="builtinlambda7">Section 2: Using Built-in
Lambda Types</a>

The sections  
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html) and
[Aggregate Operations](../../collections/streams/index.html) cover some of the following items:

**Item 1:** Describe the interfaces of the java.util.function
package

**Item 2:** Develop code that uses the Function interface

**Item 3:** Develop code that uses the Consumer interface

**Item 4:** Develop code that uses the Supplier interface

**Item 5:** Develop code that uses the UnaryOperator
interface

**Item 6:** Develop code that uses the Predicate
interface

**Item 7:** Develop code that uses the primitive and binary
variations of the base interfaces of the java.util.function
package

**Item 8:** Develop code that uses a method reference,
including refactoring a lambda expression to a method reference

### <a name="collectionslambda7" id="collectionslambda7">Section 3: Java
Collections and Streams with Lambdas</a>

The sections  
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html) and
[Aggregate Operations](../../collections/streams/index.html) cover some of the following items:

**Item 1:** Develop code that iterates a collection by using
the forEach() method and method chaining

**Item 2:** Describe the Stream interface and pipelines

**Item 3:** Filter a collection by using lambda
expressions

**Item 4:** Identify the operations, on stream, that are
lazy

### <a name="collectionoperations7" id="collectionoperations7">Section 4: Collection Operations with
Lambda</a>

The sections  
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html) and
[Aggregate Operations](../../collections/streams/index.html) cover some of the following items:

**Item 1:** Develop code to extract data from an object by
using the map() method

**Item 2:** Search for data by using methods such as
findFirst(), findAny(), anyMatch(), allMatch(), and noneMatch()

**Item 3:** Describe the unique characteristics of the
Optional class

**Item 4:** Perform calculations by using Java Stream
methods, such as count(), max(), min(), average(), and sum()

**Item 5:** Sort a collection by using lambda expressions

**Item 6:** Develop code that uses the Stream.collect()
method and Collectors class methods, such as averagingDouble(),
groupingBy(), joining(), and partitioningBy()

### <a name="parallel7" id="parallel7">Section 5: Parallel Streams</a>

**Item 1:** Develop code that uses parallel streams

**Item 2:** Implement decomposition and reduction in
streams

### <a name="lambdacookbook7" id="lambdacookbook7">Section 6: Lambda
Cookbook</a>

**Item 1:** Develop code that uses Java SE 8 collection
improvements, including Collection.removeIf, List.replaceAll,
Map.computeIfAbsent, and Map.computeIfPresent methods

**Item 2:** Develop code that uses Java SE 8 I/O
improvements, including Files.find, Files.walk, and Files.lines methods

**Item 3:** Use flatMap() methods in the Stream API

**Item 4:** Develop code that creates a stream by using the
Arrays.stream() and IntStream.range() methods

### <a name="methodenhancements" id="methodenhancements">Section 7: Method
Enhancements</a>

**Item 1:** Add static methods to interfaces

**Item 2:** Define and use a default method of an interface
and describe the inheritance rules for the default method

### <a name="datetime7" id="datetime7">Section 8: Use Java SE 8 Date/Time
API</a>

**Item 1:** Create and manage date- and time-based events,
including a combination of date and time in a single object, by
using LocalDate, LocalTime, LocalDateTime, Instant, Period, and
Duration

<li>
[Date Classes](../../datetime/iso/date.html)</li>
<li>
[Date and Time Classes](../../datetime/iso/datetime.html)</li>

**Item 2:** Work with dates and times across time zones and
manage changes resulting from daylight savings, including Format
date and times values

<li>
[Time Zone and Offset Classes](../../datetime/iso/timezones.html)</li>

**Item 3:** Define, create, and manage date- and time-based
events using Instant, Period, Duration, and TemporalUnit

<li>
[Instant Class](../../datetime/iso/instant.html)</li>
<li>
[The Temporal Package](../../datetime/iso/temporal.html)</li><li>
[Period and Duration](../../datetime/iso/period.html)</li>
