
# How to Write a Tree-Will-Expand Listener

The **tree-will-expand** listener prevents a 
[tree](../components/tree.html) node from expanding or collapsing. To be notified just **after** an expansion or collapse occurs, you should use a **tree expansion listener** instead.

&#160;

This demo adds a tree-will-expand listener to the `TreeExpandEventDemo` example discussed in [How to Write a Tree Expansion Listener](treeexpansionlistener.html). The code added here demonstrates that **tree-will-expand** listeners prevent node expansions and collapses: The listener will prompt you for confirmation each time you try to expand a node.

<li>Click the Launch button to run TreeExpandEventDemo2 using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#TreeExpandEventDemo2).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the TreeExpandEventDemo2 example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TreeExpandEventDemo2Project/TreeExpandEventDemo2.jnlp)<br /></li>
<li>Click the graphic to the left of the **Potrero Hill** node. This tells the tree that you want to expand the node.<br />
A dialog appears asking you whether you really want to expand the node.</li>
<li>Click "Expand" or dismiss the dialog.<br />
Messages in the text area tell you that both a tree-will-expand event and a tree-expanded event have occurred. At the end of each message is the path to the expanded node.</li>
<li>Try to expand another node, but this time press the "Cancel Expansion" button in the dialog.<br />
The node does not expand. Messages in the text area tell you that a tree-will-expand event occurred, and that you cancelled a tree expansion.</li>
<li>Collapse the **Potrero Hill** node.<br />
The node collapses without a dialog appearing, because the event handler's `treeWillCollapse` method lets the collapse occur, uncontested.</li>

The following snippet shows the code that this program adds to `TreeExpandEventDemo`. The bold line prevents the tree expansion from happening. You can find all the demo's source code in 
[`TreeExpandEventDemo2.java`](../examples/events/TreeExpandEventDemo2Project/src/events/TreeExpandEventDemo2.java).

```

public class TreeExpandEventDemo2 ... {
    ...
    class DemoArea ... implements ... TreeWillExpandListener {
        ...
        public DemoArea() {
            ...
            tree.addTreeWillExpandListener(this);
            ...
        }
        ...
        //Required by TreeWillExpandListener interface.
        public void treeWillExpand(TreeExpansionEvent e) 
                    throws ExpandVetoException {
            saySomething("Tree-will-expand event detected", e);
            **//...show a dialog...**
            if (**/* user said to cancel the expansion */**) {
                //Cancel expansion.
                saySomething("Tree expansion cancelled", e);
                **throw new ExpandVetoException(e);**
            }
        }

        //Required by TreeWillExpandListener interface.
        public void treeWillCollapse(TreeExpansionEvent e) {
            saySomething("Tree-will-collapse event detected", e);
        }
        ...
    }
}

```

## <a name="api" id="api">The Tree-Will-Expand Listener API</a>

<a name="treewillexpandlistener" id="treewillexpandlistener">The TreeWillExpandListener Interface</a>

**`TreeWillExpandListener` has no adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[treeWillCollapse(TreeExpansionEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeWillExpandListener.html#treeWillCollapse-javax.swing.event.TreeExpansionEvent-)</td><td headers="h2">Called just before a tree node collapses. To prevent the collapse from occurring, your implementation of this method should throw a [`ExpandVetoException`](https://docs.oracle.com/javase/8/docs/api/javax/swing/tree/ExpandVetoException.html) event.</td>
<td headers="h1">[treeWillExpand(TreeExpansionEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeWillExpandListener.html#treeWillExpand-javax.swing.event.TreeExpansionEvent-)</td><td headers="h2">Called just before a tree node expands. To prevent the expansion from occurring, your implementation of this method should throw a [`ExpandVetoException`](https://docs.oracle.com/javase/8/docs/api/javax/swing/tree/ExpandVetoException.html) event.</td>

See [The Tree Expansion Event API](treeexpansionlistener.html#api) for information about the 
[`TreeExpansionEvent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeExpansionEvent.html) argument for the preceding methods.

## <a name="eg" id="eg">Examples that Use Tree-Will-Expand Listeners</a>

[`TreeExpandEventDemo2`](../examples/events/index.html#TreeExpandEventDemo2), featured in this section, is our only example that uses a tree-will-expand listener.
