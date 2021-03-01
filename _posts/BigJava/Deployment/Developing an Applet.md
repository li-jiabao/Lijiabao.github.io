
# Developing an Applet

An application designed using 
[component-based architecture](../index.html#componentBasedArch) can be developed into a Java applet. Consider the example of a Java applet with a Swing-based graphical user interface (GUI). With component-based design, the GUI can be built with smaller building blocks or components. The following general steps are used to create an applet GUI:

- Create a class `MyTopJPanel` that is a subclass of `javax.swing.JPanel`. Lay out your applet's GUI components in the constructor of the `MyTopJPanel` class.
- Create a class called `MyApplet` that is a subclass of `javax.swing.JApplet`.
- In the `init` method of `MyApplet`, instantiate `MyTopJPanel` and set it as the applet's content pane.

The following sections explore these steps in greater detail by using the Dynamic Tree Demo applet.  If you are not familiar with Swing, see 
[Creating a GUI with Swing](../../uiswing/index.html) to learn more about using Swing GUI components.

## Creating the Top `JPanel` Class

Create a class that is a subclass of `JPanel`. This top `JPanel` acts as a container for all your other UI components. In the following example, the `DynamicTreePanel` class is the topmost `JPanel`. The constructor of the `DynamicTreePanel` class invokes other methods to create and lay out the UI controls properly.

```

public class DynamicTreePanel extends JPanel implements ActionListener {
    private int newNodeSuffix = 1;
    private static String ADD_COMMAND = "add";
    private static String REMOVE_COMMAND = "remove";
    private static String CLEAR_COMMAND = "clear";
    
    private DynamicTree treePanel;

    public DynamicTreePanel() {
        super(new BorderLayout());
        
        //Create the components.
        treePanel = new DynamicTree();
        populateTree(treePanel);

        JButton addButton = new JButton("Add");
        addButton.setActionCommand(ADD_COMMAND);
        addButton.addActionListener(this);
        
        JButton removeButton = new JButton("Remove");
        
        // ...
        
        JButton clearButton = new JButton("Clear");
        
        // ...
        
        //Lay everything out.
        treePanel.setPreferredSize(
            new Dimension(300, 150));
        add(treePanel, BorderLayout.CENTER);

        JPanel panel = new JPanel(new GridLayout(0,3));
        panel.add(addButton);
        panel.add(removeButton); 
        panel.add(clearButton);
        add(panel, BorderLayout.SOUTH);
    }
    // ...
}

```

## Creating the Applet

For a Java applet that has a Swing-based GUI, create a class that is a subclass of `javax.swing.JApplet`. An applet that does not contain a Swing-based GUI can extend the `java.applet.Applet` class.

Override the applet's `init` method to instantiate your top `JPanel` class and create the applet's GUI. The `init` method of the `DynamicTreeApplet` class invokes the `createGUI` method in the AWT Event Dispatcher thread.

```

package appletComponentArch;

import javax.swing.JApplet;
import javax.swing.SwingUtilities;

public class DynamicTreeApplet extends JApplet {
    //Called when this applet is loaded into the browser.
    public void init() {
        //Execute a job on the event-dispatching thread; creating this applet's GUI.
        try {
            SwingUtilities.invokeAndWait(new Runnable() {
                public void run() {
                    createGUI();
                }
            });
        } catch (Exception e) { 
            System.err.println("createGUI didn't complete successfully");
        }
    }
    
    private void createGUI() {
        //Create and set up the content pane.
        DynamicTreePanel newContentPane = new DynamicTreePanel();
        newContentPane.setOpaque(true); 
        setContentPane(newContentPane);        
    }        
}

```

## Benefits of Separating Core Functionality From the Final Deployment Mechanism

Another way to create an applet is to just remove the layer of abstraction (separate top `JPanel`) and lay out all the controls in the applet's `init` method itself. The downside to creating the GUI directly in the applet is that it will now be more difficult to deploy your functionality as a Java Web Start application, if you choose to do so later.

In the Dynamic Tree Demo example, the core functionality resides in the `DynamicTreePanel` class. It is now trivial to drop the `DynamicTreePanel` class into a `JFrame` and deploy as a Java Web Start application.

Hence, to preserve portability and keep deployment options open, follow component-based design as described on this page.
