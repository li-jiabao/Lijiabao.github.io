
# General Information about Writing Event Listeners

This section discusses several design considerations to keep in mind when implementing event handlers in your application. We then introduce you to event objects small objects that describe each event. In particular, we talk about `EventObject`, the superclass for all AWT and Swing events. Next, we introduce the concepts of low-level events and semantic events, recommending that you prefer semantic events when possible. The remainder of this section discusses implementation techniques you might use in some event listeners or see in event listeners created by other people or by GUI builders.

- [Design Considerations](#design)
- [Getting Event Information: Event Objects](#eventobjects)
- [Concepts: Low-Level Events and Semantic Events](#twokinds)
- [Event Adapters](#eventAdapters)
- [Inner Classes and Anonymous Inner Classes](#innerClasses)
- [The EventHandler Class](#eventHandlers)

<a name="design" id="design"></a>

## <a name="design__1" id="design__1">Design Considerations</a>

The most important rule to keep in mind about event listeners is that they should execute very quickly. Because all drawing and event-listening methods are executed in the same thread, a slow event-listener method can make the program seem unresponsive and slow to repaint itself. If you need to perform some lengthy operation as the result of an event, do it by starting up another thread (or somehow sending a request to another thread) to perform the operation. For help on using threads, see 
[Concurrency in Swing](../concurrency/index.html).

You have many choices on how to implement an event listener. We can not recommend a specific approach because one solution would not suit all situations. However, we can give you some hints and show you some techniques that you might see, even if you do not use the same solution in your program.

For example, you might choose to implement separate classes for different kinds of event listeners. This can be an easy architecture to maintain, but many classes can also mean reduced performance.

When designing your program, you might want to implement your event listeners in a class that is not public, but somewhere more hidden. A private implementation is a more secure implementation.

If you have a very specific kind of simple event listener, you might be able to avoid creating a class at all by using the `EventHandler` class. <a name="eventobjects" id="eventobjects"></a>

## <a name="eventobjects__1" id="eventobjects__1">Getting Event Information: Event Objects</a>

Every event-listener method has a single argument an object that inherits from the 
[`EventObject`](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html) class. Although the argument always descends from `EventObject`, its type is generally specified more precisely. For example, the argument for methods that handle mouse events is an instance of `MouseEvent`, where `MouseEvent` is an indirect subclass of `EventObject`.

The `EventObject` class defines one very useful method:

Note that the `getSource` method returns an `Object`. Event classes sometimes define methods similar to `getSource`, but that have more restricted return types. For example, the `ComponentEvent` class defines a `getComponent` method that just like `getSource` returns the object that fired the event. The difference is that `getComponent` always returns a `Component`. Each how-to page for event listeners mentions whether you should use `getSource` or another method to get the event source.

Often, an event class defines methods that return information about the event. For example, you can query a `MouseEvent` object for information about where the event occurred, how many clicks the user made, which modifier keys were pressed, and so on.

## <a name="twokinds" id="twokinds">Concepts: Low-Level Events and Semantic Events</a>

Events can be divided into two groups: **low-level** events and **semantic** events. Low-level events represent window-system occurrences or low-level input. Everything else is a semantic event.

Examples of low-level events include mouse and key events both of which result directly from user input. Examples of semantic events include action and item events. A semantic event might be triggered by user input; for example, a button customarily fires an action event when the user clicks it, and a text field fires an action event when the user presses *Enter*. However, some semantic events are not triggered by low-level events, at all. For example, a table-model event might be fired when a table model receives new data from a database.

Whenever possible, you should listen for semantic events rather than low-level events. That way, you can make your code as robust and portable as possible. For example, listening for action events on buttons, rather than mouse events, means that the button will react appropriately when the user tries to activate the button using a keyboard alternative or a look-and-feel-specific gesture. When dealing with a compound component such as a combo box, it is imperative that you stick to semantic events, since you have no reliable way of registering listeners on all the look-and-feel-specific components that might be used to form the compound component.

## <a name="eventAdapters" id="eventAdapters">Event Adapters</a>

Some listener interfaces contain more than one method. For example, the `MouseListener` interface contains five methods: `mousePressed`, `mouseReleased`, `mouseEntered`, `mouseExited`, and `mouseClicked`. Even if you care only about mouse clicks, if your class directly implements `MouseListener`, then you must implement all five `MouseListener` methods. Methods for those events you do not care about can have empty bodies. Here is an example:

```

//An example that implements a listener interface directly.
public class MyClass implements MouseListener {
    ...
        someObject.addMouseListener(this);
    ...
    /* Empty method definition. */
    public void mousePressed(MouseEvent e) {
    }

    /* Empty method definition. */
    public void mouseReleased(MouseEvent e) {
    }

    /* Empty method definition. */
    public void mouseEntered(MouseEvent e) {
    }

    /* Empty method definition. */
    public void mouseExited(MouseEvent e) {
    }

    public void mouseClicked(MouseEvent e) {
        **...//Event listener implementation goes here...**
    }
}

```

The resulting collection of empty method bodies can make code harder to read and maintain. To help you avoid implementing empty method bodies, the API generally includes an **adapter** class for each listener interface with more than one method. (The [Listener API Table](api.html) lists all the listeners and their adapters.) For example, the `MouseAdapter` class implements the `MouseListener` interface. An adapter class implements empty versions of all its interface's methods.

To use an adapter, you create a subclass of it and override only the methods of interest, rather than directly implementing all methods of the listener interface. Here is an example of modifying the preceding code to extend `MouseAdapter`. By extending `MouseAdapter`, it inherits empty definitions of all five of the methods that `MouseListener` contains.

```

/*
 * An example of extending an adapter class instead of
 * directly implementing a listener interface.
 */
public class MyClass extends MouseAdapter {
    ... 
        someObject.addMouseListener(this);
    ... 
    public void mouseClicked(MouseEvent e) {
        **...//Event listener implementation goes here...**
    }
}

```

## <a name="innerClasses" id="innerClasses">Inner Classes and Anonymous Inner Classes</a>

What if you want to use an adapter class, but do not want your public class to inherit from an adapter class? For example, suppose you write an applet, and you want your `Applet` subclass to contain some code to handle mouse events. Since the Java language does not permit multiple inheritance, your class cannot extend both the `Applet` and `MouseAdapter` classes. A solution is to define an **inner class** a class inside of your `Applet` subclass that extends the `MouseAdapter` class.

Inner classes can also be useful for event listeners that implement one or more interfaces directly.

```

//An example of using an inner class.
public class MyClass extends Applet {
    ...
        someObject.addMouseListener(new MyAdapter());
    ...
    class MyAdapter extends MouseAdapter {
        public void mouseClicked(MouseEvent e) {
            **...//Event listener implementation goes here...**
        }
    }
}

```

When considering whether to use an inner class, keep in mind that application startup time and memory footprint are typically directly proportional to the number of classes you load. The more classes you create, the longer your program takes to start up and the more memory it will take. As an application developer you have to balance this with other design constraints you may have. We are not suggesting you turn your application into a single monolithic class in hopes of cutting down startup time and memory footprint this would lead to unnecessary headaches and maintenance burdens.

You can create an inner class without specifying a name this is known as an **anonymous inner class**. While it might look strange at first glance, anonymous inner classes can make your code easier to read because the class is defined where it is referenced. However, you need to weigh the convenience against possible performance implications of increasing the number of classes.

Here is an example of using an anonymous inner class:

```

//An example of using an anonymous inner class.
public class MyClass extends Applet {
    ...
        someObject.addMouseListener(new MouseAdapter() {
            public void mouseClicked(MouseEvent e) {
                **...//Event listener implementation goes here...**
            }
        });
    ...
    }
}

```

One drawback of anonymous inner classes is that they can not be seen by the long-term persistence mechanism. For more information see the API documentation for the 
[JavaBeans&#8482; package](https://docs.oracle.com/javase/8/docs/api/java/beans/package-summary.html#package_description) and the 
[Bean Persistence](../../javabeans/advanced/persistence.html) lesson in the 
[JavaBeans](../../javabeans/) trail.

Inner classes work even if your event listener needs access to private instance variables from the enclosing class. As long as you do not declare an inner class to be `static`, an inner class can refer to instance variables and methods just as if its code is in the containing class. To make a local variable available to an inner class, just save a copy of the variable as a `final` local variable.

To refer to the enclosing instance, you can use `**EnclosingClass**.this`. For more information about inner classes, see 
[Nested Classes](../../java/javaOO/nested.html).

## <a name="eventHandlers" id="eventHandlers">The EventHandler Class</a>

An 
[`EventHandler`](https://docs.oracle.com/javase/8/docs/api/java/beans/EventHandler.html) class supports dynamic generation of simple, one-statement event listeners. Although `EventHandler` is only useful for a certain type of extremely simple event listeners, it is worth mentioning for two reasons. It is useful for:

- Making an event listener that persistence can see and yet does not clog up your own classes with event listener interfaces and methods.
- Not adding to the number of classes defined in an application this can help performance.

Creating an `EventHandler` by hand is difficult. An `EventHandler` must be carefully constructed. If you make a mistake, you would not be notified at compile time it will throw an obscure exception at runtime. For this reason, `EventHandler`s are best created by a GUI builder. `EventHandler`s should be carefully documented. Otherwise you run the risk of producing hard-to-read code.

The `EventHandler` class is intended to be used by interactive tools, such as application builders, that allow developers to make connections between beans. Typically connections are made from a user interface bean (the event source) to an application logic bean (the target). The most effective connections of this kind isolate the application logic from the user interface. For example, the `EventHandler` for a connection from a JCheckBox to a method that accepts a boolean value can deal with extracting the state of the check box and passing it directly to the method so that the method is isolated from the user interface layer.

Inner classes are another, more general way to handle events from user interfaces. The `EventHandler` class handles only a subset of what is possible using inner classes. However, `EventHandler` works better with the long-term persistence scheme than inner classes. Also, using `EventHandler` in large applications in which the same interface is implemented many times can reduce the disk and memory footprint of the application.

Examples of Using `EventHandler` The simplest use of `EventHandler` is to install a listener that calls a method on the target object with no arguments. In the following example we create an ActionListener that invokes the toFront method on an instance of `javax.swing.JFrame`.

```

    myButton.addActionListener(
        (ActionListener)EventHandler.create(ActionListener.class, frame, "toFront"));

```

When myButton is pressed, the statement frame.toFront() will be executed. One could get the same effect, with some additional compile-time type safety, by defining a new implementation of the ActionListener interface and adding an instance of it to the button:

```

    //Equivalent code using an inner class instead of `EventHandler`.
    myButton.addActionListener(new ActionListener() {
        public void actionPerformed(ActionEvent e) {
            frame.toFront();
        }
    });

```

The next simplest use of `EventHandler` is to extract a property value from the first argument of the method in the listener interface (typically an event object) and use it to set the value of a property in the target object. In the following example we create an ActionListener that sets the nextFocusableComponent property of the target (myButton) object to the value of the "source" property of the event.

```

    EventHandler.create(ActionListener.class, myButton, "nextFocusableComponent", "source")

```

This would correspond to the following inner class implementation:

```

    //Equivalent code using an inner class instead of `EventHandler`.
    new ActionListener() {
        public void actionPerformed(ActionEvent e) {
            myButton.setNextFocusableComponent((Component)e.getSource()); 
        }
    }

```

It is also possible to create an `EventHandler` that just passes the incoming event object to the target's action. If the fourth `EventHandler.create` argument is an empty string, then the event is just passed along:

```

    EventHandler.create(ActionListener.class, target, "doActionEvent", "")

```

This would correspond to the following inner class implementation:

```

    //Equivalent code using an inner class instead of `EventHandler`.
    new ActionListener() {
        public void actionPerformed(ActionEvent e) {
            target.doActionEvent(e);
        }
    }

```

Probably the most common use of `EventHandler` is to extract a property value from the source of the event object and set this value as the value of a property of the target object. In the following example we create an ActionListener that sets the "label" property of the target object to the value of the "text" property of the source (the value of the "source" property) of the event.

```

    EventHandler.create(ActionListener.class, myButton, "label", "source.text")

```

This would correspond to the following inner class implementation:

```

    //Equivalent code using an inner class instead of `EventHandler`.
    new ActionListener {
        public void actionPerformed(ActionEvent e) {
            myButton.setLabel(((JTextField)e.getSource()).getText()); 
        }
    }

```
