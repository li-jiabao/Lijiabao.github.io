
# Questions and Exercises: Operators

## Questions

<li>Consider the following code snippet.
<pre><code>
arrayOfInts[j] &gt; arrayOfInts[j+1]
</code></pre>
Which operators does the code contain?</li>
<li>Consider the following code snippet.
<pre><code>
int i = 10;
int n = i++%5;
</code></pre>
<ol type="a">
1. What are the values of `i` and `n` after the code is executed?
1. What are the final values of `i` and `n` if instead of using the postfix increment operator (`i++`), you use the prefix version (`++i)`)?

## Exercises

<li>Change the following program to use compound assignments:
<pre><code>
class ArithmeticDemo {

     public static void main (String[] args){
          
          int result = 1 + 2; // result is now 3
          System.out.println(result);

          result = result - 1; // result is now 2
          System.out.println(result);

          result = result * 2; // result is now 4
          System.out.println(result);

          result = result / 2; // result is now 2
          System.out.println(result);

          result = result + 8; // result is now 10
          result = result % 7; // result is now 3
          System.out.println(result);
     }
}

</code></pre>
</li>
<li>In the following program, explain why the value "6" is printed twice in a row:
<pre><code>
class PrePostDemo {
    public static void main(String[] args){
        int i = 3;
        i++;
        System.out.println(i);    // "4"
        ++i;                     
        System.out.println(i);    // "5"
        System.out.println(++i);  // "6"
        System.out.println(i++);  // "6"
        System.out.println(i);    // "7"
    }
}
</code></pre>
</li>


[Check your answers](answers_operators.html)
