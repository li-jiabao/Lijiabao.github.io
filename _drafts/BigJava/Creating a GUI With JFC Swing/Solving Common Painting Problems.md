
# Solving Common Painting Problems

**Problem:** I don't know where to put my painting code.

- Painting code belongs in the `paintComponent` method of any component descended from `JComponent`.

**Problem:** The stuff I paint doesn't show up.

<li>Check whether your component is showing up at all. 
[Solving Common Component Problems](../components/problems.html) should help you with this.</li>
- Check whether `repaint` is invoked on your component whenever its appearance needs to be updated.

**Problem:** My component's foreground shows up, but its background is invisible. The result is that one or more components directly behind my component are unexpectedly visible.

- Make sure your component is opaque. `JPanel`s, for example, are opaque by default in many but not all look and feels. To make components such as `JLabel`s and GTK+ `JPanel`s opaque, you must invoke `setOpaque(true)` on them.
- If your custom component extends `JPanel` or a more specialized `JComponent` descendant, then you can paint the background by invoking `super.paintComponent` before painting the contents of your component.
<li>You can paint the background yourself using this code at the top of a custom component's `paintComponent` method:
<pre><code>
g.setColor(getBackground());
g.fillRect(0, 0, getWidth(), getHeight());
g.setColor(getForeground());
</code></pre>
</li>

**Problem:** I used `setBackground` to set my component's background color, but it seemed to have no effect.

- Most likely, your component isn't painting its background, either because it's not opaque or your custom painting code doesn't paint the background. If you set the background color for a `JLabel`, for example, you must also invoke `setOpaque(true)` on the label to make the label's background be painted.

**Problem:** I'm using the exact same code as a tutorial example, but it doesn't work. Why?

- Is the code executed in the exact same method as the tutorial example? For example, if the tutorial example has the code in the example's `paintComponent` method, then this method might be the only place where the code is guaranteed to work.

**Problem:** How do I paint thick lines? patterns?

<li>The Java&#8482; 2D API provides extensive support for implementing line widths and styles, as well as patterns for use in filling and stroking shapes. See the 
[2D Graphics](../../2d/index.html) trail for more information on using the Java 2D API.</li>

**Problem:** The edges of a particular component look odd.

- Because components often update their borders to reflect component state, you generally should avoid invoking `setBorder` except on `JPanel`s and custom subclasses of `JComponent`.
- Is the component painted by a look and feel such as GTK+ or Windows XP that uses UI-painted borders instead of `Border` objects? If so, don't invoke `setBorder` on the component.
- Does the component have custom painting code? If so, does the painting code take the component's insets into account?

**Problem:** Visual artifacts appear in my GUI.

- If you set the background color of a component, be sure the color has no transparency if the component is supposed to be opaque.
- Use the `setOpaque` method to set component opacity if necessary. For example, the content pane must be opaque, but components with transparent backgrounds must not be opaque.
- Make sure your custom component fills its painting area completely if it's opaque.

**Problem:** The performance of my custom painting code is poor.

- If you can paint part of your component, use the `getClip` or `getClipBounds` method of `Graphics` to determine which area you need to paint. The less you paint, the faster it will be.
- If only part of your component needs to be updated, make paint requests using a version of `repaint` that specifies the painting region.
<li>For help on choosing efficient painting techniques, look for the string "performance" in the 
[Java Media APIs home page](http://www.oracle.com/technetwork/java/javase/tech/media-141984.html).</li>

**Problem:** The same transforms applied to seemingly identical `Graphics` objects sometimes have slightly different effects.

- Because the Swing painting code sets the transform (using the `Graphics` method `translate`) before invoking `paintComponent`, any transforms that you apply have a cumulative effect. This doesn't matter when doing a simple translation, but a more complex `AffineTransform`, for example, might have unexpected results.

If you don't see your problem in this list, see 
[Solving Common Component Problems](../components/problems.html) and 
[Solving Common Layout Problems](../layout/problems.html).
