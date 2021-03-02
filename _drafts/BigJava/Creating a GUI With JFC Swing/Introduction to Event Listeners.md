
# Introduction to Event Listeners

If you have read any of the component how-to pages, you probably already know the basics of event listeners.

Let us look at one of the simplest event handling examples possible. It is called Beeper, and it features a button that beeps when you click it.

Click the Launch button to run Beeper using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#Beeper).

You can find the entire program in 
[`Beeper.java`](../examples/events/BeeperProject/src/events/Beeper.java). Here is the code that implements the event handling for the button:

```

public class Beeper ... implements ActionListener {
    ...
    **//where initialization occurs:**
        button.addActionListener(this);
    ...
    public void actionPerformed(ActionEvent e) {
        **...//Make a beep sound...**
    }
}

```

The `Beeper` class implements the [`ActionListener`](actionlistener.html) interface, which contains one method: `actionPerformed`. Since `Beeper` implements `ActionListener`, a `Beeper` object can register as a listener for the action events that buttons fire. Once the `Beeper` has been registered using the `Button` `addActionListener` method, the `Beeper`'s `actionPerformed` method is called every time the button is clicked.

## <a name="example2" id="example2">A More Complex Example</a>

The event model, which you saw at its simplest in the preceding example, is quite powerful and flexible. Any number of event listener objects can listen for all kinds of events from any number of event source objects. For example, a program might create one listener per event source. Or a program might have a single listener for all events from all sources. A program can even have more than one listener for a single kind of event from a single event source.

Multiple listeners can register to be notified of events of a particular type from a particular source. Also, the same listener can listen to notifications from different objects.

Each event is represented by an object that gives information about the event and identifies the event source. Event sources are often components or models, but other kinds of objects can also be event sources.

Whenever you want to detect events from a particular component, first check the how-to section for that component. A list of the component how-to sections is 
[here](../components/componentlist.html). The how-to sections give examples of handling the events that you are most likely to care about. In 
[How to Use Color Choosers](../components/colorchooser.html), for instance, you will find an example of writing a change listener to track when the color changes in the color chooser.

The following example demonstrates that event listeners can be registered on multiple objects and that the same event can be sent to multiple listeners. The example contains two event sources (`JButton` instances) and two event listeners. One of the event listeners (an instance of a class called `MultiListener`) listens for events from both buttons. When it receives an event, it adds the event's "action command" (which is set to the text on the button's label) to the top text area. The second event listener (an instance of a class called `Eavesdropper`) listens for events on only one of the buttons. When it receives an event, it adds the action command to the bottom text area.

<li>Click the Launch button to run MultiListener using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#MultiListener).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the MultiListener example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/MultiListenerProject/MultiListener.jnlp)<br /></li>
1. Click the **Blah blah blah** button. Only the `MultiListener` object is registered to listen to this button.
1. Click the **You do not say!** button. Both the `MultiListener` object and the `Eavesdropper` object are registered to listen to this button.

You can find the entire program in 
[`MultiListener.java`](../examples/events/MultiListenerProject/src/events/MultiListener.java). Here is the code that implements the event handling for the button:

```

public class MultiListener ... implements ActionListener {
    ...
    **//where initialization occurs:**
        button1.addActionListener(this);
        button2.addActionListener(this);

        button2.addActionListener(new Eavesdropper(bottomTextArea));
    }

    public void actionPerformed(ActionEvent e) {
        topTextArea.append(e.getActionCommand() + newline);
    }
}

class Eavesdropper implements ActionListener {
    ...
    public void actionPerformed(ActionEvent e) {
        myTextArea.append(e.getActionCommand() + newline);
    }
}

```

In the above code, both `MultiListener` and `Eavesdropper` implement the `ActionListener` interface and register as action listeners using the `JButton` `addActionListener` method. Both classes' implementations of the `actionPerformed` method are similar: they simply add the event's action command to a text area.
