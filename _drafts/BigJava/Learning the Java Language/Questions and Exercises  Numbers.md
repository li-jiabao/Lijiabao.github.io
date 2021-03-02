
# Questions and Exercises: Numbers

## Questions

<li>
Use the API documentation to find the answers to the following questions:
<ol type="a">
<li>
What `Integer` method can you use to convert an `int` into a string that expresses the number in hexadecimal? For example, what method converts the integer 65 into the string "41"?
</li>
<li>
What `Integer` method would you use to convert a string expressed in base 5 into the equivalent `int`? For example, how would you convert the string "230" into the integer value 65? Show the code you would use to accomplish this task.
</li>
<li>
What Double method can you use to detect whether a floating-point number has the special value Not a Number (`NaN`)?
</li>

What is the value of the following expression, and why?

```

Integer.valueOf(1).equals(Long.valueOf(1))

```

## Exercises

<li>
<p>Change 
[`MaxVariablesDemo`](MaxVariablesDemo.java) to show minimum values instead of maximum values. You can delete all code related to the variables `aChar` and `aBoolean`. What is the output?</p>
</li>
<li>
Create a program that reads an unspecified number of integer arguments from the command line and adds them together. For example, suppose that you enter the following:
<pre><code>
java Adder 1 3 2 10
</code></pre>
<p>The program should display `16` and then exit. The program should display an error message if the user enters only one argument. You can base your program on 
[`ValueOfDemo`](../../data/examples/ValueOfDemo.java).</p>
</li>
<li>
Create a program that is similar to the previous one but has the following differences:
<ul>
1. Instead of reading integer arguments, it reads floating-point arguments.
1. It displays the sum of the arguments, using exactly two digits to the right of the decimal point.
</ul>
For example, suppose that you enter the following:
<pre><code>
java FPAdder 1 1e2 3.0 4.754
</code></pre>
The program would display `108.75`. Depending on your locale, the decimal point might be a comma (`,`) instead of a period (`.`).
</li>


[Check your answers.](numbers-answers.html)
