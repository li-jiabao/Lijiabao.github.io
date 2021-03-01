
# Using Top-Level Containers

As we mentioned before, Swing provides three generally useful top-level container classes: [`JFrame`](frame.html), [`JDialog`](dialog.html), and [`JApplet`](applet.html). When using these classes, you should keep these facts in mind:

- To appear onscreen, every GUI component must be part of a **containment hierarchy**. A containment hierarchy is a tree of components that has a top-level container as its root. We'll show you one in a bit.
- Each GUI component can be contained only once. If a component is already in a container and you try to add it to another container, the component will be removed from the first container and then added to the second.
- Each top-level container has a content pane that, generally speaking, contains (directly or indirectly) the visible components in that top-level container's GUI.
- You can optionally add a menu bar to a top-level container. The menu bar is by convention positioned within the top-level container, but outside the content pane. Some look and feels, such as the Mac OS look and feel, give you the option of placing the menu bar in another place more appropriate for the look and feel, such as at the top of the screen.

<a name="windownote" id="windownote"></a>

Here's a picture of a frame created by an application. The frame contains a green menu bar (with no menus) and, in the frame's content pane, a large blank, yellow label.
|<center><img src="../../figures/uiswing/components/TopLevelDemoMetal.png" width="208" height="234" align="bottom" alt="A simple application with a frame that contains a menu bar and a content pane." /></center>|<center><img src="../../figures/uiswing/components/ConceptualPane.gif" width="264" height="164" align="bottom" alt="A diagram of the frame's major parts" /></center>

You can find the entire source for this example in 
[`TopLevelDemo.java`](../examples/components/TopLevelDemoProject/src/components/TopLevelDemo.java). Although the example uses a `JFrame` in a standalone application, the same concepts apply to `JApplet`s and `JDialog`s.

Here's the containment hierarchy for this example's GUI:

As the ellipses imply, we left some details out of this diagram. We reveal the missing details a bit later. Here are the topics this section discusses:

- [Top-Level Containers and Containment Hierarchies](#general)
- [Adding Components to the Content Pane](#contentpane)
- [Adding a Menu Bar](#menubar)
- [The Root Pane (a.k.a. The Missing Details)](#rootpane)

<a name="general" id="general"></a>

## <a name="general__1" id="general__1">Top-Level Containers and Containment Hierarchies</a>

Each program that uses Swing components has at least one top-level container. This top-level container is the root of a containment hierarchy &#151; the hierarchy that contains all of the Swing components that appear inside the top-level container.

As a rule, a standalone application with a Swing-based GUI has at least one containment hierarchy with a `JFrame` as its root. For example, if an application has one main window and two dialogs, then the application has three containment hierarchies, and thus three top-level containers. One containment hierarchy has a `JFrame` as its root, and each of the other two has a `JDialog` object as its root.

A Swing-based applet has at least one containment hierarchy, exactly one of which is rooted by a `JApplet` object. For example, an applet that brings up a dialog has two containment hierarchies. The components in the browser window are in a containment hierarchy rooted by a `JApplet` object. The dialog has a containment hierarchy rooted by a `JDialog` object.

## <a name="contentpane" id="contentpane">Adding Components to the Content Pane</a>

Here's the code that the preceding example uses to get a frame's content pane and add the yellow label to it:

```

frame.getContentPane().add(yellowLabel, BorderLayout.CENTER);

```

As the code shows, you find the content pane of a top-level container by calling the `getContentPane` method. The default content pane is a simple intermediate container that inherits from `JComponent`, and that uses a `BorderLayout` as its layout manager.

It's easy to customize the content pane &#151; setting the layout manager or adding a border, for example. However, there is one tiny gotcha. The `getContentPane` method returns a `Container` object, not a `JComponent` object. This means that if you want to take advantage of the content pane's `JComponent` features, you need to either typecast the return value or create your own component to be the content pane. Our examples generally take the second approach, since it's a little cleaner. Another approach we sometimes take is to simply add a customized component to the content pane, covering the content pane completely.

<!-- deleted per shannon 3/15/07 
If you create your own content pane, 
make sure it's opaque.
An opaque `JPanel` object
makes a good content pane. -->
 Note that the default layout manager for `JPanel` is `FlowLayout`; you'll probably want to change it.

To make a component the content pane, use the top-level container's `setContentPane` method. For example:

```

//Create a panel and add components to it.
JPanel contentPane = new JPanel(new BorderLayout());
contentPane.setBorder(**someBorder**);
contentPane.add(**someComponent**, BorderLayout.CENTER);
contentPane.add(**anotherComponent**, BorderLayout.PAGE_END);

<!-- two lines deleted per shannon 
//Make it the content pane.
contentPane.setOpaque(true); -->
**topLevelContainer**.setContentPane(contentPane);

```

As a convenience, the `add` method and its variants, `remove` and `setLayout` have been overridden to forward to the `contentPane` as necessary. This means you can write

```

frame.add(child);

```

and the child will be added to the `contentPane.`<br />
<br />
Note that only these three methods do this. This means that `getLayout()` will not return the layout set with `setLayout()`.

## <a name="menubar" id="menubar">Adding a Menu Bar</a>

In theory, all top-level containers can hold a menu bar. In practice, however, menu bars usually appear only in frames and applets. To add a menu bar to a top-level container, create a `JMenuBar` object, populate it with menus, and then call `setJMenuBar`. The `TopLevelDemo` adds a menu bar to its frame with this code:

```

frame.setJMenuBar(greenMenuBar);

```

For more information about implementing menus and menu bars, see [How to Use Menus](menu.html).

## <a name="rootpane" id="rootpane">The Root Pane</a>

Each top-level container relies on a reclusive intermediate container called the **root pane**. The root pane manages the content pane and the menu bar, along with a couple of other containers. You generally don't need to know about root panes to use Swing components. However, if you ever need to intercept mouse clicks or paint over multiple components, you should get acquainted with root panes.

Here's a list of the components that a root pane provides to a frame (and to every other top-level container):

We've already told you about the content pane and the optional menu bar. The two other components that a root pane adds are a layered pane and a glass pane. The layered pane contains the menu bar and content pane, and enables Z-ordering of other components. The glass pane is often used to intercept input events occuring over the top-level container, and can also be used to paint over multiple components.

For more details, see [How to Use Root Panes](rootpane.html).

<!-- removed temporarily to insert Shannon's copy







     Don't use non-opaque containers such as
     `JScrollPane`, `JSplitPane`, 
     and `JTabbedPane`
     as content panes.
     A non-opaque content pane results in messy repaints.
     Although you can make any Swing component opaque 
     by invoking `setOpaque(true)` on it,
     some components don't look right 
     when they're completely opaque.
     For example, tabbed panes generally
     let part of the 
     underlying container show through,
     so that the tabs look non-rectangular.
     An opaque tabbed pane just tends to look bad.
     <p>
     In most look and feels, `JPanel`s are opaque
     by default.
     However, `JPanel`s in the GTK+ look and feel
     are not initially opaque.
     To be safe, we invoke `setOpaque` 
     on all `JPanel`s used as content panes.
     <hr />

<a name="menubar">
<h2>Adding a Menu Bar</h2>
</a>


All top-level containers can, in theory, have a menu bar.
In practice, however, menu bars usually appear only in frames
and perhaps in applets.
To add a menu bar to a top-level container,
you create a `JMenuBar` object,
populate it with menus,
and then call `setJMenuBar`.
The `TopLevelDemo` adds a menu bar
to its frame with this code:
<pre><code>
frame.setJMenuBar(greenMenuBar);
</code></pre>
For more information about implementing menus
and menu bars, see 
[How to Use Menus](menu.html).


<a name="rootpane">
<h2>The Root Pane</h2>
</a>

Each top-level container relies on a reclusive intermediate container
called the **root pane**.
The root pane manages the content pane and the menu bar,
along with a couple of other containers.
You generally don't need to know about root panes
to use Swing components.
However, if you ever need to intercept mouse clicks
or paint over multiple components,
you should get acquainted with root panes.

<p>

Here's a glimpse at the components that a root pane provides
to a frame (and to every other top-level container):

<center><img src="../../figures/uiswing/../ui/ui-rootPane.gif" width="367" height="166" align="bottom" alt="A root pane manages four other panes: a layered pane, a menu bar, a content pane, and a glass pane." /></center>We've already told you about the content pane and the optional menu bar.
The two other components that a root pane adds 
are a layered pane and a glass pane.
The layered pane directly contains the menu bar and content pane,
and enables Z-ordering of other components you might add.
The glass pane is often used to intercept input events
occuring over the top-level container,
and can also be used to paint over multiple components.

<p>

For more information about the intricacies of root panes, see 
[How to Use Root Panes](rootpane.html).

<p>
-->

## The Root Pane
