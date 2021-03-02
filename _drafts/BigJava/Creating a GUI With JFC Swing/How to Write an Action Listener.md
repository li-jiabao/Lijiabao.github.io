
# How to Write an Action Listener

Action listeners are probably the easiest &#8212; and most common &#8212; event handlers to implement. You implement an action listener to define what should be done when an user performs certain operation.

An action event occurs, whenever an action is performed by the user. Examples: When the user clicks a 
[button](../components/button.html), chooses a 
[menu item](../components/menu.html), presses Enter in a 
[text field](../components/textfield.html). The result is that an `actionPerformed` message is sent to all action listeners that are registered on the relevant component.

To write an Action Listener, follow the steps given below:

<li>Declare an event handler class and specify that the class either implements an ActionListener interface or extends a class that implements an ActionListener interface. For example:
<pre><code>
public class MyClass implements ActionListener { 
</code></pre>
</li>
<li>Register an instance of the event handler class as a listener on one or more components. For example:
<pre><code>
someComponent.addActionListener(instanceOfMyClass);
</code></pre>
</li>
<li>Include code that implements the methods in listener interface. For example:
<pre><code>
public void actionPerformed(ActionEvent e) { 
    ...//code that reacts to the action... 
}
</code></pre>
</li>

In general, to detect when the user clicks an onscreen button (or does the keyboard equivalent), a program must have an object that implements the ActionListener interface. The program must register this object as an action listener on the button (the event source), using the addActionListener method. When the user clicks the onscreen button, the button fires an action event. This results in the invocation of the action listener's actionPerformed method (the only method in the ActionListener interface). The single argument to the method is an ActionEvent object that gives information about the event and its source.

Let us write a simple program which displays how many number of times a button is clicked by the user. First, here is the code that sets up the TextField , button and numClicks variable:

```

public class AL extends Frame implements WindowListener,ActionListener {
TextField text = new TextField(20);
Button b;
private int numClicks = 0;

```

In the above example, the event handler class is AL which implements ActionListener.

We would like to handle the button-click event, so we add an action listener to the button b as below:

```

b = new Button("Click me");
b.addActionListener(this); 

```

In the above code, Button b is a component upon which an instance of event handler class AL is registered.

Now, we want to display the text as to how many number of times a user clicked button. We can do this by writing the code as below:

```

public void actionPerformed(ActionEvent e) {
         numClicks++;
         text.setText("Button Clicked " + numClicks + " times");

```

Now, when the user clicks the Button b, the button fires an action event which invokes the action listener's actionPerformed method. Each time the user presses the button, numClicks variable is appended and the message is displayed in the text field.

Here is the complete program(AL.java):

```


import java.awt.*;
import java.awt.event.*;

public class AL extends Frame implements WindowListener,ActionListener {
        TextField text = new TextField(20);
        Button b;
        private int numClicks = 0;

        public static void main(String[] args) {
                AL myWindow = new AL("My first window");
                myWindow.setSize(350,100);
                myWindow.setVisible(true);
        }

        public AL(String title) {

                super(title);
                setLayout(new FlowLayout());
                addWindowListener(this);
                b = new Button("Click me");
                add(b);
                add(text);
                b.addActionListener(this);
        }

        public void actionPerformed(ActionEvent e) {
                numClicks++;
                text.setText("Button Clicked " + numClicks + " times");
        }

        public void windowClosing(WindowEvent e) {
                dispose();
                System.exit(0);
        }

        public void windowOpened(WindowEvent e) {}
        public void windowActivated(WindowEvent e) {}
        public void windowIconified(WindowEvent e) {}
        public void windowDeiconified(WindowEvent e) {}
        public void windowDeactivated(WindowEvent e) {}
        public void windowClosed(WindowEvent e) {}

}


```

More Examples: `Beeper` program example is available in this trail's introduction to events, [Introduction to Event Listeners](intro.html). You can find the entire program in 
[`Beeper.java`](../examples/events/BeeperProject/src/events/Beeper.java). The other example described in that section, 
[`MultiListener.java`](../examples/events/MultiListenerProject/src/events/MultiListener.java), has two action sources and two action listeners, with one listener listening to both sources and the other listening to just one.

## <a name="api" id="api">The Action Listener API</a>

<a name="actionlistener" id="actionlistener">The ActionListener Interface</a>

**Because `ActionListener` has only one method, it has no corresponding adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[actionPerformed(actionEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ActionListener.html#actionPerformed-java.awt.event.ActionEvent-)</td><td headers="h2">Called just after the user performs an action.</td>

<a name="actionevent" id="actionevent">The ActionEvent Class</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[String getActionCommand()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ActionEvent.html#getActionCommand--)</td><td headers="h102">Returns the string associated with this action. Most objects that can fire action events support a method called `setActionCommand` that lets you set this string.</td>
<td headers="h101">[int getModifiers()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ActionEvent.html#getModifiers--)</td><td headers="h102">Returns an integer representing the modifier keys the user was pressing when the action event occurred. You can use the `ActionEvent`-defined constants `SHIFT_MASK`, `CTRL_MASK`, `META_MASK`, and `ALT_MASK` to determine which keys were pressed. For example, if the user Shift-selects a menu item, then the following expression is nonzero:<pre>`actionEvent.getModifiers() &amp; ActionEvent.SHIFT_MASK`</pre></td>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)<br />(**in `java.util.EventObject`**)</td><td headers="h102">Returns the object that fired the event.</td>

## <a name="eg" id="eg">Examples that Use Action Listeners</a>

The following table lists some of the many examples that use action listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
