
# Constructing Complex Shapes from Geometry Primitives

Constructive area geometry (CAG) is the process of creating new geometric shapes by performing boolean operations on existing ones. In the Java 2D API the 
[`Area`](https://docs.oracle.com/javase/8/docs/api/java/awt/geom/Area.html) class implements the 
[`Shape`](https://docs.oracle.com/javase/8/docs/api/java/awt/Shape.html) interface and supports the following boolean operations.
|<img src="../../figures/2d/2D-47.gif" width="131" height="86" alt="Venn diagram showing Union" />|Union|<img src="../../figures/2d/2D-48.gif" width="131" height="86" alt="Venn diagram showing Subtraction" />|Subtraction
|<img src="../../figures/2d/2D-49.gif" width="135" height="89" alt="Venn diagram showing Intersection" />|Intersection|<img src="../../figures/2d/2D-50.gif" width="134" height="88" alt="Venn diagram showing Exclusive-or operation" />|Exclusive-or (`XOR`)

## Example: Areas

In this example `Area` objects construct a pear shape from several ellipses.


<applet code="Pear" archive="examples/lib/PearApplet.jar" width="150" height="200" alt="applet constructs pear shape"><param name="permissions" value="sandbox" /></applet>


[`<code>Pear.java`</code>](examples/Pear.java) contains the complete code for this applet.

The leaves are each created by performing an intersection on two overlapping circles.

```

leaf = new Ellipse2D.Double();
...
leaf1 = new Area(leaf);
leaf2 = new Area(leaf);
...
leaf.setFrame(ew-16, eh-29, 15.0, 15.0);
leaf1 = new Area(leaf);
leaf.setFrame(ew-14, eh-47, 30.0, 30.0);
leaf2 = new Area(leaf);
leaf1.intersect(leaf2);
g2.fill(leaf1);
...
leaf.setFrame(ew+1, eh-29, 15.0, 15.0);
leaf1 = new Area(leaf);
leaf2.intersect(leaf1);
g2.fill(leaf2);

```

Overlapping circles are also used to construct the stem through a subtraction operation.

```

stem = new Ellipse2D.Double();
...
stem.setFrame(ew, eh-42, 40.0, 40.0);
st1 = new Area(stem);
stem.setFrame(ew+3, eh-47, 50.0, 50.0);
st2 = new Area(stem);
st1.subtract(st2);
g2.fill(st1);

```

The body of the pear is constructed by performing a union operation on a circle and an oval.

```

circle = new Ellipse2D.Double();
oval = new Ellipse2D.Double();
circ = new Area(circle);
ov = new Area(oval);
...
circle.setFrame(ew-25, eh, 50.0, 50.0);
oval.setFrame(ew-19, eh-20, 40.0, 70.0);
circ = new Area(circle);
ov = new Area(oval);
circ.add(ov);
g2.fill(circ);

```
