
# Lesson: Using Swing Components

This lesson gives you the background information you need to use the Swing components, and then describes every Swing component. It assumes that you have successfully compiled and run a program that uses Swing components, and that you are familiar with basic Swing concepts. These prerequisites are covered in 
[Getting Started with Swing](../start/index.html) and 
[Learning Swing with the NetBeans IDE](../learn/index.html).

## [A Visual Index to the Swing Components (Java Look and Feel)](../../ui/features/components.html)<br />
[A Visual Index to the Swing Components (Windows Look and Feel)](../../ui/features/compWin.html)

Before you get started, you may want to check out these pages (from the 
[Getting Started](../../ui/index.html) lesson in the Core trail) which have pictures of all the standard Swing components, from top-level containers to scroll panes to buttons. To find the section that discusses a particular component, just click the component&#39;s picture.

## [Using Top-Level Containers](toplevel.html)

Discusses how to use the features shared by the `JFrame`, `JDialog`, and `JApplet` classes &#151; content panes, menu bars, and root panes. It also discusses the **containment hierarchy**, which refers to the tree of components contained by a top-level container.

## [The JComponent Class](jcomponent.html)

Tells you about the features `JComponent` provides to its subclasses &#151; which include almost all Swing components &#151; and gives tips on how to take advantage of these features. This section ends with API tables describing the commonly used API defined by `JComponent` and its superclasses, `Container` and `Component`.

## [Using Text Components](text.html)

Describes the features and API shared by all components that descend from `JTextComponent`. You probably do not need to read this section if you are just using text fields (formatted or not) or text areas.

## [How to...](componentlist.html)

Sections on how to use each Swing component, in alphabetical order. We do not expect you to read these sections in order. Instead, we recommend reading the relevant "How to" sections once you are ready to start using Swing components in your own programs. For example, if your program needs a frame, a label, a button, and a color chooser, you should read [How to Make Frames](frame.html), [How to Use Labels](label.html), [How to Use Buttons](button.html), and [How to Use Color Choosers](colorchooser.html).

## [Using HTML in Swing Components](html.html)

Describes how to vary the font, color, or other formatting of text displayed by Swing components by using HTML tags.

## [Using Models](model.html)

Tells you about the Swing model architecture. This variation on Model-View-Controller (MVC) means that you can, if you wish, specify how the data and state of a Swing component are stored and retrieved. The benefits are the ability to share data and state between components, and to greatly improve the performance of components such as tables that display large amounts of data.

## [Using Borders](border.html)

Borders are very handy for drawing lines, titles, and empty space around the edges of components. (You might have noticed that the examples in this trail use a lot of borders.) This section tells you how to add a border to any `JComponent`.

## [Using Icons](icon.html)

Many Swing components can display icons. Usually, icons are implemented as instances of the `ImageIcon` class.

## [Solving Common Component Problems](problems.html)

This section discusses solutions to common component-related problems.

If you are interested in using JavaFX to create your GUI, see
[Using JavaFX Charts](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/charts.htm) and
[Using JavaFX UI Controls](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/ui_controls.htm).
