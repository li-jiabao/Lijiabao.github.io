
# Showing the Drop Location

Generally during a drag operation, a component gives visual feedback when it can accept the data. It might highlight the drop location, or it might show a caret or a horizontal line where the insertion would occur. Swing renders the drop location when the `canImport` method for the component's `TransferHandler` returns true.

To control this programmatically, you can use the 
[`setShowDropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#setShowDropLocation-boolean-) method. Calling this method with `true` causes the visual feedback for the drop location to always be displayed, even if the drop will not be accepted. Calling this method with `false` prevents any visual feedback, even if the drop will be accepted. You always invoke this method from `canImport`.

The 
[Demo - LocationSensitiveDemo](locsensitivedemo.html) page includes a combo box that enables you to choose to always show the drop location, or never show the drop location, or the default behavior. But first we will talk about location sensitive drop.
