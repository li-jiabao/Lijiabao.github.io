
# Questions and Exercises: Nested Classes

## Questions

<li>
<p>The program 
[`Problem.java`](Problem.java) doesn't compile. What do you need to do to make it compile? Why?</p>
</li>
<li>
<p>Use the Java API documentation for the 
[`Box`](https://docs.oracle.com/javase/8/docs/api/javax/swing/Box.html) class (in the `javax.swing` package) to help you answer the following questions.</p>
<ol type="a">
<li>
What static nested class does `Box` define?
</li>
<li>
What inner class does `Box` define?
</li>
<li>
What is the superclass of `Box`'s inner class?
</li>
<li>
Which of `Box`'s nested classes can you use from any class?
</li>
<li>
How do you create an instance of `Box`'s `Filler` class?
</li>

## Exercises

<li>
<p>Get the file 
[`Class1.java`](Class1.java). Compile and run `Class1`. What is the output?</p>
</li>
<li><p>The following exercises involve modifying the class
[`DataStructure.java`](../examples/DataStructure.java), which the section
[Inner Class Example](../../../java/javaOO/innerclasses.html) discusses.</p>
    <ol type="a">
    
      1. Define a method named `print(DataStructureIterator iterator)`. Invoke this method with an instance of the class `EvenIterator` so that it performs the same function as the method `printEven`.
      
      1. Invoke the method `print(DataStructureIterator iterator)` so that it prints elements that have an odd index value. Use an anonymous class as the method's argument instead of an instance of the interface `DataStructureIterator`.
      
      1. Define a method named `print(java.util.Function&lt;Integer, Boolean&gt; iterator)` that performs the same function as `print(DataStructureIterator iterator)`. Invoke this method with a lambda expression to print elements that have an even index value. Invoke this method again with a lambda expression to print elements that have an odd index value.
      
      <li>Define two methods so that the following two statements print elements that have an even index value and elements that have an odd index value:
<pre class="codeblock">DataStructure ds = new DataStructure()
// ...
ds.print(DataStructure::isEvenIndex);
ds.print(DataStructure::isOddIndex);</code></pre></li>

    

[Check your answers.](nested-answers.html)
