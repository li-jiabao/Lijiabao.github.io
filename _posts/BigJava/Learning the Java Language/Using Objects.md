
# Using Objects

Once you've created an object, you probably want to use it for something. You may need to use the value of one of its fields, change one of its fields, or call one of its methods to perform an action.

<a name="fields" id="fields"></a>

## Referencing an Object's Fields

Object fields are accessed by their name. You must use a name that is unambiguous.

You may use a simple name for a field within its own class. For example, we can add a statement *within* the `Rectangle` class that prints the `width` and `height`:

```

System.out.println("Width and height are: " + width + ", " + height);

```

In this case, `width` and `height` are simple names.

Code that is outside the object's class must use an object reference or expression, followed by the dot (.) operator, followed by a simple field name, as in:

```

objectReference.fieldName

```

For example, the code in the <tt>CreateObjectDemo</tt> class is outside the code for the <tt>Rectangle</tt> class. So to refer to the <tt>origin</tt>, <tt>width</tt>, and <tt>height</tt> fields within the <tt>Rectangle</tt> object named <tt>rectOne</tt>, the <tt>CreateObjectDemo</tt> class must use the names <tt>rectOne.origin</tt>, <tt>rectOne.width</tt>, and <tt>rectOne.height</tt>, respectively. The program uses two of these names to display the <tt>width</tt> and the <tt>height</tt> of <tt>rectOne</tt>:

```

System.out.println("Width of rectOne: "  + rectOne.width);
System.out.println("Height of rectOne: " + rectOne.height);

```

Attempting to use the simple names <tt>width</tt> and <tt>height</tt> from the code in the <tt>CreateObjectDemo</tt> class doesn't make sense &#151; those fields exist only within an object &#151; and results in a compiler error.

Later, the program uses similar code to display information about <tt>rectTwo</tt>. Objects of the same type have their own copy of the same instance fields. Thus, each <tt>Rectangle</tt> object has fields named <tt>origin</tt>, <tt>width</tt>, and <tt>height</tt>. When you access an instance field through an object reference, you reference that particular object's field. The two objects <tt>rectOne</tt> and <tt>rectTwo</tt> in the <tt>CreateObjectDemo</tt> program have different <tt>origin</tt>, <tt>width</tt>, and <tt>height</tt> fields.

To access a field, you can use a named reference to an object, as in the previous examples, or you can use any expression that returns an object reference. Recall that the <tt>new</tt> operator returns a reference to an object. So you could use the value returned from new to access a new object's fields:

```

int height = new Rectangle().height;

```

This statement creates a new <tt>Rectangle</tt> object and immediately gets its height. In essence, the statement calculates the default height of a <tt>Rectangle</tt>. Note that after this statement has been executed, the program no longer has a reference to the created <tt>Rectangle</tt>, because the program never stored the reference anywhere. The object is unreferenced, and its resources are free to be recycled by the Java Virtual Machine. <a name="methods" id="methods"></a>

## Calling an Object's Methods

You also use an object reference to invoke an object's method. You append the method's simple name to the object reference, with an intervening dot operator (.). Also, you provide, within enclosing parentheses, any arguments to the method. If the method does not require any arguments, use empty parentheses.

```

objectReference.methodName(argumentList);

```

or:

```

objectReference.methodName();

```

The <tt>Rectangle</tt> class has two methods: <tt>getArea()</tt> to compute the rectangle's area and <tt>move()</tt> to change the rectangle's origin. Here's the <tt>CreateObjectDemo</tt> code that invokes these two methods:

```

System.out.println("Area of rectOne: " + rectOne.getArea());
...
rectTwo.move(40, 72);

```

The first statement invokes <tt>rectOne</tt>'s `getArea()` method and displays the results. The second line moves <tt>rectTwo</tt> because the <tt>move()</tt> method assigns new values to the object's <tt>origin.x</tt> and <tt>origin.y</tt>.

As with instance fields, **objectReference** must be a reference to an object. You can use a variable name, but you also can use any expression that returns an object reference. The <tt>new</tt> operator returns an object reference, so you can use the value returned from new to invoke a new object's methods:

```

new Rectangle(100, 50).getArea()

```

The expression <tt>new Rectangle(100, 50)</tt> returns an object reference that refers to a <tt>Rectangle</tt> object. As shown, you can use the dot notation to invoke the new <tt>Rectangle</tt>'s <tt>getArea()</tt> method to compute the area of the new rectangle.

Some methods, such as <tt>getArea()</tt>, return a value. For methods that return a value, you can use the method invocation in expressions. You can assign the return value to a variable, use it to make decisions, or control a loop. This code assigns the value returned by <tt>getArea()</tt> to the variable `areaOfRectangle`:

```

int areaOfRectangle = new Rectangle(100, 50).getArea();

```

Remember, invoking a method on a particular object is the same as sending a message to that object. In this case, the object that <tt>getArea()</tt> is invoked on is the rectangle returned by the constructor.

## The Garbage Collector

Some object-oriented languages require that you keep track of all the objects you create and that you explicitly destroy them when they are no longer needed. Managing memory explicitly is tedious and error-prone. The Java platform allows you to create as many objects as you want (limited, of course, by what your system can handle), and you don't have to worry about destroying them. The Java runtime environment deletes objects when it determines that they are no longer being used. This process is called **garbage collection**.

An object is eligible for garbage collection when there are no more references to that object. References that are held in a variable are usually dropped when the variable goes out of scope. Or, you can explicitly drop an object reference by setting the variable to the special value <tt>null</tt>. Remember that a program can have multiple references to the same object; all references to an object must be dropped before the object is eligible for garbage collection.

The Java runtime environment has a garbage collector that periodically frees the memory used by objects that are no longer referenced. The garbage collector does its job automatically when it determines that the time is right.
