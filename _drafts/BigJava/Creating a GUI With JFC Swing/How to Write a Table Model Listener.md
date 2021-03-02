
# How to Write a Table Model Listener

Upon instantiation, each 
[`JTable`](../components/table.html) object is passed a table model object that manages the data it displays. By default, a `JTable` object inherits a `DefaultTable` object if no custom `TableModel` object is specified, but by default, this model only manages strings. To handle objects, perform calculations, or to retrieve data from databases or other programs, you must design your own custom `TableModel` object, which implements the `TableModel` interface. See [Creating a Table Model](../components/table.html#data) for details.

To detect changes to the data managed by a table model object, the `JTable` class needs to implement the `TableModelListener` interface, call `addTableModelListener()` to catch events, and then override `tableChanged()` to respond to listener events. See [Listening for Data Changes](../components/table.html#modelchange) for details.

The `JTable` object itself 
automatically uses a table model listener 
to make its GUI reflect the current state of the table model.

<p>You register a table model listener using the `TableModel`
`addTableModelListener` method. -->
<h2><a name="api" id="api">The Table Model Listener API</a></h2>
<p style="text-align: center"><a name="tablemodellistener" id="tablemodellistener">The TableModelListener Interface</a>

<a name="tablemodellistener" id="tablemodellistener">The TableModelListener Interface</a>

**Because `TableModelListener` has only one method, it has no corresponding adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[tableChanged(TableModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelListener.html#tableChanged-javax.swing.event.TableModelEvent-)</td><td headers="h2">Called when the structure of or data in the table has changed.</td>

<a name="tablemodelevent" id="tablemodelevent">The TableModelEvent API</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)<br />(**in `java.util.EventObject`**)</td><td headers="h102">Return the object that fired the event.</td>
<td headers="h101">[int getFirstRow()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelEvent.html#getFirstRow--)</td><td headers="h102">Return the index of the first row that changed. `TableModelEvent.HEADER_ROW` specifies the table header.</td>
<td headers="h101">[int getLastRow()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelEvent.html#getLastRow--)</td><td headers="h102">The last row that changed. Again, `HEADER_ROW` is a possible value.</td>
<td headers="h101">[int getColumn()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelEvent.html#getColumn--)</td><td headers="h102">Return the index of the column that changed. The constant `TableModelEvent.ALL_COLUMNS` specifies that all the columns might have changed.</td>
<td headers="h101">[int getType()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelEvent.html#getType--)</td><td headers="h102">What happened to the changed cells. The returned value is one of the following: `TableModelEvent.INSERT`, `TableModelEvent.DELETE`, or `TableModelEvent.UPDATE`.</td>
