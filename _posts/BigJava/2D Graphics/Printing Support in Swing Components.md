
# Printing Support in Swing Components

The 
[`PrintUIWindow.java`](examples/PrintUIWindow.java) example represented in the previous section demonstrates that the printout is exactly the same you saw on the screen. This appearance seems reasonable. However, if a window is scrollable, then the contents that are currently scrolled out of view are not included in the printout. This creates a dump effect on the printer. This becomes a particular problem when printing large components such as a Swing table or text components. Components may contain many lines of text which cannot all be entirely visible on the screen. In this case, print the contents displayed by the component in a manner consistent with the screen display.

To solve this problem, the Swing table and all text components are printing aware. The following methods directly provide the use of the Java 2D printing:

<li>
[`javax.swing.JTable.print();`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.html#print--)</li>
<li>
[`javax.swing.text.JTextComponent.print();`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#print--)</li>

These methods provide a full implementation of printing for their contents. An application doesn't need directly create a `PrinterJob` object and implement the `Printable` interface. The call of these methods displays a print dialog and prints the component's data in accordance with the user's selections. There are also additional methods which provide more options.
