
# Questions and Exercises

## Questions

<li>Is the following code legal?
<pre><code>
try {
    
} finally {
    
}
</code></pre>
</li>
<li>What exception types can be caught by the following handler?
<pre><code>
catch (Exception e) {
     
}
</code></pre>
What is wrong with using this type of exception handler?</li>
<li>Is there anything wrong with the following exception handler as written? Will this code compile?
<pre><code>
try {

} catch (Exception e) {
    
} catch (ArithmeticException a) {
    
}
</code></pre>
</li>
<li>Match each situation in the first list with an item in the second list.
<ol type="a">
<li><code>int[] A;<br />
A[0] = 0;</code></li>
1. The JVM starts running your program, but the JVM can't find the Java platform classes. (The Java platform classes reside in `classes.zip` or `rt.jar`.)
1. A program is reading a stream and reaches the `end of stream` marker.
1. Before closing the stream and after reaching the `end of stream` marker, a program tries to read the stream again.

1. __error
1. __checked exception
1. __compile error
1. __no exception

## Exercises

<li>Add a `readList` method to 
[`<code>ListOfNumbers.java`</code>](../examples/ListOfNumbers.java). This method should read in `int` values from a file, print each value, and append them to the end of the vector. You should catch all appropriate errors. You will also need a text file containing numbers to read in.</li>
<li>Modify the following `cat` method so that it will compile.
<pre><code>
public static void cat(File file) {
    RandomAccessFile input = null;
    String line = null;

    try {
        input = new RandomAccessFile(file, "r");
        while ((line = input.readLine()) != null) {
            System.out.println(line);
        }
        return;
    } finally {
        if (input != null) {
            input.close();
        }
    }
}
</code></pre>
</li>
