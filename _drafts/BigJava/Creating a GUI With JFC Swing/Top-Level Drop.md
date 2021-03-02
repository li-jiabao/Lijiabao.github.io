
# Top-Level Drop

Up until now, we have primarily focused on attaching a `TransferHandler` to one of the `JComponent` subclasses. But you can also set a `TransferHandler` directly on a top-level container, such as `JFrame` and `JDialog`.

This is particularly useful for applications that import files, such as editors, IDEs, image manipulation programs, CD burning programs. Such an application generally includes a menu, a toolbar, an area for editing documents, and probably a list or mechanism for switching between open documents.

We have such an example but because this demo reads files, we do not provide a Java Web Start version &#8212; you will have to download and compile the demo yourself.

As you can see in the screen shot below, `TopLevelTransferHandlerDemo` has a menu (empty, except for the Demo submenu), a (non-functional) toolbar, an area (on the left) that displays a list of open documents, and a area (to the right) that displays the content of each open document. At startup the blue document area has been assigned a transfer handler that supports file imports &#8212; so is the only place that can accept a drop.

<li>Compile and run the 
[`<code>TopLevelTransferHandlerDemo`</code>](../examples/dnd/TopLevelTransferHandlerDemoProject/src/dnd/TopLevelTransferHandlerDemo.java) example, consult the [example index](../examples/dnd/index.html#TopLevelTransferHandlerDemo) if you would like to download a zip file structured for NetBeans.</li>
1. Drag a file from your native desktop or file system and drop it on the blue document area to the right. The file is opened and a frame filled with its contents will appear. The document area, a `JDesktopPane`, contains a transfer handler that supports import of `javaFileListFlavor`.
1. Drag another file and attempt to drop it on the document area. You will find that you cannot drop it on top of the frame displaying the last file. You also cannot drop it on the list, the menu, or the toolbar. The only place you can drop is the blue portion of the document area or on the menu bar of a previously opened frame. Inside each content frame there is a text component's transfer handler that doesn't understand a file drop &#8212; you can drop text into that area, but not a file.
1. From the menu, choose Demo-&gt;Use Top-Level TransferHandler to install the transfer handler on the top-level container &#8212; a `JFrame`.
1. Try dragging over the demo again. The number of areas that accept drops has increased. You can now drop most anywhere on the application including the menu bar, toolbar, the frame's title bar, except for the list (on the left) or the content area of a previously opened file. Neither the `JList`'s nor the text area's transfer handlers know how to import files.
1. Disable the transfer handlers on those remaining components by choosing Demo-&gt;Remove TransferHandler from List and Text from the menu.
1. Drag over the demo again. You can now drop a file **anywhere** on the application!
1. From the menu, choose Demo-&gt;Use COPY Action.
<li>Drag over the demo again. Note that the mouse cursor now shows the COPY cursor &#8212; this provides more accurate feedback because a successful drop does not remove the file from the source. The target can be programmed to select from the available drop actions as described in 
[Choosing the Drop Action](dropaction.html).</li>

Note one undesirable side effect of disabling the default transfer handler on the text component: You can no longer drag and drop (or cut/copy/paste) text within the editing area. To fix this, you will need to implement a custom transfer handler for the text component that accepts file drops and also re-implements the missing support for text transfers. You might want to watch 
[RFE 4830695](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=4830695) which would allow adding data import on top of an existing `TransferHandler`.

Here is the source code for 
[`TopLevelTransferHandlerDemo.java`](../examples/dnd/TopLevelTransferHandlerDemoProject/src/dnd/TopLevelTransferHandlerDemo.java):

```

/**
 * Demonstration of the top-level {@code TransferHandler}
 * support on {@code JFrame}.
 */
public class TopLevelTransferHandlerDemo extends JFrame {
    
    private static boolean DEMO = false;

    private JDesktopPane dp = new JDesktopPane();
    private DefaultListModel listModel = new DefaultListModel();
    private JList list = new JList(listModel);
    private static int left;
    private static int top;
    private JCheckBoxMenuItem copyItem;
    private JCheckBoxMenuItem nullItem;
    private JCheckBoxMenuItem thItem;

    private class Doc extends InternalFrameAdapter implements ActionListener {
        String name;
        JInternalFrame frame;
        TransferHandler th;
        JTextArea area;

        public Doc(File file) {
            this.name = file.getName();
            try {
                init(file.toURI().toURL());
            } catch (MalformedURLException e) {
                e.printStackTrace();
            }
        }
        
        public Doc(String name) {
            this.name = name;
            init(getClass().getResource(name));
        }
        
        private void init(URL url) {
            frame = new JInternalFrame(name);
            frame.addInternalFrameListener(this);
            listModel.add(listModel.size(), this);

            area = new JTextArea();
            area.setMargin(new Insets(5, 5, 5, 5));

            try {
                BufferedReader reader = new BufferedReader(new InputStreamReader(url.openStream()));
                String in;
                while ((in = reader.readLine()) != null) {
                    area.append(in);
                    area.append("\n");
                }
                reader.close();
            } catch (Exception e) {
                e.printStackTrace();
                return;
            }

            th = area.getTransferHandler();
            area.setFont(new Font("monospaced", Font.PLAIN, 12));
            area.setCaretPosition(0);
            area.setDragEnabled(true);
            area.setDropMode(DropMode.INSERT);
            frame.getContentPane().add(new JScrollPane(area));
            dp.add(frame);
            frame.show();
            if (DEMO) {
                frame.setSize(300, 200);
            } else {
                frame.setSize(400, 300);
            }
            frame.setResizable(true);
            frame.setClosable(true);
            frame.setIconifiable(true);
            frame.setMaximizable(true);
            frame.setLocation(left, top);
            incr();
            SwingUtilities.invokeLater(new Runnable() {
                public void run() {
                    select();
                }
            });
            nullItem.addActionListener(this);
            setNullTH();
        }

        public void internalFrameClosing(InternalFrameEvent event) {
            listModel.removeElement(this);
            nullItem.removeActionListener(this);
        }

        public void internalFrameOpened(InternalFrameEvent event) {
            int index = listModel.indexOf(this);
            list.getSelectionModel().setSelectionInterval(index, index);
        }

        public void internalFrameActivated(InternalFrameEvent event) {
            int index = listModel.indexOf(this);
            list.getSelectionModel().setSelectionInterval(index, index);
        }

        public String toString() {
            return name;
        }
        
        public void select() {
            try {
                frame.toFront();
                frame.setSelected(true);
            } catch (java.beans.PropertyVetoException e) {}
        }
        
        public void actionPerformed(ActionEvent ae) {
            setNullTH();
        }
        
        public void setNullTH() {
            if (nullItem.isSelected()) {
                area.setTransferHandler(null);
            } else {
                area.setTransferHandler(th);
            }
        }
    }

    private TransferHandler handler = new TransferHandler() {
        public boolean canImport(TransferHandler.TransferSupport support) {
            if (!support.isDataFlavorSupported(DataFlavor.javaFileListFlavor)) {
                return false;
            }

            if (copyItem.isSelected()) {
                boolean copySupported = (COPY &amp; support.getSourceDropActions()) == COPY;

                if (!copySupported) {
                    return false;
                }

                support.setDropAction(COPY);
            }

            return true;
        }

        public boolean importData(TransferHandler.TransferSupport support) {
            if (!canImport(support)) {
                return false;
            }
            
            Transferable t = support.getTransferable();

            try {
                java.util.List&lt;File&gt; l =
                    (java.util.List&lt;File&gt;)t.getTransferData(DataFlavor.javaFileListFlavor);

                for (File f : l) {
                    new Doc(f);
                }
            } catch (UnsupportedFlavorException e) {
                return false;
            } catch (IOException e) {
                return false;
            }

            return true;
        }
    };

    private static void incr() {
        left += 30;
        top += 30;
        if (top == 150) {
            top = 0;
        }
    }

    public TopLevelTransferHandlerDemo() {
        super("TopLevelTransferHandlerDemo");
        setJMenuBar(createDummyMenuBar());
        getContentPane().add(createDummyToolBar(), BorderLayout.NORTH);

        JSplitPane sp = new JSplitPane(JSplitPane.HORIZONTAL_SPLIT, list, dp);
        sp.setDividerLocation(120);
        getContentPane().add(sp);
        //new Doc("sample.txt");
        //new Doc("sample.txt");
        //new Doc("sample.txt");

        list.getSelectionModel().setSelectionMode(ListSelectionModel.SINGLE_SELECTION);

        list.addListSelectionListener(new ListSelectionListener() {
            public void valueChanged(ListSelectionEvent e) {
                if (e.getValueIsAdjusting()) {
                    return;
                }
                
                Doc val = (Doc)list.getSelectedValue();
                if (val != null) {
                    val.select();
                }
             }
        });
        
        final TransferHandler th = list.getTransferHandler();

        nullItem.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent ae) {
                if (nullItem.isSelected()) {
                    list.setTransferHandler(null);
                } else {
                    list.setTransferHandler(th);
                }
            }
        });
        thItem.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent ae) {
                if (thItem.isSelected()) {
                    setTransferHandler(handler);
                } else {
                    setTransferHandler(null);
                }
            }
        });
        dp.setTransferHandler(handler);
    }

    private static void createAndShowGUI(String[] args) {
        try {
            UIManager.setLookAndFeel(UIManager.getSystemLookAndFeelClassName());
        } catch (Exception e) {
        }

        TopLevelTransferHandlerDemo test = new TopLevelTransferHandlerDemo();
        test.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        if (DEMO) {
            test.setSize(493, 307);
        } else {
            test.setSize(800, 600);
        }
        test.setLocationRelativeTo(null);
        test.setVisible(true);
        test.list.requestFocus();
    }
    
    public static void main(final String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                //Turn off metal's use of bold fonts
                UIManager.put("swing.boldMetal", Boolean.FALSE);
                createAndShowGUI(args);
            }
        });
    }
    
    private JToolBar createDummyToolBar() {
        JToolBar tb = new JToolBar();
        JButton b;
        b = new JButton("New");
        b.setRequestFocusEnabled(false);
        tb.add(b);
        b = new JButton("Open");
        b.setRequestFocusEnabled(false);
        tb.add(b);
        b = new JButton("Save");
        b.setRequestFocusEnabled(false);
        tb.add(b);
        b = new JButton("Print");
        b.setRequestFocusEnabled(false);
        tb.add(b);
        b = new JButton("Preview");
        b.setRequestFocusEnabled(false);
        tb.add(b);
        tb.setFloatable(false);
        return tb;
    }
    
    private JMenuBar createDummyMenuBar() {
        JMenuBar mb = new JMenuBar();
        mb.add(createDummyMenu("File"));
        mb.add(createDummyMenu("Edit"));
        mb.add(createDummyMenu("Search"));
        mb.add(createDummyMenu("View"));
        mb.add(createDummyMenu("Tools"));
        mb.add(createDummyMenu("Help"));
        
        JMenu demo = new JMenu("Demo");
        demo.setMnemonic(KeyEvent.VK_D);
        mb.add(demo);

        thItem = new JCheckBoxMenuItem("Use Top-Level TransferHandler");
        thItem.setMnemonic(KeyEvent.VK_T);
        demo.add(thItem);

        nullItem = new JCheckBoxMenuItem("Remove TransferHandler from List and Text");
        nullItem.setMnemonic(KeyEvent.VK_R);
        demo.add(nullItem);

        copyItem = new JCheckBoxMenuItem("Use COPY Action");
        copyItem.setMnemonic(KeyEvent.VK_C);
        demo.add(copyItem);

        return mb;
    }
    
    private JMenu createDummyMenu(String str) {
        JMenu menu = new JMenu(str);
        JMenuItem item = new JMenuItem("[Empty]");
        item.setEnabled(false);
        menu.add(item);
        return menu;
    }
}

```
