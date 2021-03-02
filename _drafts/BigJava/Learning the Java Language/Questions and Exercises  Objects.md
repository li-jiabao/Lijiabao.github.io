
# Questions and Exercises: Objects

## Questions

<li>
What's wrong with the following program?
<pre><code>
public class SomethingIsWrong {
    public static void main(String[] args) {
        Rectangle myRect;
        myRect.width = 40;
        myRect.height = 50;
        System.out.println("myRect's area is " + myRect.area());
    }
}
</code></pre>
</li>
<li>
The following code creates one array and one string object. How many references to those objects exist after the code executes? Is either object eligible for garbage collection?
<pre><code>
...
String[] students = new String[10];
String studentName = "Peter Parker";
students[0] = studentName;
studentName = null;
...
</code></pre>
</li>
<li>
How does a program destroy an object that it creates?
</li>

## Exercises

<li>
Fix the program called `SomethingIsWrong` shown in Question 1.
</li>
<li>
<p>Given the following class, called 
[`NumberHolder`](NumberHolder.java), write some code that creates an instance of the class, initializes its two member variables, and then displays the value of each member variable.</p>
<pre><code>
public class NumberHolder {
    public int anInt;
    public float aFloat;
}
</code></pre>
</li>


[Check your answers.](objects-answers.html)
