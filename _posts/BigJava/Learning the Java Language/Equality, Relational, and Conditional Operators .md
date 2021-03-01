
# Equality, Relational, and Conditional Operators 

## The Equality and Relational Operators

The equality and relational operators determine if one operand is greater than, less than, equal to, or not equal to another operand. The majority of these operators will probably look familiar to you as well. Keep in mind that you must use "`==`", not "`=`", when testing if two primitive values are equal.

```

==      equal to
!=      not equal to
&gt;       greater than
&gt;=      greater than or equal to
&lt;       less than
&lt;=      less than or equal to

```

The following program, 
[`ComparisonDemo`](examples/ComparisonDemo.java), tests the comparison operators:

```


class ComparisonDemo {

    public static void main(String[] args){
        int value1 = 1;
        int value2 = 2;
        if(value1 == value2)
            System.out.println("value1 == value2");
        if(value1 != value2)
            System.out.println("value1 != value2");
        if(value1 &gt; value2)
            System.out.println("value1 &gt; value2");
        if(value1 &lt; value2)
            System.out.println("value1 &lt; value2");
        if(value1 &lt;= value2)
            System.out.println("value1 &lt;= value2");
    }
}

```

Output:

```

value1 != value2
value1 &lt;  value2
value1 &lt;= value2

```

## The Conditional Operators

The `&amp;&amp;` and `||` operators perform **Conditional-AND** and **Conditional-OR** operations on two boolean expressions. These operators exhibit "short-circuiting" behavior, which means that the second operand is evaluated only if needed.

```

&amp;&amp; Conditional-AND
|| Conditional-OR

```

The following program, 
[`ConditionalDemo1`](examples/ConditionalDemo1.java), tests these operators:

```


class ConditionalDemo1 {

    public static void main(String[] args){
        int value1 = 1;
        int value2 = 2;
        if((value1 == 1) &amp;&amp; (value2 == 2))
            System.out.println("value1 is 1 AND value2 is 2");
        if((value1 == 1) || (value2 == 1))
            System.out.println("value1 is 1 OR value2 is 1");
    }
}

```

Another conditional operator is `?:`, which can be thought of as shorthand for an `if-then-else` statement (discussed in the 
[Control Flow Statements](flow.html) section of this lesson). This operator is also known as the *ternary operator* because it uses three operands. In the following example, this operator should be read as: "If `someCondition` is `true`, assign the value of `value1` to `result`. Otherwise, assign the value of `value2` to `result`."

The following program, 
[`ConditionalDemo2`](examples/ConditionalDemo2.java), tests the `?:` operator:

```


class ConditionalDemo2 {

    public static void main(String[] args){
        int value1 = 1;
        int value2 = 2;
        int result;
        boolean someCondition = true;
        result = someCondition ? value1 : value2;

        System.out.println(result);
    }
}

```

Because `someCondition` is true, this program prints "1" to the screen. Use the `?:` operator instead of an `if-then-else` statement if it makes your code more readable; for example, when the expressions are compact and without side-effects (such as assignments).

## The Type Comparison Operator instanceof

The `instanceof` operator compares an object to a specified type. You can use it to test if an object is an instance of a class, an instance of a subclass, or an instance of a class that implements a particular interface.

The following program, 
[`InstanceofDemo`](examples/InstanceofDemo.java), defines a parent class (named `Parent`), a simple interface (named `MyInterface`), and a child class (named `Child`) that inherits from the parent and implements the interface.

```


class InstanceofDemo {
    public static void main(String[] args) {

        Parent obj1 = new Parent();
        Parent obj2 = new Child();

        System.out.println("obj1 instanceof Parent: "
            + (obj1 instanceof Parent));
        System.out.println("obj1 instanceof Child: "
            + (obj1 instanceof Child));
        System.out.println("obj1 instanceof MyInterface: "
            + (obj1 instanceof MyInterface));
        System.out.println("obj2 instanceof Parent: "
            + (obj2 instanceof Parent));
        System.out.println("obj2 instanceof Child: "
            + (obj2 instanceof Child));
        System.out.println("obj2 instanceof MyInterface: "
            + (obj2 instanceof MyInterface));
    }
}

class Parent {}
class Child extends Parent implements MyInterface {}
interface MyInterface {}

```

Output:

```

obj1 instanceof Parent: true
obj1 instanceof Child: false
obj1 instanceof MyInterface: false
obj2 instanceof Parent: true
obj2 instanceof Child: true
obj2 instanceof MyInterface: true

```

When using the `instanceof` operator, keep in mind that `null` is not an instance of anything.
