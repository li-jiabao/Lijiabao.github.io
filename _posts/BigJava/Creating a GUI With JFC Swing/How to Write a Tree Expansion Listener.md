
# How to Write a Tree Expansion Listener

Sometimes when using a 
[tree](../components/tree.html), you might need to react when a branch becomes expanded or collapsed. For example, you might need to load or save data.

Two kinds of listeners report expansion and collapse occurrences: **tree expansion** listeners and **tree-will-expand** listeners. This page discusses **tree expansion** listeners. See [How to Write a Tree-Will-Expand Listener](treewillexpandlistener.html) for a description of Tree-Will-Expand listeners.

A tree expansion listener detects when an expansion or collapse has already occured. In general, you should implement a tree expansion listener unless you need to prevent an expansion or collapse from ocurring .

&#160;

This example demonstrates a simple tree expansion listener. The text area at the bottom of the window displays a message every time a tree expansion event occurs. It's a straightforward, simple demo. To see a more interesting version that can veto expansions, see [How to Write a Tree-Will-Expand Listener](treewillexpandlistener.html).

<li>Click the Launch button to run TreeExpandEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#TreeExpandEventDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the TreeExpandEventDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TreeExpandEventDemoProject/TreeExpandEventDemo.jnlp)<br /></li>
1. Expand a node. A tree-expanded event is fired.
1. Collapse the node. A tree-collapsed event is fired.

The following code shows how the program handles expansion events. You can find the source code for this example in 
[`TreeExpandEventDemo.java`](../examples/events/TreeExpandEventDemoProject/src/events/TreeExpandEventDemo.java).

```

public class TreeExpandEventDemo ... {
    ...
    void saySomething(String eventDescription, TreeExpansionEvent e) {
        textArea.append(eventDescription + "; "
                        + "path = " + e.getPath()
                        + newline);
    }

    class DemoArea ... implements TreeExpansionListener {
        ...
        public DemoArea() {
            ...
            tree.addTreeExpansionListener(this);
            ...
        }
        ...
        // Required by TreeExpansionListener interface.
        public void treeExpanded(TreeExpansionEvent e) {
            saySomething("Tree-expanded event detected", e);
        }

        // Required by TreeExpansionListener interface.
        public void treeCollapsed(TreeExpansionEvent e) {
            saySomething("Tree-collapsed event detected", e);
        }
    }
}

```

## <a name="api" id="api">The Tree Expansion Listener API</a>

<a name="treeexpansionlistener" id="treeexpansionlistener">The TreeExpansionListener Interface</a>

**`TreeExpansionListener` has no adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[treeCollapsed(TreeExpansionEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeExpansionListener.html#treeCollapsed-javax.swing.event.TreeExpansionEvent-)</td><td headers="h2">Called just after a tree node collapses.</td>
<td headers="h1">[treeExpanded(TreeExpansionEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeExpansionListener.html#treeExpanded-javax.swing.event.TreeExpansionEvent-)</td><td headers="h2">Called just after a tree node expands.</td>

<a name="treeexpansionevent" id="treeexpansionevent">The TreeExpansionEvent API</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)</td><td headers="h102">Return the object that fired the event.</td>
<td headers="h101">[TreePath getPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeExpansionEvent.html#getPath--)</td><td headers="h102">Returns a [`TreePath`](https://docs.oracle.com/javase/8/docs/api/javax/swing/tree/TreePath.html) object that identifies each node from the root of the tree to the collapsed/expanded node, inclusive.</td>

## <a name="eg" id="eg">Examples that Use Tree Expansion Listeners</a>

The following table lists the examples that use tree expansion listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
