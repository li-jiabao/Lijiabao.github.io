
# Questions and Exercises: Classes

## Questions

<li>
Consider the following class:
<pre><code>
public class IdentifyMyParts {
    public static int x = 7; 
    public int y = 3; 
}
</code></pre>
<ol type="a">
<li>
What are the class variables?
</li>
<li>
What are the instance variables?
</li>
<li>
What is the output from the following code:
<pre><code>
IdentifyMyParts a = new IdentifyMyParts();
IdentifyMyParts b = new IdentifyMyParts();
a.y = 5;
b.y = 6;
a.x = 1;
b.x = 2;
System.out.println("a.y = " + a.y);
System.out.println("b.y = " + b.y);
System.out.println("a.x = " + a.x);
System.out.println("b.x = " + b.x);
System.out.println("IdentifyMyParts.x = " + IdentifyMyParts.x);
</code></pre>
</li>

## Exercises

<li>
Write a class whose instances represent a single playing card from a deck of cards. Playing cards have two distinguishing properties: rank and suit. Be sure to keep your solution as you will be asked to rewrite it in [Enum Types](enum-questions.html).
<hr />**Hint:**&#160;You can use the `assert` statement to check your assignments. You write:
<pre><code>
assert (boolean expression to test); 
</code></pre>
If the boolean expression is false, you will get an error message. For example,
<pre><code>
assert toString(ACE) == "Ace";
</code></pre>
should return `true`, so there will be no error message.
If you use the `assert` statement, you must run your program with the `ea` flag:
<pre><code>
java -ea YourProgram.class
</code></pre>
<hr />
</li>
<li>
Write a class whose instances represent a **full** deck of cards. You should also keep this solution.
</li>
<li>
3. Write a small program to test your deck and card classes. The program can be as simple as creating a deck of cards and displaying its cards.
</li>


[Check your answers.](creating-answers.html)
