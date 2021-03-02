
# Lambda Expressions

One issue with anonymous classes is that if the implementation of your anonymous class is very simple, such as an interface that contains only one method, then the syntax of anonymous classes may seem unwieldy and unclear. In these cases, you're usually trying to pass functionality as an argument to another method, such as what action should be taken when someone clicks a button. Lambda expressions enable you to do this, to treat functionality as method argument, or code as data.

The previous section,
[Anonymous Classes](anonymousclasses.html), shows you how to implement a base class without giving it a name.
Although this is often more concise than a named class, for classes
with only one method, even an anonymous class seems a bit
excessive and cumbersome. Lambda expressions let you express instances of
single-method classes more compactly.

This section covers the following topics:

  <li>[Ideal Use Case for Lambda Expressions](#use-case)
    <ul>
      - [Approach 1: Create Methods That Search for Members That Match One Characteristic](#approach1)
      - [Approach 2: Create More Generalized Search Methods](#approach2)
      - [Approach 3: Specify Search Criteria Code in a Local Class](#approach3)
      - [Approach 4: Specify Search Criteria Code in an Anonymous Class](#approach4)
      - [Approach 5: Specify Search Criteria Code with a Lambda Expression](#approach5)
      - [Approach 6: Use Standard Functional Interfaces with Lambda Expressions](#approach6)
      - [Approach 7: Use Lambda Expressions Throughout Your Application](#approach7)
      - [Approach 8: Use Generics More Extensively](#approach8)
      - [Approach 9: Use Aggregate Operations That Accept Lambda Expressions as Parameters](#approach9)
    
      - [Target Types and Method Arguments](#target-types-and-method-arguments)
    
## <a name="use-case" id="use-case">Ideal Use Case for Lambda Expressions</a>

Suppose that you are creating a social networking application. You
want to create a feature that enables an administrator to perform
any kind of action, such as sending a message, on members of the
social networking application that satisfy certain criteria. The following table describes this use case in detail:
    <th id="h1">Field</th>    <th id="h2">Description</th>  
    <td headers="h1">Name</td>    <td headers="h2">Perform action on selected members</td>  
    <td headers="h1">Primary Actor</td>    <td headers="h2">Administrator</td>  
     <td headers="h1">Preconditions</td>     <td headers="h2">Administrator is logged in to the system.</td>  
    <td headers="h1">Postconditions</td>    <td headers="h2">Action is performed only on members that fit the specified criteria.</td>  
    <td headers="h1">Main Success Scenario</td>    <td headers="h2">              - Administrator specifies criteria of members on which to perform a certain action.        - Administrator specifies an action to perform on those selected members.        - Administrator selects the **Submit** button.        - The system finds all members that match the specified criteria.        - The system performs the specified action on all matching members.          </td>  
    <td headers="h1">Extensions</td>    <td headers="h2">      1a. Administrator has an option to preview those members who match the specified criteria before he or she specifies the action to be performed or before selecting the **Submit** button.    </td>  
    <td headers="h1">Frequency of Occurrence</td>    <td headers="h2">Many times during the day.</td>  

Suppose that members of this social networking application are
represented by the following
[`Person`](examples/Person.java) class:

```
RosterTest</code></a>.
</p>

<h3><a name="approach1" id="approach1">Approach 1: Create Methods That Search for Members That Match One Characteristic</a></h3>

<p>One simplistic approach is to create several methods; each method searches for members that match one characteristic, such as gender or age. The following method prints members that are older than a specified
age:</p>


<pre class="codeblock">public static void printPersonsOlderThan(List&lt;Person&gt; roster, int age) {
    for (Person p : roster) {
        if (p.getAge() &gt;= age) {
            p.printPerson();
        }
    }
}
```

**Note**: A
[`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) is an ordered
[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html). A **collection** is an object
that groups multiple elements into a single unit. Collections are
used to store, retrieve, manipulate, and communicate aggregate
data. For more information about collections, see the
[Collections](../../collections/index.html) trail.

This approach can potentially make your application **brittle**, which is the likelihood of an application not working because of the introduction of updates (such as newer data types). Suppose that you upgrade your application and change the structure of the `Person` class such that it contains different member variables; perhaps the class records and measures ages with a different data type or algorithm. You would have to rewrite a lot of your API to accommodate this change. In addition, this approach is unnecessarily restrictive; what if you
wanted to print members younger than a certain age, for example?

### <a name="approach2" id="approach2">Approach 2: Create More Generalized Search Methods</a>

The following method is more generic than `printPersonsOlderThan`; it prints members within a specified range of ages:

```
processElements</code> Action</th>
  <th id="h102">Aggregate Operation</th>
</tr>

<tr>
  <td headers="h101">Obtain a source of objects</td>
  
  <td headers="h102">`Stream&lt;E&gt; **stream**()`</td>
</tr>

<tr>
  <td headers="h101">Filter objects that match a `Predicate` object</td>
  <td headers="h102">`Stream&lt;T&gt; **filter**(Predicate&lt;? super T&gt; predicate)`</td>
</tr>

<tr>

  <td headers="h101">Map objects to another value as specified by a `Function` object</td>
  
  <td headers="h102">`&lt;R&gt; Stream&lt;R&gt; **map**(Function&lt;? super T,? extends R&gt; mapper)`</td>
  
</tr>

<tr>
  <td headers="h101">Perform an action as specified by a `Consumer` object</td>
  <td headers="h102">`void **forEach**(Consumer&lt;? super T&gt; action)`</td>
</tr>

</table>

The operations `filter`, `map`, and `forEach` are **aggregate operations**. Aggregate operations process elements from a stream, not directly from a collection (which is the reason why the first method invoked in this example is `stream`). A **stream** is a sequence of elements. Unlike a collection, it is not a data structure that stores elements. Instead, a stream carries values from a source, such as collection, through a pipeline. A **pipeline** is a sequence of stream operations, which in this example is `filter`- `map`-`forEach`. In addition, aggregate operations typically accept lambda expressions as parameters, enabling you to customize how they behave.

<p>For a more thorough discussion of aggregate operations, see the
[Aggregate Operations](../../collections/streams/index.html) lesson.</p>


<h2><a name="lambda-expressions-in-gui-applications" id="lambda-expressions-in-gui-applications">Lambda Expressions in GUI Applications</a></h2>

<p>To process events in a graphical user interface (GUI) application, such as keyboard
actions, mouse actions, and scroll actions, you typically create
event handlers, which usually involves implementing a particular
interface. Often, event handler interfaces are functional
interfaces; they tend to have only one method.</p>

<p>In the JavaFX example <a href="https://docs.oracle.com/javase/8/javafx/get-started-tutorial/hello_world.htm">
`HelloWorld.java`</a> (discussed in the previous section
[Anonymous Classes](anonymousclasses.html)), you can
replace the highlighted anonymous class with a lambda expression in this
statement:</p>

<pre class="codeblock">        btn.setOnAction(**new EventHandler&lt;ActionEvent&gt;() {**

            **@Override**
            **public void handle(ActionEvent event) {**
                **System.out.println("Hello World!");**
            **}**
        **}**);
```

### <a name="approach4" id="approach4">Approach 4: Specify Search Criteria Code in an Anonymous Class</a>

### <a name="approach6" id="approach6">Approach 6: Use Standard Functional Interfaces with Lambda Expressions</a>

### <a name="approach8" id="approach8">Approach 8: Use Generics More Extensively</a>

The method invocation `btn.setOnAction` specifies what
happens when you select the button represented by the `btn` object. This method requires
an object of type `EventHandler&lt;ActionEvent&gt;`. The `EventHandler&lt;ActionEvent&gt;`
interface contains only one method, `void handle(T event)`.
This interface is a functional interface, so you could use the following highlighted lambda expression to replace it:

```
Calculator</code></a>, is an example of lambda expressions that take
more than one formal parameter:</p>

<pre class="codeblock">

public class Calculator {
  
    interface IntegerMath {
        int operation(int a, int b);   
    }
  
    public int operateBinary(int a, int b, IntegerMath op) {
        return op.operation(a, b);
    }
 
    public static void main(String... args) {
    
        Calculator myApp = new Calculator();
        IntegerMath addition = (a, b) -&gt; a + b;
        IntegerMath subtraction = (a, b) -&gt; a - b;
        System.out.println("40 + 2 = " +
            myApp.operateBinary(40, 2, addition));
        System.out.println("20 - 10 = " +
            myApp.operateBinary(20, 10, subtraction));    
    }
}


```

The method `operateBinary` performs a
mathematical operation on two integer operands. The operation
itself is specified by an instance of `IntegerMath`. The example defines two operations with lambda expressions, `addition` and `subtraction`. The example prints
the following:

```
LambdaScopeTest</code></a>, demonstrates this:</p>

<pre class="codeblock">

import java.util.function.Consumer;

public class LambdaScopeTest {

    public int x = 0;

    class FirstLevel {

        public int x = 1;

        void methodInFirstLevel(int x) {
            
            // The following statement causes the compiler to generate
            // the error "local variables referenced from a lambda expression
            // must be final or effectively final" in statement A:
            //
            // x = 99;
            
            Consumer&lt;Integer&gt; myConsumer = (y) -&gt; 
            {
                System.out.println("x = " + x); // Statement A
                System.out.println("y = " + y);
                System.out.println("this.x = " + this.x);
                System.out.println("LambdaScopeTest.this.x = " +
                    LambdaScopeTest.this.x);
            };

            myConsumer.accept(x);

        }
    }

    public static void main(String... args) {
        LambdaScopeTest st = new LambdaScopeTest();
        LambdaScopeTest.FirstLevel fl = st.new FirstLevel();
        fl.methodInFirstLevel(23);
    }
}

```

This example generates the following output:

```
public static void printPersons(List&lt;Person&gt; roster, CheckPerson tester)</code> in [Approach 3: Specify Search Criteria Code in a Local Class](#approach3)</p></li>

- `public void printPersonsWithPredicate(List&lt;Person&gt; roster, Predicate&lt;Person&gt; tester)` in [Approach 6: Use Standard Functional Interfaces with Lambda Expressions](#approach6)

</ul>

<p>When the Java runtime invokes the method `printPersons`, it's expecting a data type of `CheckPerson`, so the
lambda expression is of this type. However,
when the Java runtime invokes the method `printPersonsWithPredicate`,
it's expecting a data type of `Predicate&lt;Person&gt;`,
so the lambda expression is of this type. The data type that
these methods expect is called the **target type**. To determine the type of a lambda
expression, the Java compiler uses the target type of the context
or situation in which the lambda expression was found. It follows
that you can only use lambda expressions in situations in which
the Java compiler can determine a target type:</p>

<ul>
  - Variable declarations
  - Assignments
  - Return statements
  - Array initializers
  - Method or constructor arguments
  - Lambda expression bodies
  - Conditional expressions, `?:`
  - Cast expressions
</ul>

<h3><a name="target-types-and-method-arguments" id="target-types-and-method-arguments">Target Types and Method Arguments</a></h3>

<p>For method arguments, the Java compiler determines the target
type with two other language features: overload resolution and
type argument inference.</p>

<p>Consider the following two functional interfaces (
[`java.lang.Runnable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html) and
[`java.util.concurrent.Callable&lt;V&gt;`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Callable.html)):</p>

<pre class="codeblock">public interface Runnable {
    void run();
}

public interface Callable&lt;V&gt; {
    V call();
}
```

  - Variable declarations
  - Assignments
  - Return statements
  - Array initializers
  - Method or constructor arguments
  - Lambda expression bodies
  - Conditional expressions, `?:`
  - Cast expressions

The method `Runnable.run` does not return a value, whereas `Callable&lt;V&gt;.call` does.

Suppose that you have overloaded the method `invoke` as follows
(see
[Defining Methods](methods.html) for more information about overloading methods):

Which method will be invoked in the following statement?

The method `invoke(Callable&lt;T&gt;)` will be
invoked because that method returns a value; the method
`invoke(Runnable)` does not. In this case, the type of the lambda expression `() -&gt; "done"` is `Callable&lt;T&gt;`.

## <a name="serialization" id="serialization">Serialization</a>

You can 
[serialize](../../jndi/objects/serial.html) a lambda expression if its target type and its captured arguments are serializable. However, like
[inner classes](nested.html#serialization), the serialization of lambda expressions is strongly discouraged.
