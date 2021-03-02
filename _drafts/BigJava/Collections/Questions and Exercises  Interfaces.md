
# Questions and Exercises: Interfaces

## Questions

<li>At the beginning of this lesson, you learned that the core collection interfaces
 are organized into two distinct
inheritance trees. One interface in particular is not considered to be 
a true `Collection`, and therefore sits at the top of its own tree. What is the name 
of this interface?
</li>

<li>
Each interface in the collections framework is declared
with the `&lt;E&gt;` syntax, which tells you that it is 
generic. When you declare a `Collection` instance, what is
the advantage of specifying the type of objects that it will contain?
</li>

<li>
What interface represents a collection that does not allow duplicate elements?
</li>

<li>
What interface forms the root of the collections hierarchy?
</li>

<li>
What interface represents an ordered collection that may contain duplicate elements?
</li>

<li>
What interface represents a collection that holds elements prior to processing?
</li>

<li>
What interface repesents a type that maps keys to values?
</li>

<li>
What interface represents a double-ended queue?
</li>

1. Name three different ways to iterate over the elements of a `List`.

1. True or False: Aggregate operations are mutative operations that modify the underlying collection.



## Exercises

<li>Write a program that prints its arguments in random order. Do not make a copy of the argument array.
Demonstrate how to print out the elements using both streams and the traditional enhanced for statement.

</li>
<li>Take the 
[`FindDups`](../examples/FindDups.java)example 
 and modify it to use a `SortedSet` instead of a `Set`. Specify a `Comparator` so that case is ignored when sorting and identifying set elements.</li>
<li>Write a method that takes a `List&lt;String&gt;` and applies 
[`String.trim`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#trim--) to each element. 
</li>


<li>Consider the four core interfaces, `Set`, `List`, `Queue`, and `Map`.
For each of the following four assignments, specify which of the four core
interfaces is best-suited, and explain how to use it to implement the assignment.
  <ol>
    <li>Whimsical Toys Inc (WTI) needs to record the names of all its employees. Every month, an employee will be chosen at random
 from these records to receive a free toy.</li>
   <li>WTI has decided that each new product will be named after an employee but only first names will be used, and each name
 will be used only once. Prepare a list of unique first names.</li>
   <li>WTI decides that it only wants to use the most popular names for its toys. Count up the number of employees who have each first
 name.</li>
   <li>WTI acquires season tickets for the local lacrosse team, to be shared by employees. Create a waiting list for this popular
sport.</li>
  

[Check your answers.](answers.html)
