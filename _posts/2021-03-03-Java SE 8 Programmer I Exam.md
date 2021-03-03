---
title: Java SE 8 Programmer I Exam
author: lijiabao
date: 2020-12-06 01:38:37.819221700 +0800
category: Bonus
categories: Bonus
tags: Bonus
---

# Java SE 8 Programmer II Exam

This page maps sections in the Java Tutorials to topics covered in the Java SE 8 Programmer II exam.  This exam is associated with the Oracle Certified Professional, Java SE 8 Programmer certificate. The topics covered in this exam are:

1. [Java Class Design](#classDesign)
1. [Advanced Class Design](#advancedClassDesign)
1. [Generics and Collections](#generics)
1. [Lambda Built-In Functional Interfaces](#lambda)
1. [Java Stream API](#stream)
1. [Exceptions and Assertions](#exceptions)
1. [Use Java SE 8 Date/Time API](#datetime)
1. [Java I/O Fundamentals](#io)
1. [Java File I/O (NIO.2)](#nio2)
1. [Concurrency](#concurrency)
1. [Building Database Applications with JDBC](#jdbc)
1. [Localization](#localization)

## <a name="classDesign" id="classDesign">Section 1: Java Class Design</a>

**Item 1:** Implement encapsulation.

<li>
[What Is an Object?](../../java/concepts/object.html)</li>

**Item 2:** Implement inheritance including visibility modifiers and composition.

<li>
[Inheritance](../../java/IandI/subclasses.html)</li>
<li>
[Overriding and Hiding Methods](../../java/IandI/override.html)</li>

**Item 3:** Implement polymorphism.

<li>
[Polymorphism](../../java/IandI/polymorphism.html)</li>

**Item 4:** Override <tt>hasCode</tt>, <tt>equals</tt>, and <tt>toString</tt> methods from <tt>Object</tt> class.

<li>                              
[Object as a Superclass](../../java/IandI/objectclass.html)</li>

**Item 5:** Create and use singleton classes and immutable classes.

<li>                              
[The Singleton Design Pattern](../../ext/basics/spi.html#singleton)</li>
<li>                              
[A Strategy for Defining Immutable Objects](../../essential/concurrency/imstrat.html)</li>

**Item 6:** Develop code that uses the <tt>static</tt> keyword on initialize blocks, variables, methods, and classes.

<li>                              
[Understanding Class Members](../../java/javaOO/classvars.html)</li>
<li>                              
[Initializing Fields](../../java/javaOO/initial.html)</li>
<li>                              
[Overriding and Hiding Methods](../../java/IandI/override.html)</li>
<li>                              
[Default Methods](../../java/IandI/defaultmethods.html)</li>

## <a name="advancedClassDesign" id="advancedClassDesign">Section 2: Advanced Class Design</a>

**Item 1:** Develop code that uses abstract classes and methods.

<li>
[Abstract Methods and Classes](../../java/IandI/abstract.html)</li>

**Item 2:** Develop code that uses the <tt>final</tt>.

<li>
[Variables](../../java/nutsandbolts/variables.html)</li>
<li>
[Understanding Class Members](../../java/javaOO/classvars.html)</li>

**Item 3:** Create inner classes including static inner classes, local classes, nested classes, and anonymous innter classes.

<li>
[Nested Classes](../../java/javaOO/nested.html)</li>
<li>
[Inner Class Example](../../java/javaOO/innerclasses.html)</li>
<li>
[Local Classes](../../java/javaOO/localclasses.html)</li>
<li>
[Anonymous Classes](../../java/javaOO/anonymousclasses.html)</li>
<li>
[When to Use Nested Classes, Local Classes, Anonymous Classes, and Lambda Expressions ](../../java/javaOO/whentouse.html)</li>

**Item 4:** Use enumerated types including methods, and constructors in an <tt>enum</tt> type

<li>
[Enum Types](../../java/javaOO/enum.html)</li>
<li>
[Default Methods](../../java/IandI/defaultmethods.html)</li>
<li>
[Enumerated Types](../../reflect/special/enum.html)</li>

**Item 5:** Develop code that declares, implements and/or extends interfaces and use the <tt>@Override</tt> annotation.

<li>
[Predefined Annotation Types](../../java/annotations/predefined.html)</li>

**Item 6:** Create and use lambda expressions.

<li>
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html)</li>

## <a name="generics" id="generics">Section 3: Generics and Collections</a>

The
[Generics (Updated)](../../java/generics/index.html) lesson, the
[Collections](../../collections/index.html) trail and, in particular, the specified pages.

**Item 1**: Create and use a generic class.

<li>
[Generic Types](../../java/generics/types.html)</li>

**Item 2**: Create and use <tt>ArrayList</tt>, <tt>TreeSet</tt>, <tt>TreeMap</tt>, and <tt>ArrayDeque</tt> objects.

<li>
[The List Interface](../../collections/interfaces/list.html)</li>
<li>
[The Set Interface](../../collections/interfaces/set.html)</li>
<li>
[The Map Interface](../../collections/interfaces/map.html)</li>
<li>
[The Deque Interface](../../collections/interfaces/deque.html)</li>

**Item 3**: Use <tt>java.util.Comparator</tt> and <tt>java.lang.Comparable</tt> interfaces.

<li>
[Object Ordering](../../collections/interfaces/order.html)</li>

**Item 4**: Collections, streams, and filters.

<li>
[Aggregate Operations](../../collections/streams/index.html)</li>

**Item 5**: Iterate using <tt>forEach</tt> methods of <tt>Streams</tt> and <tt>List</tt>.

<li>
[The Collection Interface](../../collections/interfaces/collection.html)</li>
<li>
[Aggregate Operations](../../collections/streams/index.html)</li>

**Item 6**: Describe the <tt>Stream</tt> interface and the <tt>Stream</tt> pipeline.

<li>
[Aggregate Operations](../../collections/streams/index.html)</li>

**Item 7**: Filter a collection by using lambda expressions.

<li>
[The Collection Interface](../../collections/interfaces/collection.html)</li>
<li>
[Aggregate Operations](../../collections/streams/index.html)</li>

**Item 8**: Use method references with streams.

<li>
[Method References](../../java/javaOO/methodreferences.html)</li>

## <a name="lambda" id="lambda">Section 4: Lambda Built-In Functional Interfaces</a>

The sections  
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html) and
[Aggregate Operations](../../collections/streams/index.html) cover some of the following items:

**Item 1**: Use the built-in interfaces included in the <tt>java.util.function</tt> package such as <tt>Predicate</tt>, <tt>Consumer</tt>, <tt>Function</tt>, and <tt>Supplier</tt>.

**Item 2**: Develop code that uses primitive versions of functional interfaces.

**Item 3**: Develop code that uses binary versions of functional interfaces.

**Item 4**: Develop code that uses the <tt>UnaryOperator</tt> interface.

## <a name="stream" id="stream">Section 5: Java Stream API</a>

The sections  
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html) and
[Aggregate Operations](../../collections/streams/index.html) cover some of the following items:

**Item 1**: Develop code to extract data from an object using peek() and map() methods including primitive versions of the map() method.

**Item 2**: Search for data by using search methods of the Stream classes including findFirst, findAny, anyMatch, allMatch, noneMatch.

**Item 3**: Develop code that uses the Optional class.

**Item 4**: Develop code that uses Stream data methods and calculation methods.

**Item 5**: Sort a collection using Stream API.

**Item 6**: Save results to a collection using the collect method and group/partition data using the Collectors class.

**Item 7**: Use flatMap() methods in the Stream API.

## <a name="exceptions" id="exceptions">Section 6: Exceptions and Assertions</a>

**Item 1:** Use <tt>try</tt>-<tt>catch</tt> and <tt>throws</tt> statements.

<li>
[Specifying the Exceptions Thrown by a Method](../../essential/exceptions/declaring.html)</li>
<li>
[How to Throw Exceptions](../../essential/exceptions/throwing.html)</li>

**Item 2:** Use <tt>catch</tt>, <tt>multi-catch</tt>, and <tt>finally</tt> clauses.

<li>
[Catching and Handling Exceptions](../../essential/exceptions/handling.html)</li>
<li>
[The try Block](../../essential/exceptions/try.html)</li>
<li>
[The catch Blocks](../../essential/exceptions/catch.html)</li>
<li>
[The finally Block](../../essential/exceptions/finally.html)</li>
<li>
[Putting It All Together](../../essential/exceptions/putItTogether.html)</li>

**Item 3:** Use autoclose resources with a <tt>try</tt>-with-resources statement.

<li>
[The try-with-resources Statement](../../essential/exceptions/tryResourceClose.html)</li>

**Item 4:** Create custom exceptions and autocloseable resources.

<li>
[Creating Exception Classes](../../essential/exceptions/creating.html)</li>

**Item 5:** Test invariants by using assertions.

<li>
[Questions and Exercises: Classes (assertion example)](../../java/javaOO/QandE/creating-questions.html)</li>

## <a name="datetime" id="datetime">Section 7: Use Java SE 8 Date/Time API </a>

**Item 1:** Create and manage date-based and time-based events including a combination of date and time into a single object using <tt>LocalDate</tt>, <tt>LocalTime</tt>, <tt>LocalDateTime</tt>, <tt>Instant</tt>, <tt>Period</tt>, and <tt>Duration</tt>.

<li>
[Date Classes](../../datetime/iso/date.html)</li>
<li>
[Date and Time Classes](../../datetime/iso/datetime.html)</li>
<li>
[Instant Class](../../datetime/iso/instant.html)</li>
<li>
[Period and Duration](../../datetime/iso/period.html)</li>

**Item 2:** Work with dates and times across timezones and manage changes resulting from daylight savings including Format date and times values.

<li>
[Time Zone and Offset Classes](../../datetime/iso/timezones.html)</li>

**Item 3:** Define and create and manage date-based and time-based events using <tt>Instant</tt>, <tt>Period</tt>, <tt>Duration</tt>, and <tt>TemporalUnit</tt>.

<li>
[Instant Class](../../datetime/iso/instant.html)</li>
<li>
[Period and Duration](../../datetime/iso/period.html)</li>

<li>
[The Temporal Package](../../datetime/iso/temporal.html)</li>

## <a name="io" id="io">Section 8: Java I/O Fundamentals</a>

**Item 1**: Read and write data from the console.

The 
[I/O Streams](../../essential/io/streams.html) lesson and, in particular, the following pages:

<li>
[Byte Streams](../../essential/io/bytestreams.html)</li>
<li>
[I/O from the Command Line](../../essential/io/cl.html)</li>

**Item 2**: Use <tt>BufferedReader</tt>, <tt>BufferedWriter</tt>, <tt>File</tt>, <tt>FileReader</tt>, <tt>FileWriter</tt>, <tt>FileInputStream</tt>, <tt>FileOutputStream</tt>, <tt>ObjectOutputStream</tt>, <tt>ObjectInputStream</tt>, and <tt>PrintWriter</tt> in the <tt>java.io package</tt>.

The 
[File I/O (Featuring NIO.2)](../../essential/io/fileio.html) lesson, and in particular, the following pages:

<li>
[Reading, Writing, and Creating Files](../../essential/io/file.html)</li>
<li>
[Creating and Reading Directories](../../essential/io/dirs.html)</li>
<li>
[Random Access Files](../../essential/io/rafs.html)</li>

## <a name="nio2" id="nio2">Section 9: Java File I/O (NIO.2)</a>

**Item 1**: Use the <tt>Path</tt> interface to operate on file and directory paths.

<li>
[What Is a Path? (And Other File System Facts)](../../essential/io/path.html)</li>
<li>
[Path Operations](../../essential/io/pathOps.html)</li>

**Item 2**: Use the <tt>Files</tt> class to check, read, delete, copy, move, and manage metadata a file or directory.

<li>
[File Operations](../../essential/io/fileOps.html)</li>
<li>
[Checking a File or Directory](../../essential/io/check.html)</li>
<li>
[Deleting a File or Directory](../../essential/io/delete.html)</li>
<li>
[Copying a File or Directory](../../essential/io/copy.html)</li>
<li>
[Moving a File or Directory](../../essential/io/move.html)</li>
<li>
[Managing Metadata (File and File Store Attributes)](../../essential/io/fileAttr.html)</li>
<li>
[Walking the File Tree](../../essential/io/walk.html)</li>
<li>
[Finding Files](../../essential/io/find.html)</li>
<li>
[What is a Glob?](../../essential/io/fileOps.html#glob)</li>
<li>
[Watching a Directory for Changes](../../essential/io/notification.html)</li>

**Item 3**: Use Stream API with NIO.2.

## <a name="ooDesign" id="ooDesign">Section 3: Object-Oriented Design Principles</a>

The Java Tutorials do not cover Design Patterns topics. The following references cover design patterns using the Java programming language:

- **Head First Design Patterns** by Elizabeth Freeman, et al.
- **Java Design Pattern Essentials** by Tony Bevis

**Item 1:** Write code that declares, implements and/or extends interfaces.

<li>
[Defining an Interface](../../java/IandI/interfaceDef.html)</li>
<li>
[Interfaces](../../java/IandI/createinterface.html)</li>
<li>
[Implementing an Interface](../../java/IandI/usinginterface.html)</li>

**Item 2:** Choose between interface inheritance and class inheritance.

**Item 3:** Develop code that implements "is-a" and/or "has-a" relationships.

**Item 4:** Apply object composition principles.

**Item 5:** Design a class using the Singleton design pattern.

**Item 6:** Write code to implement the DAO pattern.

**Item 7:** Design and create objects using a factory, and use factories from the API.

## <a name="strings" id="strings">Section 5: String Processing</a>

**Item 1:** Search, parse and build strings.

<li>
[Strings](../../java/data/strings.html)</li>
<li>
[Converting Between Numbers and Strings](../../java/data/converting.html)</li>
<li>
[Comparing Strings and Portions of Strings](../../java/data/comparestrings.html)</li>
<li>
[Manipulating Characters in a String](../../java/data/manipstrings.html)</li>

**Item 2:** Search, parse, and replace strings by using regular expressions.

<li>
[Methods of the Pattern Class](../../essential/regex/pattern.html)</li>
<li>
[Methods of the Matcher Class](../../essential/regex/matcher.html)</li>

**Item 3:** Use string formatting.

<li>
[Strings](../../java/data/strings.html)</li>
<li>
[Formatting Numeric Print Output](../../java/data/numberformat.html)</li>

## <a name="concurrency" id="concurrency">Section 10: Concurrency</a>

**Item 1:** Create worker threads using <tt>Runnable</tt>, <tt>Callable</tt> and use an <tt>ExecutorService</tt> to concurrently execute tasks.

<li>
[Executors](../../essential/concurrency/executors.html)</li>
<li>
[Executor Interfaces](../../essential/concurrency/exinter.html)</li>
<li>
[Thread Pools](../../essential/concurrency/pools.html)</li>

**Item 2:** Identify potential threading problems among deadlock, starvation, livelock, and race conditions.

<li>
[Memory Consistency Errors](../../essential/concurrency/memconsist.html)</li>
<li>
[Deadlock](../../essential/concurrency/deadlock.html)</li>

**Item 3:** Use synchronized keyword and java.util.concurrent.atomic package to control the order of thread execution.

<li>
[Atomic Variables](../../essential/concurrency/atomicvars.html)</li>

**Item 4:** Use java.util.concurrent collections and classes including CyclicBarrier and CopyOnWriteArrayList.

<li>
[Concurrent Collections](../../essential/concurrency/collections.html)</li>

**Item 5:** Use parallel Fork/Join Framework.

<li>
[Fork/Join](../../essential/concurrency/forkjoin.html)</li>

**Item 6:** Use parallel Streams including reduction, decomposition, merging processes, pipelines and performance.

## <a name="jdbc" id="jdbc">Section 11: Building Database Applicatons with JDBC</a>

**Item 1:**  Describe the interfaces that make up the core of the JDBC API including the <tt>Driver</tt>, <tt>Connection</tt>, <tt>Statement</tt>, and <tt>ResultSet</tt> interfaces and their relationship to provider implementations.

<li>
[JDBC Basics: Getting Started](../../jdbc/basics/gettingstarted.html)</li>

**Item 2:** Identify the components required to connect to a database using the DriverManager class including the JDBC URL.

<li>
[Establishing a Connection](../../jdbc/basics/connecting.html)</li>
<li>
[Connecting with DataSource Objects](../../jdbc/basics/sqldatasources.html)</li>

**Item 3:**  Submit queries and read results from the database including creating statements, returning result sets, iterating through the results, and properly closing result sets, statements, and connections.

<li>
[Processing SQL Statements with JDBC](../../jdbc/basics/processingsqlstatements.html)</li>
<li>
[Using Transactions](../../jdbc/basics/transactions.html)</li>
<li>
[Using RowSet Objects](../../jdbc/basics/rowset.html)</li>
<li>
[Using JdbcRowSet Objects](../../jdbc/basics/jdbcrowset.html)</li>

## <a name="localization" id="localization">Section 12: Localization</a>

**Item 1:** Read and set the locale by using the <tt>Locale</tt> object..

<li>
[Setting the Locale](../../i18n/locale/index.html)</li>
<li>
[Creating a Locale](../../i18n/locale/create.html)</li>

**Item 2:** Create and read a <tt>Properties</tt> file.

<li>
[About the ResourceBundle Class](../../i18n/resbundle/concept.html)</li>
<li>
[Backing a ResourceBundle with Properties Files](../../i18n/resbundle/propfile.html)</li>

**Item 3:** Build a resource bundle for each locale and load a resource bundle in an application.

<li>
[Customizing Resource Bundle Loading](../../i18n/resbundle/control.html)</li>
