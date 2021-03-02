
# Solving Common Component Problems

This section discusses problems that you might encounter while using components. If you do not find your problem in this section, consult 
the following sections:

<li>
[Solving Common Problems Using Other Swing Features](../misc/problems.html)</li>
<li>
[Solving Common Layout Problems](../layout/problems.html)</li>
<li>
[Solving Common Event-Handling Problems](../events/problems.html)</li>
<li>
[Solving Common Painting Problems](../painting/problems.html)</li>

**Problem:** I am having trouble implementing a model (or some other code that is similar to something already in Java SE Platform, Standard Edition).

- Look at the Java SE source code. It is distributed with the JDK, and it is a great resource for finding code examples of implementing models, firing events, and the like.

**Problem:** Whenever the text in my text field updates, the text field's size changes.

- You should specify the preferred width of the text field by specifying the number of columns it should have room to display. To do this, you can use either an `int` argument to the `JTextField` constructor or the `setColumns` method.

**Problem:** Certain areas of the content pane look weird when they are repainted.

- If you set the content pane, make sure it is opaque. You can do this by invoking `setOpaque(true)` on your content pane. Note that although `JPanel`s are opaque in most look and feels, that is not true in the GTK+ look and feel. See [Adding Components to the Content Pane](toplevel.html#contentpane) for details.
<li>If one or more of your components performs custom painting, make sure you implemented it correctly. See 
[Solving Common Painting Problems](../painting/problems.html) for help.</li>
- You might have a thread safety problem. See the next entry.

**Problem:** My program is exhibiting weird symptoms that sometimes seem to be related to timing.

<li>Make sure your code is thread-safe. See 
[Concurrency in Swing](../concurrency/index.html) for details.</li>

**Problem:** My modal dialog gets lost behind other windows.

- If the dialog has a null parent component, try setting it to a valid frame or component when you create it.
<li>This bug was fixed in the 6.0 release. For more information, see 
[4255200](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=4255200).</li>

**Problem:** The scroll bar policies do not seem to be working as advertised.

- Some Swing releases contain bugs in the implementations for the `VERTICAL_SCROLLBAR_AS_NEEDED` and the `HORIZONTAL_SCROLLBAR_AS_NEEDED` policies. If feasible for your project, use the most recent release of Swing.
- If the scroll pane's client can change size dynamically, the program should set the client's preferred size and then call `revalidate` on the client.
- Make sure you specified the policy you intended for the orientation you intended.

**Problem:** My scroll pane has no scroll bars.

- If you want a scroll bar to appear all the time, specify either `VERTICAL_SCROLLBAR_ALWAYS` or `HORIZONTAL_SCROLLBAR_ALWAYS` for the scroll bar policy as appropriate.
- If you want the scroll bars to appear as needed, and you want to force the scroll bars to be needed when the scroll pane is created, you have two choices: either set the preferred size of scroll pane or its container, or implement a scroll-savvy class and return a value smaller than the component's standard preferred size from the `getPreferredScrollableViewportSize` method. Refer to [Sizing a Scroll Pane](scrollpane.html#sizing) for information.

**Problem:** The divider in my split pane does not move!

- You need to set the minimum size of at least one of the components in the split pane. Refer to [Positioning the Divider and Restricting Its Range](splitpane.html#divider) for information.

**Problem:** The `setDividerLocation` method of `JSplitPane` does not work.

- The `setDividerLocation(double)` method has no effect if the split pane has no size (typically true if it is not onscreen yet). You can either use `setDividerLocation(int)` or specify the preferred sizes of the split pane's contained components and the split pane's resize weight instead. Refer to [Positioning the Divider and Restricting Its Range](splitpane.html#divider) for information.

<a name="nestedborders" id="nestedborders">**Problem:** The borders on nested split panes look too wide.</a>

<li>If you nest split panes, the borders accumulate &#151; the border of the inner split panes display next to the border of the outer split pane causing borders that look extra wide. The problem is particularly noticeable when nesting many split panes. The workaround is to set the border to null on any split pane that is placed within another split pane. For information, see bug # 
[4131528](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=4131528) in the Java Bug Database.</li>

**Problem:** The buttons in my tool bar are too big.

<li>Try reducing the margin for the buttons. For example:
<pre><code>
button.setMargin(new Insets(0,0,0,0));
</code></pre>
</li>

<a name="layeredpane" id="layeredpane">**Problem:**</a> The components in my layered pane are not layered correctly. In fact, the layers seem to be inversed &#151; the lower the depth the higher the component.

<li>This can happen if you use an `int` instead of an `Integer` when adding components to a layered pane. To see what happens, in the `LayeredPaneDemo` class, change<br />
`layeredPane.add(label, new Integer(i));`<br />
to<br />
`layeredPane.add(label, **i**);`.</li>

**Problem:** The method call `**colorChooser**.setPreviewPanel(null)` does not remove the color chooser's preview panel as expected.

- A `null` argument specifies the default preview panel. To remove the preview panel, specify a standard panel with no size, like this: `**colorChooser**.setPreviewPanel(new JPanel());`
