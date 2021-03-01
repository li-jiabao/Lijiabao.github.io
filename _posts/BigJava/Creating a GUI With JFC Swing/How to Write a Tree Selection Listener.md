
# How to Write a Tree Selection Listener

To detect when the user selects a node in a 
[tree](../components/tree.html), you need to register a tree selection listener. Here is an example, taken from the `TreeDemo` example discussed in 
[Responding to Node Selection](../components/tree.html#select), of detecting node selection in a tree that can have at most one node selected at a time: <!-- 
<p><font color=red>
[PENDING: Update this code snippet after example has been
modified to remove the inner class.]</font>
-->

```

tree.addTreeSelectionListener(new TreeSelectionListener() {
    public void valueChanged(TreeSelectionEvent e) {
        DefaultMutableTreeNode node = (DefaultMutableTreeNode)
                           tree.getLastSelectedPathComponent();

   ** /* if nothing is selected */ **
        if (node == null) return;

   ** /* retrieve the node that was selected */ **
        Object nodeInfo = node.getUserObject();
        ...
   ** /* React to the node selection. */**
        ...
    }
});

```

To specify that the tree should support single selection, the program uses this code:

```

tree.getSelectionModel().setSelectionMode
        (TreeSelectionModel.SINGLE_TREE_SELECTION);

```

The `TreeSelectionModel` interface defines three values for the selection mode:

## <a name="api" id="api">The Tree Selection Listener API</a>

<a name="treeselectionlistener" id="treeselectionlistener">The TreeSelectionListener Interface</a>

**Because `TreeSelectionListener` has only one method, it has no corresponding adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[valueChanged(TreeSelectionEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionListener.html#valueChanged-javax.swing.event.TreeSelectionEvent-)</td><td headers="h2">Called whenever the selection changes.</td>

<a name="treeselectionevent" id="treeselectionevent">The TreeSelectionEvent API</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)<br />(**in `java.util.EventObject`**)</td><td headers="h102">Return the object that fired the event.</td>
<td headers="h101">[TreePath getNewLeadSelectionPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#getNewLeadSelectionPath--)</td><td headers="h102">Return the current lead path.</td>
<td headers="h101">[TreePath getOldLeadSelectionPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#getOldLeadSelectionPath--)</td><td headers="h102">Return the path that was previously the lead path.</td>
<td headers="h101">[TreePath getPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#getPath--)</td><td headers="h102">Return the first path element.</td>
<td headers="h101">[TreePath[] getPaths()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#getPaths--)</td><td headers="h102">Return the paths that have been added or removed from the selection.</td>
<td headers="h101">[boolean isAddedPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#isAddedPath--)</td><td headers="h102">Return true if the first path element has been added to the selection. Returns false if the first path has been removed from the selection.</td>
<td headers="h101">[boolean isAddedPath(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#isAddedPath-int-)</td><td headers="h102">Return true if the path specified by the index was added to the selection.</td>
<td headers="h101">[boolean isAddedPath(TreePath)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#isAddedPath-javax.swing.tree.TreePath-)</td><td headers="h102">Return true if the specified path was added to the selection.</td>
<td headers="h101">[Object getLastSelectedPathComponent()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#getLastSelectedPathComponent--)</td><td headers="h102">Return the last path component in the first node of the current selection.</td>
<td headers="h101">[TreePath getLeadSelectionPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#getLeadSelectionPath--)<br />(**in `JTree`**)</td><td headers="h102">Return the current lead path.</td>

## <a name="eg" id="eg">Examples that Use Tree Selection Listeners</a>

The following table lists the examples that use tree selection listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
