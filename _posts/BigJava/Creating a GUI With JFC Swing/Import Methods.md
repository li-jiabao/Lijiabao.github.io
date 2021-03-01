
# Import Methods

Now we will look at the methods used for importing data into a component. These methods are invoked for the drop gesture, or the paste action, when the component is the target of the operation. The `TransferHandler` methods for importing data are:

<li>
<p>
[`canImport(TransferHandler.TransferSupport)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#canImport-javax.swing.TransferHandler.TransferSupport-) &#8212; This method is called repeatedly during a drag gesture and returns true if the area below the cursor can accept the transfer, or false if the transfer will be rejected. For example, if a user drags a color over a component that accepts only text, the `canImport` method for that component's `TransferHandler` should return false.</p>
</li>
<li>
<p>
[`importData(TransferHandler.TransferSupport)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#importData-javax.swing.TransferHandler.TransferSupport-) &#8212; This method is called on a successful drop (or paste) and initiates the transfer of data to the target component. This method returns true if the import was successful and false otherwise.</p>
</li>

These methods replace older versions that do not use the `TransferSupport` class. Unlike its replacement method, `canImport(JComponent, DataFlavor[])` is not called continuously.

You will notice that these import methods take a `TransferHandler.TransferSupport` argument. Next we look at the `TransferSupport` class and then some sample import methods.
