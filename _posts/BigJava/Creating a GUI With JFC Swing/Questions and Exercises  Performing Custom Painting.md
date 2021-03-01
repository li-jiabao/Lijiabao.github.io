
# Questions and Exercises: Performing Custom Painting

## Questions

1. What method defined by `JComponent` paints the inside of a component?

2. Which of the following code snippets paint a rectangle (filled or not) that is 100x100 pixels?

a. `g.fillRect(x, y, 100, 100)`<br />
b. `g.fillRect(x, y, 99, 99)`<br />
c. `g.drawRect(x, y, 100, 100)`<br />
d. b and c<br />
e. a and c

3. What code would you use to make a component perform the next painting operation using the background color at 50% transparency?

## Exercises

1. Using a standard border and custom component painting, implement a component that has a preferred size of 250x100, is opaque by default, has a 5-pixel black border, and paints an &#226;&#128;&#156;X&#226;&#128;&#157; (using 5-pixel-thick lines) in the foreground color, as shown in the following figure.

<img src="../../figures/uiswing/QandE/ComponentDisplayer-1.png" width="258" height="134" align="bottom" alt="ComponentDisplayer-1.png" />

2. Implement an icon that&#226;&#128;&#153;s 10x10 pixels and paints a solid rectangle that fills the 10x10 area. If the icon&#226;&#128;&#153;s component is enabled, the rectangle should be red; if disabled, gray. Make a copy of `ButtonDemo.java` that uses your custom `Icon` for the middle button, instead of displaying `middle.gif`. The following pictures show what the icon should look like.<br />
<td width="50%"><img src="../../figures/uiswing/QandE/SquareIcon-1.png" width="111" height="44" align="bottom" alt="SquareIcon-1.png" /></td><td width="50%"><img src="../../figures/uiswing/QandE/SquareIcon-2.png" width="111" height="44" align="bottom" alt="SquareIcon-2.png" /></td>

3. Implement a border that paints a red 15-pixel-tall stripe all the way across the top of its component. Test this border by substituting it for the border on the component you created in exercise 1. The result should look like the following figure. <img src="../../figures/uiswing/QandE/ComponentDisplayer-2.png" width="258" height="134" align="bottom" alt="ComponentDisplayer-2.png" />
[Check your answers.](answers-ch6.html)
