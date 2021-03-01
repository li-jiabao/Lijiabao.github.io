
# Creating a Package

To create a package, you choose a name for the package (naming conventions are discussed in the next section) and put a `package` statement with that name at the top of *every source file* that contains the types (classes, interfaces, enumerations, and annotation types) that you want to include in the package.

The package statement (for example, `package graphics;`) must be the first line in the source file. There can be only one package statement in each source file, and it applies to all types in the file.

If you put the graphics interface and classes listed in the preceding section in a package called `graphics`, you would need six source files, like this:

```

//**in the Draggable.java file**
package graphics;
public interface Draggable {
    . . .
}

//**in the Graphic.java file**
package graphics;
public abstract class Graphic {
    . . .
}

//**in the Circle.java file**
package graphics;
public class Circle extends Graphic
    implements Draggable {
    . . .
}

//**in the Rectangle.java file**
package graphics;
public class Rectangle extends Graphic
    implements Draggable {
    . . .
}

//**in the Point.java file**
package graphics;
public class Point extends Graphic
    implements Draggable {
    . . .
}

//**in the Line.java file**
package graphics;
public class Line extends Graphic
    implements Draggable {
    . . .
}

```

If you do not use a `package` statement, your type ends up in an unnamed package. Generally speaking, an unnamed package is only for small or temporary applications or when you are just beginning the development process. Otherwise, classes and interfaces belong in named packages.
