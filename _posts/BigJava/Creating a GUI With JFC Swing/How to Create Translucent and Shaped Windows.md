
# How to Create Translucent and Shaped Windows

As of the Java Platform, Standard Edition 6 (Java SE 6) Update 10 release, you can add translucent and shaped windows to your Swing applications. This page covers the following topics:

- [Supported Capabilities](#supported)
- [Determining a Platform's Capabilities](#capability)
- [How to Implement Uniform Translucency](#uniform)
- [How to Implement Per-Pixel Translucency](#per-pixel)
- [How to Implement a Shaped Window](#shaped)
- [Java SE Release 6 Update 10 API](#6u10)

## <a name="supported" id="supported">Supported Capabilities</a>

This functionality, which is part of the public AWT package in the JDK 7 release, takes three forms, as follows:

<li>You can create a window with **uniform** translucency, where each pixel has the same translucency (or alpha) value. The following screen capture shows a window with 45 percent translucency.
<center><img src="../../figures/uiswing/misc/TranslucentWindow.png" width="331" height="225" align="bottom" alt="A translucent window" /></center>
<!-- ************************ -->

<hr />**Try this:**&#160;<p>Click the Launch button to run the TranslucentWindowDemo example using
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html). This example requires [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or later. Alternatively, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#TranslucentWindowDemo).</p>
[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the TranslucentWindowDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TranslucentWindowDemoProject/TranslucentWindowDemo.jnlp)
<hr />

<!-- ************************ -->


</li>
<li>You can create a window with **per-pixel** translucency, where each pixel has its own alpha value. With this feature you can, for example, create a window that fades away to nothing by defining a gradient in the alpha values. The following screen capture shows a window with gradient translucency from the top (fully translucent) to the bottom (fully opaque).
<center><img src="../../figures/uiswing/misc/GradientWindow.png" width="333" height="222" align="bottom" alt="A window with per-pixel translucency." /></center>
<!-- ************************ -->

<hr />**Try this:**&#160;<p>Click the Launch button to run the GradientTranslucentWindowDemo example using
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html). This example requires [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or later. Alternatively, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#GradientTranslucentWindowDemo).</p>
[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the GradientTranslucentWindowDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/GradientTranslucentWindowDemoProject/GradientTranslucentWindowDemo.jnlp)
<hr />

<!-- ************************ -->


</li>


<li>You can create a window with any `Shape` object that you can define. Shaped windows can be opaque, or they can use uniform, or per-pixel, translucency. The following screen capture shows an oval-shaped window with 30 percent translucency.
<center><img src="../../figures/uiswing/misc/ShapedWindow.png" width="339" height="229" align="bottom" alt="A oval-shaped window." /></center>
<!-- ************************ -->

<hr />**Try this:**&#160;<p>Click the Launch button to run the ShapedWindowDemo example using
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html). This example requires [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or later. Alternatively, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#ShapedWindowDemo).</p>
[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ShapedWindowDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ShapedWindowDemoProject/ShapedWindowDemo.jnlp)
<hr />

<!-- ************************ -->


</li>


## <a name="capability" id="capability">Determining a Platform's Capabilities</a>

- `TRANSLUCENT` &#8211; The underlying platform supports windows with uniform translucency, where each pixel has the same alpha value.
- `PERPIXEL_TRANSLUCENT` &#8211; The underlying platform supports windows with per-pixel translucency. This capability is required to implement windows that fade away.
- `PERPIXEL_TRANSPARENT` &#8211; The underlying platform supports shaped windows.

```

import static java.awt.GraphicsDevice.WindowTranslucency.*;

// Determine what the default GraphicsDevice can support.
GraphicsEnvironment ge =
    GraphicsEnvironment.getLocalGraphicsEnvironment();
GraphicsDevice gd = ge.getDefaultScreenDevice();

boolean isUniformTranslucencySupported =
    gd.isWindowTranslucencySupported(TRANSLUCENT);
boolean isPerPixelTranslucencySupported =
    gd.isWindowTranslucencySupported(PERPIXEL_TRANSLUCENT);
boolean isShapedWindowSupported =
    gd.isWindowTranslucencySupported(PERPIXEL_TRANSPARENT);

```

## <a name="uniform" id="uniform">How to Implement Uniform Translucency</a>

The
[`<code>TranslucentWindowDemo.java`</code>](../examples/misc/TranslucentWindowDemoProject/src/misc/TranslucentWindowDemo.java) example creates a window that is 55 percent opaque (45 percent translucent). If the underlying platform does not support translucent windows, the example exits. The code relating to opacity is shown in bold.

```

import java.awt.*;
import javax.swing.*;
import static java.awt.GraphicsDevice.WindowTranslucency.*;

public class TranslucentWindowDemo extends JFrame {
    public TranslucentWindowDemo() {
        super("TranslucentWindow");
        setLayout(new GridBagLayout());

        setSize(300,200);
        setLocationRelativeTo(null);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        //Add a sample button.
        add(new JButton("I am a Button"));
    }

    public static void main(String[] args) {
        // Determine if the GraphicsDevice supports translucency.
        <b>GraphicsEnvironment ge = 
            GraphicsEnvironment.getLocalGraphicsEnvironment();
        GraphicsDevice gd = ge.getDefaultScreenDevice();</b>

        //If translucent windows aren't supported, exit.
        <b>if (!gd.isWindowTranslucencySupported(TRANSLUCENT)) {
            System.err.println(
                "Translucency is not supported");
                System.exit(0);
        }</b>
        
        JFrame.setDefaultLookAndFeelDecorated(true);

        // Create the GUI on the event-dispatching thread
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                TranslucentWindowDemo tw = new TranslucentWindowDemo();

                // Set the window to 55% opaque (45% translucent).
                **tw.setOpacity(0.55f);**

                // Display the window.
                tw.setVisible(true);
            }
        });
    }
}

```

Note that the button is also affected by the uniform translucency. Setting the opacity affects the whole window, including any components that the window contains.

## <a name="per-pixel" id="per-pixel">How to Implement Per-Pixel Translucency</a>

Invoking 
[`setBackground(new Color(0,0,0,0))`](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html#setBackground-java.awt.Color-) on the window causes the software to use the alpha values to render per-pixel translucency. In fact, invoking `setBackground(new Color(0,0,0,alpha)`, where `alpha` is less than 255, installs per-pixel transparency. So, if you invoke `setBackground(new Color(0,0,0,128))` and do nothing else, the window is rendered with 50 percent translucency for each background pixel. However, if you are creating your own range of alpha values, you most likely will want an alpha value of 0.

While not prohibited by the public API, you will generally want to enable per-pixel translucency on undecorated windows. In most cases, using per-pixel translucency on decorated windows does not make sense. Doing so can disable the decorations, or cause other platform-dependent side effects.

To determine if a window is using per-pixel translucency, you can use the 
[`isOpaque`](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html#isOpaque--) method.

An example follows. First, here are the steps required to implement the example:

1. Invoke `setBackground(new Color(0,0,0,0))` on the window.
1. Create a `JPanel` instance that overrides the `paintComponent` method.
1. In the `paintComponent` method, create a `GradientPaint` instance.
1. In the example, the top of the rectangle has an alpha value of 0 (the most transparent) and the bottom has an alpha value of 255 (the most opaque). The `GradientPaint` class smoothly interpolates the alpha values from the top to the bottom of the rectangle.
1. Set the `GradientPaint` instance as the panel's paint method.

Here is the code for the
[`<code>GradientTranslucentWindowDemo.java`</code>](../examples/misc/GradientTranslucentWindowDemoProject/src/misc/GradientTranslucentWindowDemo.java) example. If the underlying platform does not support per-pixel translucency, this example exits. The code specifically relating to creating the gradient window is shown in bold.

```

import java.awt.*;
import javax.swing.*;
import static java.awt.GraphicsDevice.WindowTranslucency.*;

public class GradientTranslucentWindowDemo extends JFrame {
    public GradientTranslucentWindowDemo() {
        super("GradientTranslucentWindow");

        **setBackground(new Color(0,0,0,0));**
        setSize(new Dimension(300,200));
        setLocationRelativeTo(null);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        **JPanel panel = new JPanel() {**
            <b>@Override
            protected void paintComponent(Graphics g) {
                if (g instanceof Graphics2D) {
                    final int R = 240;
                    final int G = 240;
                    final int B = 240;

                    Paint p =
                        new GradientPaint(0.0f, 0.0f, new Color(R, G, B, 0),
                            0.0f, getHeight(), new Color(R, G, B, 255), true);
                    Graphics2D g2d = (Graphics2D)g;
                    g2d.setPaint(p);
                    g2d.fillRect(0, 0, getWidth(), getHeight());
                }
            }
        };</b>
        setContentPane(panel);
        setLayout(new GridBagLayout());
        add(new JButton("I am a Button"));
    }

    public static void main(String[] args) {
        // Determine what the GraphicsDevice can support.
        <b>GraphicsEnvironment ge = 
            GraphicsEnvironment.getLocalGraphicsEnvironment();
        GraphicsDevice gd = ge.getDefaultScreenDevice();</b>
        boolean isPerPixelTranslucencySupported = 
            gd.isWindowTranslucencySupported(PERPIXEL_TRANSLUCENT);

        //If translucent windows aren't supported, exit.
        <b>if (!isPerPixelTranslucencySupported) {
            System.out.println(
                "Per-pixel translucency is not supported");
                System.exit(0);
        }</b>

        JFrame.setDefaultLookAndFeelDecorated(true);

        // Create the GUI on the event-dispatching thread
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                GradientTranslucentWindowDemo gtw = new
                    GradientTranslucentWindowDemo();

                // Display the window.
                gtw.setVisible(true);
            }
        });
    }
}

```

Note that the button is not affected by the per-pixel translucency. Setting the per-pixel translucency affects the background pixels only. If you want a window that has a uniformly translucent effect on the background pixels only, you can invoke `setBackground(new Color(0,0,0,alpha))` where `alpha` specifies your desired translucency.

## <a name="shaped" id="shaped">How to Implement a Shaped Window</a>

The best practice for setting the window's shape is to invoke `setShape` in the `componentResized` method of the component event listener. This practice will ensure that the shape is correctly calculated for the actual size of the window. The following example uses this approach.

The
[`<code>ShapedWindowDemo.java`</code>](../examples/misc/ShapedWindowDemoProject/src/misc/ShapedWindowDemo.java) example creates an oval-shaped window with 70 percent opacity. If the underlying platform does not support shaped windows, the example exits. If the underlying platform does not support translucency, the example uses a standard opaque window. You could modify this example to create a shaped window that also uses per-pixel translucency.

The code relating to shaping the window is shown in bold.

```

import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import java.awt.geom.Ellipse2D;
import static java.awt.GraphicsDevice.WindowTranslucency.*;

public class ShapedWindowDemo extends JFrame {
    public ShapedWindowDemo() {
        super("ShapedWindow");
        setLayout(new GridBagLayout());

        // It is best practice to set the window's shape in
        // the componentResized method.  Then, if the window
        // changes size, the shape will be correctly recalculated.
        **addComponentListener(new ComponentAdapter() {**
            // Give the window an elliptical shape.
            // If the window is resized, the shape is recalculated here.
            <b>@Override
            public void componentResized(ComponentEvent e) {
                setShape(new Ellipse2D.Double(0,0,getWidth(),getHeight()));
            }
        });</b>

        **setUndecorated(true);**
        setSize(300,200);
        setLocationRelativeTo(null);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        add(new JButton("I am a Button"));
    }

    public static void main(String[] args) {
        // Determine what the GraphicsDevice can support.
        <b>GraphicsEnvironment ge = 
            GraphicsEnvironment.getLocalGraphicsEnvironment();
        GraphicsDevice gd = ge.getDefaultScreenDevice();</b>
        final boolean isTranslucencySupported = 
            gd.isWindowTranslucencySupported(TRANSLUCENT);

        //If shaped windows aren't supported, exit.
        <b>if (!gd.isWindowTranslucencySupported(PERPIXEL_TRANSPARENT)) {
            System.err.println("Shaped windows are not supported");
            System.exit(0);
        }</b>

        //If translucent windows aren't supported, 
        //create an opaque window.
        if (!isTranslucencySupported) {
            System.out.println(
                "Translucency is not supported, creating an opaque window");
        }

        // Create the GUI on the event-dispatching thread
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                ShapedWindowDemo sw = new ShapedWindowDemo();

                // Set the window to 70% translucency, if supported.
                if (isTranslucencySupported) {
                    sw.setOpacity(0.7f);
                }

                // Display the window.
                sw.setVisible(true);
            }
        });
    }
}

```

## <a name="6u10" id="6u10">Java SE 6 Update 10 API</a>
<th id="h1">Method in Java SE 6 Update 10</th><th id="h2">JDK 7 Equivalent</th>
<td headers="h1">`AWTUtilities.isTranslucencySupported(Translucency)`</td><td headers="h2">`GraphicsDevice.isWindowTranslucencySupported(WindowTranslucency)`</td>
<td headers="h1">`AWTUtilities.isTranslucencyCapable(GraphicsConfiguration)`</td><td headers="h2">`GraphicsConfiguration.isTranslucencyCapable()`</td>
<td headers="h1">`AWTUtilities.setWindowOpacity(Window, float)`</td><td headers="h2">`Window.setOpacity(float)`</td>
<td headers="h1">`AWTUtilities.setWindowShape(Window, Shape)`</td><td headers="h2">`Window.setShape(Shape)`</td>
<td headers="h1">`AWTUtilities.setWindowOpaque(boolean)`</td><td headers="h2">`Window.setBackground(Color)` Passing `new Color(0,0,0,alpha)` to this method, where `alpha` is less than 255, installs per-pixel translucency.</td>
