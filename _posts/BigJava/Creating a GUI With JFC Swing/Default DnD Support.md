
# Default DnD Support

Technically speaking, the framework for drag and drop supports all Swing components &#8212; the data transfer mechanism is built into every `JComponent`. If you wanted, you could implement drop support for a `JSlider` so that it could fully participate in data transfer. While `JSlider` does not support drop by default, the components you would want (and expect) to support drag and drop do provide specialized built-in support.

The following components recognize the drag gesture once the `setDragEnabled(true)` method is invoked on the component. For example, once you invoke `myColorChooser.setDragEnabled(true)` you can drag colors from your color chooser:

- `JColorChooser`
- `JEditorPane`
- `JFileChooser`
- `JFormattedTextField`
- `JList`
- `JTable`
- `JTextArea`
- `JTextField`
- `JTextPane`
- `JTree`

The following components support drop out of the box. If you are using one of these components, your work is done.

- `JEditorPane`
- `JFormattedTextField`
- `JPasswordField`
- `JTextArea`
- `JTextField`
- `JTextPane`
- `JColorChooser`

The framework for drop is in place for the following components, but you need to plug in a small amount of code to customize the support for your needs.

- `JList`
- `JTable`
- `JTree`

For these critical components, Swing performs the drop location calculations and rendering; it allows you to specify a drop mode; and it handles component specific details, such as tree expansions. Your work is fairly minimal.

You can also install drop support on top-level containers, such as `JFrame` and `JDialog`. You can learn more about this in 
[Top-Level Drop](toplevel.html).
