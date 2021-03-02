
# Anonymous Classes

Anonymous classes enable you to make your code more concise.
They enable you to declare and instantiate a class at the same
time. They are like local classes except that they do not have a
name. Use them if you need to use a local class only once.

This section covers the following topics:

  - [Declaring Anonymous Classes](#declaring-anonymous-classes)
  - [Syntax of Anonymous Classes](#syntax-of-anonymous-classes)
  - [Accessing Local Variables of the Enclosing Scope, and Declaring and Accessing Members of the Anonymous Class](#accessing)
  - [Examples of Anonymous Classes](#examples-of-anonymous-classes)

## <a name="declaring-anonymous-classes" id="declaring-anonymous-classes">Declaring Anonymous Classes</a>

While local classes are class declarations, anonymous classes
are expressions, which means that you define the class in another
expression. The following example,
[`HelloWorldAnonymousClasses`](examples/HelloWorldAnonymousClasses.java), uses anonymous classes in the initialization statements of the
local variables `frenchGreeting` and `spanishGreeting`, but uses a local class for the
initialization of the variable `englishGreeting``:`

```
        HelloWorld frenchGreeting = new HelloWorld() {
            String name = "tout le monde";
            public void greet() {
                greetSomeone("tout le monde");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Salut " + name);
            }
        };
```

The anonymous class expression consists of the following:

  - The `new` operator
  <li><p>The name of an interface to
implement or a class to extend. In this example, the anonymous
class is implementing the interface `HelloWorld`.</p></li>
  <li><p>Parentheses that contain the
arguments to a constructor, just like a normal class instance
creation expression. **Note**: When you implement an
interface, there is no constructor, so you use an empty pair of
parentheses, as in this example.</p></li>
  <li><p>A body, which is a class
declaration body. More specifically, in the body, method
declarations are allowed but statements are not.</p></li>

Because an anonymous class
definition is an expression, it must be part of a statement. In
this example, the anonymous class expression is part of the
statement that instantiates the `frenchGreeting` object. (This
explains why there is a semicolon after the closing brace.)

## <a name="accessing" id="accessing">Accessing Local Variables of the Enclosing Scope, and Declaring and Accessing Members of the Anonymous Class</a>

Like local classes, anonymous classes can
[capture variables](localclasses.html#accessing-members-of-an-enclosing-class); they have the same access to local variables of
the enclosing scope:

  <li><p>An anonymous class has access to the members of its enclosing
class.</p></li>
  <li><p>An anonymous class cannot access local variables in its
enclosing scope that are not declared as `final` or effectively final.</p></li>
  <li><p>Like a nested class, a declaration of a type (such as a variable) in an anonymous class shadows any other declarations in the enclosing scope that have the same name. See
[Shadowing](../../java/javaOO/nested.html#shadowing) for more information.</p></li>


Anonymous classes also have the same restrictions as local
classes with respect to their members:

  <li><p>You cannot declare static
initializers or member interfaces in an anonymous
class.</p></li>
  <li><p>An anonymous class can
have static members provided that they are constant
variables.</p></li>

Note that you can declare the following in anonymous classes:

  - Fields
  - Extra methods (even if they do not implement any methods of the supertype)
  - Instance initializers
  - Local classes

However, you cannot declare constructors in an anonymous class.

## <a name="examples-of-anonymous-classes" id="examples-of-anonymous-classes">Examples of Anonymous Classes</a>

Anonymous classes are often used in graphical user
interface (GUI) applications.

Consider the JavaFX example <a href="https://docs.oracle.com/javase/8/javafx/get-started-tutorial/hello_world.htm">
`HelloWorld.java`</a> (from the section <a href="https://docs.oracle.com/javase/8/javafx/get-started-tutorial/hello_world.htm">
Hello World, JavaFX Style</a> from <a href="https://docs.oracle.com/javase/8/javafx/get-started-tutorial/javafx_get_started.htm">
Getting Started with JavaFX</a>). This
sample creates a frame that contains a **Say 'Hello World'**
button. The anonymous class
expression is highlighted:

```
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;
 
public class HelloWorld extends Application {
    public static void main(String[] args) {
        launch(args);
    }
    
    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Hello World!");
        Button btn = new Button();
        btn.setText("Say 'Hello World'");
        btn.setOnAction(**new EventHandler&lt;ActionEvent&gt;() {**
 
            **@Override**
            **public void handle(ActionEvent event) {**
                **System.out.println("Hello World!");**
            **}**
        **}**);
        
        StackPane root = new StackPane();
        root.getChildren().add(btn);
        primaryStage.setScene(new Scene(root, 300, 250));
        primaryStage.show();
    }
}
```

In this example, the method invocation `btn.setOnAction` specifies what happens when you select the **Say 'Hello World'** button. This method requires an object of type `EventHandler&lt;ActionEvent&gt;`. The `EventHandler&lt;ActionEvent&gt;` interface contains only one method, handle. Instead of implementing this method with a new class, the example uses an anonymous class expression. Notice that this expression is the argument passed to the `btn.setOnAction` method.

Because the
`EventHandler&lt;ActionEvent&gt;`
interface contains only one method, you can use a lambda
expression instead of an anonymous class expression. See the
section
[Lambda Expressions](lambdaexpressions.html) for more
information.

Anonymous classes are ideal for implementing an interface that contains two or more methods. The following JavaFX example is from the section [ Customization of UI Controls](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/custom.htm). The highlighted code creates a text field that only accepts numeric values. It redefines the default implementation of the `TextField` class with an anonymous class by overriding the `replaceText` and `replaceSelection` methods inherited from the `TextInputControl` class.
