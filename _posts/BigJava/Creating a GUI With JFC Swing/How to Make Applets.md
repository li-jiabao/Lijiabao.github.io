
# How to Make Applets

This section covers `JApplet` &#151; a class that enables applets to use Swing components. `JApplet` is a subclass of 
[`java.applet.Applet`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html), which is covered in the 
[Java Applets](../../deployment/applet/index.html) trail. If you've never written a regular applet before, we urge you to read that trail before proceeding with this section. The information provided in that trail applies to Swing applets, with a few exceptions that this section explains.

<a name="applet" id="applet"></a> Any applet that contains Swing components must be implemented with a subclass of 
[`JApplet`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html). Here's a Swing version of one of the applets that helped make Java famous &#151; an animation applet that (in its most well known configuration) shows our mascot Duke doing cartwheels:

You can find the main source code for this applet in 
[`TumbleItem.java`](../examples/components/TumbleItemProject/src/components/TumbleItem.java).

This section discusses the following topics:

- [Features Provided by JApplet](#features)
- [Threads in Applets](#thread)
- [Using Images in a Swing Applet](#images)
- [Embedding an Applet in an HTML Page](#plugin)
- [The JApplet API](#api)
- [Applet Examples](#eg)

## <a name="features" id="features">Features Provided by JApplet</a>

Because `JApplet` is a top-level Swing container, each Swing applet has a root pane. The most noticeable effects of the root pane's presence are support for adding a [menu bar](menu.html) and the need to use a content pane.

As described in [Using Top-Level Containers](toplevel.html), each top-level container such as a `JApplet` has a single content pane. The content pane makes Swing applets different from regular applets in the following ways:

- You add components to a Swing applet's content pane, not directly to the applet. [Adding Components to the Content Pane](toplevel.html#contentpane) shows you how.
- You set the layout manager on a Swing applet's content pane, not directly on the applet.
- The default layout manager for a Swing applet's content pane is `BorderLayout`. This differs from the default layout manager for `Applet`, which is `FlowLayout`.
<li>You should not put painting code directly in a `JApplet` object. See 
[Performing Custom Painting](../painting/index.html) for examples of how to perform custom painting in applets.</li>

## <a name="thread" id="thread">Threads in Applets</a>

Swing components should be created, queried, and manipulated on the event-dispatching thread, but browsers don't invoke applet "milestone" methods from that thread. For this reason, the milestone methods &#151; `init`, `start`, `stop`, and `destroy` &#151; should use the `SwingUtilities` method `invokeAndWait` (or, if appropriate, `invokeLater`) so that code that refers to the Swing components is executed on the event-dispatching thread. More information about these methods and the event-dispatching thread is in 
[Concurrency in Swing](../concurrency/index.html).

Here is an example of an `init` method:

```

public void init() {
    //Execute a job on the event-dispatching thread:
    //creating this applet's GUI.
    try {
        javax.swing.SwingUtilities.invokeAndWait(new Runnable() {
            public void run() {
                createGUI();
            }
        });
    } catch (Exception e) {
        System.err.println("createGUI didn't successfully complete");
    }
}

private void createGUI() {
    JLabel label = new JLabel(
                       "You are successfully running a Swing applet!");
    label.setHorizontalAlignment(JLabel.CENTER);
    label.setBorder(BorderFactory.createMatteBorder(1,1,1,1,Color.black));
    getContentPane().add(label, BorderLayout.CENTER);
}

```

The `invokeLater` method is not appropriate for this implementation because it allows `init` to return before initialization is complete, which can cause applet problems that are difficult to debug.

The `init` method in `TumbleItem` is more complex, as the following code shows. Like the first example, this `init` method implementation uses `SwingUtilities.invokeAndWait` to execute the GUI creation code on the event-dispatching thread. This `init` method sets up a 
[Swing timer](../misc/timer.html) to fire action events the update the animation. Also, `init` uses 
[`javax.swing.SwingWorker`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html) to create a background task that loads the animation image files, letting the applet present a GUI right away, without waiting for all resources to be loaded.

```

private void createGUI() {
    ...
    animator = new Animator();
    animator.setOpaque(true);
    animator.setBackground(Color.white);
    setContentPane(animator);
    ...
}

public void init() {
    loadAppletParameters();

    //Execute a job on the event-dispatching thread:
    //creating this applet's GUI.
    try {
        <b>javax.swing.SwingUtilities.invokeAndWait(new Runnable() {
            public void run() {
                createGUI();
            }
        });</b>
    } catch (Exception e) { 
        System.err.println("createGUI didn't successfully complete");
    }

    //Set up the timer that will perform the animation.
    timer = new javax.swing.Timer(speed, this);
    timer.setInitialDelay(pause);
    timer.setCoalesce(false);
    timer.start(); //Start the animation.

    //Background task for loading images.
    SwingWorker worker = (new SwingWorker&lt;ImageIcon[], Object&gt;() {
            public ImageIcon[] doInBackground() {
                final ImageIcon[] innerImgs = new ImageIcon[nimgs];
            **...//Load all the images...**
            return imgs;
        }
        public void done() {
            //Remove the "Loading images" label.
            animator.removeAll();
            loopslot = -1;
            try {
                imgs = get();
            } **...//Handle possible exceptions**
        }

    }).execute();
}

```

You can find the applet's source code in 
[`TumbleItem.java`](../examples/components/TumbleItemProject/src/components/TumbleItem.java). To find all the files required for the applet, see the [example index](../examples/components/index.html#TumbleItem).

## <a name="images" id="images">Using Images in a Swing Applet</a>

The `Applet` class provides the `getImage` method for loading images into an applet. The `getImage` method creates and returns an `Image` object that represents the loaded image. Because Swing components use `Icon`s rather than `Image`s to refer to pictures, Swing applets tend not to use `getImage`. Instead Swing applets create instances of `ImageIcon` &#151; an icon loaded from an image file. `ImageIcon` comes with a code-saving benefit: it handles image tracking automatically. Refer to 
[How to Use Icons](../components/icon.html) for more information.

The animation of Duke doing cartwheels requires 17 different pictures. The applet uses one `ImageIcon` per picture and loads them in its `init` method. Because images can take a long time to load, the icons are loaded in a separate thread implemented by a `SwingWorker` object. Here's the code:

```

public void init() {
    ...
    imgs = new ImageIcon[nimgs];
    (new SwingWorker&lt;ImageIcon[], Object&gt;() {
        public ImageIcon[] doInBackground() {
            //Images are numbered 1 to nimgs,
            //but fill array from 0 to nimgs-1.
            for (int i = 0; i &lt; nimgs; i++) {
                imgs[i] = loadImage(i+1);
            }
            return imgs;
        }
        ...
    }).execute();

}
...
protected ImageIcon loadImage(int imageNum) {
    String path = dir + "/T" + imageNum + ".gif";
    int MAX_IMAGE_SIZE = 2400;  //Change this to the size of
                                 //your biggest image, in bytes.
    int count = 0;
    BufferedInputStream imgStream = new BufferedInputStream(
       this.getClass().getResourceAsStream(path));
    if (imgStream != null) {
        byte buf[] = new byte[MAX_IMAGE_SIZE];
        try {
            count = imgStream.read(buf);
            imgStream.close();
        } catch (java.io.IOException ioe) {
            System.err.println("Couldn't read stream from file: " + path);
            return null;
        }
        if (count &lt;= 0) {
            System.err.println("Empty file: " + path);
            return null;
        }
        return new ImageIcon(Toolkit.getDefaultToolkit().createImage(buf));
    } else {
        System.err.println("Couldn't find file: " + path);
        return null;
    }
}

```

The `loadImage` method loads the image for the specified frame of animation. It uses the `getResourceAsStream` method rather than the usual `getResource` method to get the images. The resulting code isn't pretty, but `getResourceAsStream` is more efficient than `getResource` for loading images from JAR files into applets that are executed using Java Plug-in&#8482; software. For further details, see 
[Loading Images Into Applets](../components/icon.html#applet).

## <a name="plugin" id="plugin">Embedding an Applet in an HTML Page</a>

You can deploy a simple applet by using the `applet` tag. Or, you can use the Deployment Toolkit. Here is the code for the cartwheeling Duke applet:

```

&lt;script src="https://www.java.com/js/deployJava.js" type="text/javascript"&gt;
&lt;/script&gt;&lt;script type="text/javascript"&gt;
//&lt;![CDATA[
    var attributes = { archive: 'https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TumbleItemProject/TumbleItem.jar',
                       codebase: 'https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TumbleItemProject',
                       code:'components.TumbleItem', width:'600', height:'95' };
    var parameters = { permissions:'sandbox', nimgs:'17', offset:'-57',
                       img: 'images/tumble', maxwidth:'120' };
    deployJava.runApplet(attributes, parameters, '1.7');
//]]&gt;
&lt;/script&gt;&lt;noscript&gt;A browser with Javascript enabled is required for this page to operate properly.&lt;/noscript&gt;

```

For more information, see 
[Deploying an Applet](../../deployment/applet/deployingApplet.html) in the 
[Java Applets](../../deployment/applet/index.html) lesson.

## <a name="api" id="api">The JApplet API</a>

The next table lists the interesting methods that `JApplet` adds to the applet API. They give you access to features provided by the root pane. Other methods you might use are defined by the 
[`Component`](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html) and 
[`Applet`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html) classes. See [Component Methods](jcomponent.html#complookapi) for a list of commonly used `Component` methods, and 
[Java Applets](../../deployment/applet/index.html) for help in using `Applet` methods.
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[void setContentPane(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setContentPane-java.awt.Container-)<br />[Container getContentPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getContentPane--)</td><td headers="h2">Set or get the applet's content pane. The content pane contains the applet's visible GUI components and should be opaque.</td>
<td headers="h1">[void setRootPane(JRootPane)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setRootPane-javax.swing.JRootPane-)<br />[JRootPane getRootPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getRootPane--)</td><td headers="h2">Create, set, or get the applet's root pane. The root pane manages the interior of the applet including the content pane, the glass pane, and so on.</td>
<td headers="h1">[void setJMenuBar(JMenuBar)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setJMenuBar-javax.swing.JMenuBar-)<br />[JMenuBar getJMenuBar()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getJMenuBar--)</td><td headers="h2">Set or get the applet's menu bar to manage a set of menus for the applet.</td>
<td headers="h1">[void setGlassPane(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setGlassPane-java.awt.Component-)<br />[Component getGlassPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getGlassPane--)</td><td headers="h2">Set or get the applet's glass pane. You can use the glass pane to intercept mouse events.</td>
<td headers="h1">[void setLayeredPane(JLayeredPane)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setLayeredPane-javax.swing.JLayeredPane-)<br />[JLayeredPane getLayeredPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getLayeredPane--)</td><td headers="h2">Set or get the applet's layered pane. You can use the applet's layered pane to put components on top of or behind other components.</td>

## <a name="eg" id="eg">Applet Example</a>

This table shows examples of Swing applets and where those examples are described.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
<td headers="h101">[`TumbleItem`](../examples/components/index.html#TumbleItem)</td><td headers="h102">This page</td><td headers="h103">An animation applet</td>
