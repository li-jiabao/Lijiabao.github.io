
# Solving Common Problems Using Other Swing Features

**Problem:** My application is not showing the look and feel I have requested via `UIManager.setLookAndFeel`.

You probably either set the look and feel to an invalid look and feel or set it after the UI manager loaded the default look and feel. If you are sure that the look and feel you specified is valid and setting the look and feel is the first thing your program does (at the top of its main method, for example), check whether you have a static field that references a Swing class. This reference can cause the default look and feel to be loaded if none has been specified. For more information, including how to set a look and feel after the GUI has been created, see the [look and feel](../lookandfeel/plaf.html) section.

**Problem:** Why is not my component getting the focus?

- Is it a custom component (for example, a direct subclass of `JComponent`) that you created? If so, you may need to give your component an input map and mouse listener. See [How to Make a Custom Component Focusable](focus.html#focusable) for more information and a demo.
- Is the component inside of a `JWindow` object? The focus system requires a `JWindow`'s owning frame to be visible for any components in the `JWindow` object to get the focus. By default, if you do not specify an owning frame for a `JWindow` object, an invisible owning frame is created for it. The solution is to either specify a visible and focusable owning frame when creating the `JWindow` object or to use `JDialog` or `JFrame` objects instead.

**Problem:** Why cannot my dialog receive the event generated when the user hits the Escape key?

If your dialog contains a text field, it may be consuming the event.

<li>If you want to get the Escape event regardless of whether a component consumes it, you should use a 
[`<code>KeyEventDispatcher`</code>](https://docs.oracle.com/javase/8/docs/api/java/awt/KeyEventDispatcher.html).</li>
<li>If you want to get the Escape event only if a component has not consumed it, then register a key binding on any `JComponent` component in the `JDialog` object, using the `WHEN_IN_FOCUSED_WINDOW` input map. For more information, see the 
[How to Use Key Bindings](../misc/keybinding.html) page.</li>

**Problem:** Why I cannot apply Swing components to a tray icon? Current implementation of the `TrayIcon` class supports the `PopupMenu` component, but not its Swing counterpart `JPopupMenu`. This limitation narrows capabilities to employ additional Swing features, for example, menu icons. See the Bug ID 
[6285881](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=6285881).

- A new `JTrayIcon` class will be created to eliminate this inconvenience. Until then, use AWT components to add a menu item, checkbox menu item, or submenu.

If you do not find your problem in this section, consult 
[Solving Common Component Problems](../components/problems.html).
