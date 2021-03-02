
# How to Write a Tree Model Listener

By implementing a tree model listener, you can detect when the data displayed by a 
[tree](../components/tree.html) changes. You can use a tree model listener to detect when the user edits tree nodes. All notifications describe changes relative to a node in the tree. For details, read 
[Dynamically Changing a Tree](../components/tree.html#dynamic).

## <a name="api" id="api">The Tree Model Listener API</a>

<a name="treemodellistener" id="treemodellistener">The TreeModelListener Interface</a>

**`TreeModelListener` has no adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[treeNodesChanged(TreeModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelListener.html#treeNodesChanged-javax.swing.event.TreeModelEvent-)</td><td headers="h2">Called when one or more sibling nodes have changed in some way.</td>
<td headers="h1">[treeNodesInserted(TreeModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelListener.html#treeNodesInserted-javax.swing.event.TreeModelEvent-)</td><td headers="h2">Called after nodes have been inserted into the tree.</td>
<td headers="h1">[treeNodesRemoved(TreeModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelListener.html#treeNodesRemoved-javax.swing.event.TreeModelEvent-)</td><td headers="h2">Called after nodes have been removed from the tree.</td>
<td headers="h1">[treeStructureChanged(TreeModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelListener.html#treeStructureChanged-javax.swing.event.TreeModelEvent-)</td><td headers="h2">Called after the tree's structure has drastically changed from the current node on down. This event applies to all nodes connected to this node.</td>

<a name="treemodelevent" id="treemodelevent">The TreeModelEvent API</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)<br />(**in `java.util.EventObject`**)</td><td headers="h102">Return the object that fired the event.</td>
<td headers="h101">[int[] getChildIndices()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelEvent.html#getChildIndices--)</td><td headers="h102">For `treeNodesChanged`, `treeNodesInserted`, and `treeNodesRemoved`, returns the indices of the changed, inserted, or deleted nodes, respectively. Returns nothing useful for `treeStructureChanged`.</td>
<td headers="h101">[Object[] getChildren()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelEvent.html#getChildren--)</td><td headers="h102">Returns the objects corresponding to the child indices.</td>
<td headers="h101">[Object[] getPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelEvent.html#getPath--)</td><td headers="h102">Returns the path to the parent of the changed, inserted, or deleted nodes. For `treeStructureChanged`, returns the path to the node beneath which the structure has changed.</td>
<td headers="h101">[TreePath getTreePath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelEvent.html#getTreePath--)</td><td headers="h102">Returns the same thing as `getPath`, but as a [`TreePath`](https://docs.oracle.com/javase/8/docs/api/javax/swing/tree/TreePath.html) object.</td>

## <a name="eg" id="eg">Examples that Use Tree Model Listeners</a>

The following table lists the examples that use tree expansion listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
