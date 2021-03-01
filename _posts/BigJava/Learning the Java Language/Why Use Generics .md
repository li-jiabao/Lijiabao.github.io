
# Why Use Generics?


In a nutshell, generics enable **types** (classes and interfaces) to be parameters when defining classes, interfaces and methods. Much like the more familiar **formal parameters** used in method declarations, type parameters provide a way for you to re-use the same code with different inputs. The difference is that the inputs to formal parameters are values, while the inputs to type parameters are types.


Code that uses generics has many benefits over non-generic code:

<li>Stronger type checks at compile time.<br />
A Java compiler applies strong type checking to generic code and issues errors if the code violates type safety. Fixing compile-time errors is easier than fixing runtime errors, which can be difficult to find.<br /><br /></li>
<li>Elimination of casts.<br />
The following code snippet without generics requires casting:
<pre><code>
List list = new ArrayList();
list.add("hello");
String s = **(String)** list.get(0);
</code></pre>
When re-written to use generics, the code does not require casting:
<pre><code>
List&lt;String&gt; list = new ArrayList&lt;String&gt;();
list.add("hello");
String s = list.get(0);   // no cast
</code></pre>
</li>
<li>Enabling programmers to implement generic algorithms.<br />
By using generics, programmers can implement generic algorithms that work on collections of different types, can be customized, and are type safe and easier to read.</li>
