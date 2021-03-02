
# Adding Cut, Copy and Paste (CCP)

So far our discussion has centered mostly around drag and drop support. However, it is an easy matter to hook up cut or copy or paste (ccp) to a transfer handler. This requires the following steps:

- Ensure a transfer handler is installed on the component.
- Create a manner by which the `TransferHandler`'s ccp support can be invoked. Typically this involves adding bindings to the input and action maps to have the `TransferHandler`'s ccp actions invoked in response to particular keystrokes.
<li>Create ccp menu items and/or buttons. (This step is optional but recommended.) This is easy to do with text components but requires a bit more work with other components, since you need logic to determine which component to fire the action on. See 
[CCP in a non-Text Component](listpaste.html) for more information.</li>
- Decide where you want to perform the paste. Perhaps above or below the current selection. Install the logic in the `importData` method.

Next we look at a cut and paste example that feature a text component.
