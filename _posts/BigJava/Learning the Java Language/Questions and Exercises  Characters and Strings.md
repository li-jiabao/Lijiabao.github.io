
# Questions and Exercises: Characters and Strings

## Questions

<li>
What is the initial capacity of the following string builder?
<pre><code>
StringBuilder sb = new StringBuilder("Able was I ere I saw Elba.");
</code></pre>
</li>
<li>
Consider the following string:
<pre><code>
String hannah = "Did Hannah see bees? Hannah did.";
</code></pre>
<ol type="a">
<li>
What is the value displayed by the expression `hannah.length()`?
</li>
<li>
What is the value returned by the method call `hannah.charAt(12)`?
</li>
<li>
Write an expression that refers to the letter `b` in the string referred to by `hannah`.
</li>

How long is the string returned by the following expression? What is the string?

```

"Was it a car or a cat I saw?".substring(9, 12)

```

In the following program, called 
[`ComputeResult`](ComputeResult.java), what is the value of `result` after each numbered line executes?

```

public class ComputeResult {
    public static void main(String[] args) {
        String original = "software";
        StringBuilder result = new StringBuilder("hi");
        int index = original.indexOf('a');

/*1*/   result.setCharAt(0, original.charAt(0));
/*2*/   result.setCharAt(1, original.charAt(original.length()-1));
/*3*/   result.insert(1, original.charAt(4));
/*4*/   result.append(original.substring(1,4));
/*5*/   result.insert(3, (original.substring(index, index+2) + " ")); 

        System.out.println(result);
    }
}

```

## Exercises

<li>
Show two ways to concatenate the following two strings together to get the string `"Hi, mom."`:
<pre><code>
String hi = "Hi, ";
String mom = "mom.";
</code></pre>
</li>
<li>
Write a program that computes your initials from your full name and displays them.
</li>
<li>
An anagram is a word or a phrase made by transposing the letters of another word or phrase; for example, "parliament" is an anagram of "partial men," and "software" is an anagram of "swear oft." Write a program that figures out whether one string is an anagram of another string. The program should ignore white space and punctuation.
</li>


[Check your answers.](characters-answers.html)
