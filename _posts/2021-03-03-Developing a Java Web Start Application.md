---
title: Developing a Java Web Start Application
author: lijiabao
date: 2020-12-06 12:43:02.918480300 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Developing a Java Web Start Application

Software designed by using 
[component-based architecture](../index.html#componentBasedArch) can easily be developed and deployed as a Java Web Start application. Consider the example of a Java Web Start application with a Swing-based graphical user interface (GUI). With component-based design, the GUI can be built with smaller building blocks or components. The following general steps are used to create an application's GUI:

- Create a `MyTopJPanel` class that is a subclass of `JPanel`. Lay out your application's GUI components in the constructor of the `MyTopJPanel` class.
- Create a class called `MyApplication` that is a subclass of the `JFrame` class.
- In the `main` method of the `MyApplication` class, instantiate the `MyTopJPanel` class and set it as the content pane of the `JFrame`.

The following sections explore these steps in greater detail by using the Dynamic Tree Demo application. If you are not familiar with Swing, see 
[Creating a GUI with Swing](../../uiswing/index.html) to learn more about using Swing GUI components.

Click the following Launch button to launch the Dynamic Tree Demo application.

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
        ....
        
        JButton clearButton = new JButton("Clear");
        ...
        
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

## Creating the Application

For an application that has a Swing-based GUI, create a class that is a subclass of `javax.swing.JFrame`.

Instantiate your top `JPanel` class and set it as the content pane of the `JFrame` in the application's `main` method. The `main` method of the `DynamicTreeApplication` class invokes the `createGUI` method in the AWT Event Dispatcher thread.

```

package webstartComponentArch;

import javax.swing.JFrame;

public class DynamicTreeApplication extends JFrame {
    public static void main(String [] args) {
        DynamicTreeApplication app = new DynamicTreeApplication();
        app.createGUI();
    }

    private void createGUI() {
        //Create and set up the content pane.
        DynamicTreePanel newContentPane = new DynamicTreePanel();
        newContentPane.setOpaque(true); 
        setContentPane(newContentPane);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        pack();
        setVisible(true);
    }    
}

```

## Benefits of Separating Core Functionality From the Final Deployment Mechanism

Another way to create an application is to just remove the layer of abstraction (separate top `JPanel`) and lay out all the controls in the application's `main` method itself. The downside to creating the GUI directly in the application's `main` method is that it will be more difficult to deploy your functionality as an applet, if you choose to do so later.

In the Dynamic Tree Demo example, the core functionality is separated into the `DynamicTreePanel` class. It is now trivial to drop the `DynamicTreePanel` class into a `JApplet` and deploy it as an applet.

Hence, to preserve portability and keep deployment options open, follow component-based design as described in this topic.


[Download source code](examplesIndex.html#DynamicTreeDemo) for the **Dynamic Tree Demo** example to experiment further.
