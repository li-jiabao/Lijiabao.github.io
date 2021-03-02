
# Supporting User Interaction

To enable the user to interact with the graphics you display, you need to be able to determine when the user clicks on one of them. The `hit` method of the 
[`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html) class provides a way to easily determine whether a mouse click occurred over a particular 
[`Shape`](https://docs.oracle.com/javase/8/docs/api/java/awt/Shape.html) object. Alternatively you can get the location of the mouse click and call `contains` on the `Shape` to determine whether the click was within the bounds of the `Shape`.

If you are using primitive text, you can perform simple hit testing by getting the outline `Shape` that corresponds to the text and then calling `hit` or `contains` with that `Shape`. Supporting text editing requires much more sophisticated hit testing. If you want to allow the user to edit text, you should generally use one of the Swing editable text components. If you are working with primitive text and are using the 
[`TextLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/TextLayout.html) class to manage the shaping and positioning of the text, you can also use `TextLayout` to perform hit testing for text editing. For more information see the chapter Text and Fonts in the 
[Java 2D Programmer's Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/2d/spec/j2d-bookTOC.html) or see the HitTestSample example below, which uses a `TextLayout` to perform simple hit-testing.

## Example: ShapeMover

This applet allows the user to drag a `Shape` around within the applet window. The `Shape` is redrawn at every mouse location to provide feedback as the user drags it.

<applet code="ShapeMover" archive="examples/lib/ShapeMoverApplet.jar" width="550" height="250" alt="ShapeMover applet"><param name="permissions" value="sandbox" /></applet>


[`ShapeMover.java`](examples/ShapeMover.java) contains the complete code for this applet.

The `contains` method is called to determine whether the cursor is within the bounds of the rectangle when the mouse is pressed. If it is, the location of the rectangle is updated.

```

public void mousePressed(MouseEvent e){
    last_x = rect.x - e.getX();
    last_y = rect.y - e.getY();
    if(rect.contains(e.getX(),
        e.getY())) updateLocation(e);
    ...

public void updateLocation(MouseEvent e){
    rect.setLocation(last_x + e.getX(),
        last_y + e.getY());
    ...
    repaint();

```

You might notice that redrawing the `Shape` at every mouse location is slow, because the filled rectangle is rerendered every time it is moved. Using double buffering can eliminate this problem. If you use Swing, the drawing will be double buffered automatically; you don't have to change the rendering code at all. The code for a Swing version of this program is 
[`SwingShapeMover.java`](examples/SwingShapeMover.java).

## Example: HitTestSample

This application illustrates hit testing by drawing the default caret wherever the user clicks on the `TextLayout`, as shown in the following figure.

<applet code="HitTestSample" archive="examples/lib/HitTestSampleApplet.jar" width="400" height="250" alt="HitTestSample applet"><param name="permissions" value="sandbox" /></applet>


[`<code>HitTestSample.java`</code>](examples/HitTestSample.java) contains the complete code for this applet.

The `mouseClicked` method uses `TextLayout.hitTestChar` to return a `java.awt.font.TextHitInfo` object that contains the mouse click location (the insertion index) in the `TextLayout` object.

Information returned by the `TextLayout` `getAscent`, `getDescent`, and `getAdvance` methods is used to compute the location of the origin for the `TextLayout` object so it is horizontally and vertically centered.

```

...

private Point2D computeLayoutOrigin() {
  Dimension size = getPreferredSize();
  Point2D.Float origin = new Point2D.Float();
     
  origin.x = (float) (size.width -
             textLayout.getAdvance()) / 2;   
  origin.y = 
    (float) (size.height -
             textLayout.getDescent() +
             textLayout.getAscent())/2;
  return origin;
}

...

public void paintComponent(Graphics g) {
  super.paintComponent(g);
  setBackground(Color.white);
  Graphics2D graphics2D = (Graphics2D) g;                
  Point2D origin = computeLayoutOrigin();
  graphics2D.translate(origin.getX(),
                       origin.getY());
                
  // Draw textLayout.
  textLayout.draw(graphics2D, 0, 0);
     
  // Retrieve caret Shapes for insertionIndex.
  Shape[] carets =
      textLayout.getCaretShapes(insertionIndex);
       
  // Draw the carets.  carets[0] is the strong
  // caret and carets[1] is the weak caret.   
  graphics2D.setColor(STRONG_CARET_COLOR);
  graphics2D.draw(carets[0]);                
  if (carets[1] != null) {
    graphics2D.setColor(WEAK_CARET_COLOR);
    graphics2D.draw(carets[1]);
  }       
}

...

private class HitTestMouseListener
              extends MouseAdapter {
                
    /**
     * Compute the character position of the
     * mouse click.
     */     
    public void mouseClicked(MouseEvent e) {
                
      Point2D origin = computeLayoutOrigin();
                
      // Compute the mouse click location
      // relative to textLayout's origin.
      float clickX =
          (float) (e.getX() - origin.getX());
      float clickY =
          (float) (e.getY() - origin.getY());
         
      // Get the character position of the
      // mouse click.
      TextHitInfo currentHit =
          textLayout.hitTestChar(clickX, clickY);
      insertionIndex =
          currentHit.getInsertionIndex();
            
      // Repaint the Component so the new
      // caret(s) will be displayed.
      hitPane.repaint();
    }

```
