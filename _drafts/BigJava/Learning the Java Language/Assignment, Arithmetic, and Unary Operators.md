
# Assignment, Arithmetic, and Unary Operators

## The Simple Assignment Operator

One of the most common operators that you'll encounter is the simple assignment operator "`=`". You saw this operator in the Bicycle class; it assigns the value on its right to the operand on its left:

```

 int cadence = 0;
 int speed = 0;
 int gear = 1;

```

This operator can also be used on objects to assign *object references*, as discussed in 
[Creating Objects](../javaOO/objectcreation.html).

## The Arithmetic Operators

The Java programming language provides operators that perform addition, subtraction, multiplication, and division. There's a good chance you'll recognize them by their counterparts in basic mathematics. The only symbol that might look new to you is "`%`", which divides one operand by another and returns the remainder as its result.
    <th id="h1">Operator</th>    <th id="h2">Description</th>  
    <td headers="h1">`+`</td>|Additive operator (also used for String concatenation)  
    <td headers="h1">`-`</td>|Subtraction operator  
    <td headers="h1">`*`</td>|Multiplication operator  
    <td headers="h1">`/`</td>|Division operator  
    <td headers="h1">`%`</td>|Remainder operator  

The following program, 
[`ArithmeticDemo`](examples/ArithmeticDemo.java), tests the arithmetic operators.

```


class ArithmeticDemo {

    public static void main (String[] args) {

        int result = 1 + 2;
        // result is now 3
        System.out.println("1 + 2 = " + result);
        int original_result = result;

        result = result - 1;
        // result is now 2
        System.out.println(original_result + " - 1 = " + result);
        original_result = result;

        result = result * 2;
        // result is now 4
        System.out.println(original_result + " * 2 = " + result);
        original_result = result;

        result = result / 2;
        // result is now 2
        System.out.println(original_result + " / 2 = " + result);
        original_result = result;

        result = result + 8;
        // result is now 10
        System.out.println(original_result + " + 8 = " + result);
        original_result = result;

        result = result % 7;
        // result is now 3
        System.out.println(original_result + " % 7 = " + result);
    }
}


```

This program prints the following:

```

1 + 2 = 3
3 - 1 = 2
2 * 2 = 4
4 / 2 = 2
2 + 8 = 10
10 % 7 = 3

```

You can also combine the arithmetic operators with the simple assignment operator to create *compound assignments*. For example, `x+=1;` and `x=x+1;` both increment the value of `x` by 1.

The `+` operator can also be used for concatenating (joining) two strings together, as shown in the following 
[`ConcatDemo`](examples/ConcatDemo.java) program:

```


class ConcatDemo {
    public static void main(String[] args){
        String firstString = "This is";
        String secondString = " a concatenated string.";
        String thirdString = firstString+secondString;
        System.out.println(thirdString);
    }
}

```

By the end of this program, the variable `thirdString` contains "This is a concatenated string.", which gets printed to standard output.

## The Unary Operators

The unary operators require only one operand; they perform various operations such as incrementing/decrementing a value by one, negating an expression, or inverting the value of a boolean.
    <th id="h101">Operator</th>    <th id="h102">Description</th>  
    <td headers="h101">`+`</td>|Unary plus operator; indicates         positive value (numbers are         positive without this, however)  
    <td headers="h101">`-`</td>|Unary minus operator; negates        an expression  
    <td headers="h101">`++`</td>|Increment operator; increments        a value by 1  
    <td headers="h101">`--`</td>|Decrement operator; decrements        a value by 1  
    <td headers="h101">`!`</td>|Logical complement operator;         inverts the value of a boolean  

The following program, 
[`UnaryDemo`](examples/UnaryDemo.java), tests the unary operators:

```


class UnaryDemo {

    public static void main(String[] args) {

        int result = +1;
        // result is now 1
        System.out.println(result);

        result--;
        // result is now 0
        System.out.println(result);

        result++;
        // result is now 1
        System.out.println(result);

        result = -result;
        // result is now -1
        System.out.println(result);

        boolean success = false;
        // false
        System.out.println(success);
        // true
        System.out.println(!success);
    }
}

```

The increment/decrement operators can be applied before (prefix) or after (postfix) the operand. The code `result++;` and `++result;` will both end in `result` being incremented by one. The only difference is that the prefix version (`++result`) evaluates to the incremented value, whereas the postfix version (`result++`) evaluates to the original value. If you are just performing a simple increment/decrement, it doesn't really matter which version you choose. But if you use this operator in part of a larger expression, the one that you choose may make a significant difference.

The following program, 
[`PrePostDemo`](examples/PrePostDemo.java), illustrates the prefix/postfix unary increment operator:

```


class PrePostDemo {
    public static void main(String[] args){
        int i = 3;
        i++;
        // prints 4
        System.out.println(i);
        ++i;			   
        // prints 5
        System.out.println(i);
        // prints 6
        System.out.println(++i);
        // prints 6
        System.out.println(i++);
        // prints 7
        System.out.println(i);
    }
}

```
