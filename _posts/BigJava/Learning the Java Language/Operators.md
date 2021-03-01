
# Operators

Now that you've learned how to declare and initialize variables, you probably want to know how to *do something* with them. Learning the operators of the Java programming language is a good place to start. Operators are special symbols that perform specific operations on one, two, or three *operands*, and then return a result.

As we explore the operators of the Java programming language, it may be helpful for you to know ahead of time which operators have the highest precedence. The operators in the following table are listed according to precedence order. The closer to the top of the table an operator appears, the higher its precedence. Operators with higher precedence are evaluated before operators with relatively lower precedence. Operators on the same line have equal precedence. When operators of equal precedence appear in the same expression, a rule must govern which is evaluated first. All binary operators except for the assignment operators are evaluated from left to right; assignment operators are evaluated right to left.
<th id="h1">Operators</th><th id="h2">Precedence</th>
<td headers="h1">postfix</td><td headers="h2">`**expr**++ **expr**--`</td>
<td headers="h1">unary</td><td headers="h2">`++**expr** --**expr** +**expr** -**expr** ~ !`</td>
<td headers="h1">multiplicative</td><td headers="h2">`* / %`</td>
<td headers="h1">additive</td><td headers="h2">`+ -`</td>
<td headers="h1">shift</td><td headers="h2">`&lt;&lt; &gt;&gt; &gt;&gt;&gt;`</td>
<td headers="h1">relational</td><td headers="h2">`&lt; &gt; &lt;= &gt;= instanceof`</td>
<td headers="h1">equality</td><td headers="h2">`== !=`</td>
<td headers="h1">bitwise AND</td><td headers="h2">`&amp;`</td>
<td headers="h1">bitwise exclusive OR</td><td headers="h2">`^`</td>
<td headers="h1">bitwise inclusive OR</td><td headers="h2">`|`</td>
<td headers="h1">logical AND</td><td headers="h2">`&amp;&amp;`</td>
<td headers="h1">logical OR</td><td headers="h2">`||`</td>
<td headers="h1">ternary</td><td headers="h2">`? :`</td>
<td headers="h1">assignment</td><td headers="h2">`= += -= *= /= %= &amp;= ^= |= &lt;&lt;= &gt;&gt;= &gt;&gt;&gt;=`</td>

In general-purpose programming, certain operators tend to appear more frequently than others; for example, the assignment operator "`=`" is far more common than the unsigned right shift operator "`&gt;&gt;&gt;`". With that in mind, the following discussion focuses first on the operators that you're most likely to use on a regular basis, and ends focusing on those that are less common. Each discussion is accompanied by sample code that you can compile and run. Studying its output will help reinforce what you've just learned.
