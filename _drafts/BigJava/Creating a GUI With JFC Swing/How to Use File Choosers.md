
# How to Use File Choosers

File choosers provide a GUI for navigating the file system, and then either choosing a file or directory from a list, or entering the name of a file or directory. To display a file chooser, you usually use the `JFileChooser` API to show a modal [dialog](dialog.html) containing the file chooser. Another way to present a file chooser is to add an instance of 
[`JFileChooser`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html) to a container.

If you intend to distribute your program as a sandbox Java Web Start application, then instead of using the `JFileChooser` API you should use the file services provided by the JNLP API. These services &#151; `FileOpenService` and `FileSaveService` &#151; not only provide support for choosing files in a restricted environment, but also take care of actually opening and saving them. An example of using these services is in [JWSFileChooserDemo](../examples/components/index.html#JWSFileChooserDemo). Documentation for using the JNLP API can be found in the 
[Java Web Start](../../deployment/webstart/index.html) lesson.

Click the Launch button to run JWSFileChooserDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#JWSFileChooserDemo).

When working with the `JWSFileChooserDemo` example, be careful not to lose files that you need. Whenever you click the save button and select an existing file, this demo brings up the File Exists dialog box with a request to replace the file. Accepting the request overwrites the file.

The rest of this section discusses how to use the `JFileChooser` API. A `JFileChooser` object only presents the GUI for choosing files. Your program is responsible for doing something with the chosen file, such as opening or saving it. Refer to 
[Basic I/O](../../essential/io/index.html) for information on how to read and write files.

The `JFileChooser` API makes it easy to bring up open and save dialogs. The type of look and feel determines what these standard dialogs look like and how they differ. In the Java look and feel, the save dialog looks the same as the open dialog, except for the title on the dialog's window and the text on the button that approves the operation. Here is a picture of a standard open dialog in the Java look and feel:

Here is a picture of an application called `FileChooserDemo` that brings up an open dialog and a save dialog.

1. Compile and run the example, consult the [example index](../examples/components/index.html#FileChooserDemo).
1. Click the Open a File button. Navigate around the file chooser, choose a file, and click the dialog's Open button.
1. Use the Save a File button to bring up a save dialog. Try to use all of the controls on the file chooser.
<li>In the source file 
[`FileChooserDemo.java`](../examples/components/FileChooserDemoProject/src/components/FileChooserDemo.java), change the file selection mode to directories-only mode. (Search for `DIRECTORIES_ONLY` and uncomment the line that contains it.) Then compile and run the example again. You will only be able to see and select directories, not ordinary files.</li>

Bringing up a standard open dialog requires only two lines of code:

```

//Create a file chooser
final JFileChooser fc = new JFileChooser();
...
**//In response to a button click:**
int returnVal = fc.showOpenDialog(**aComponent**);

```

The argument to the `showOpenDialog` method specifies the parent component for the dialog. The parent component affects the position of the dialog and the frame that the dialog depends on. For example, the Java look and feel places the dialog directly over the parent component. If the parent component is in a frame, then the dialog is dependent on that frame. This dialog disappears when the frame is minimized and reappears when the frame is maximized.

By default, a file chooser that has not been shown before displays all files in the user's home directory. You can specify the file chooser's initial directory by using one of `JFileChooser`'s other constructors, or you can set the directory with the `setCurrentDirectory` method.

The call to `showOpenDialog` appears in the `actionPerformed` method of the Open a File button's action listener:

```

public void actionPerformed(ActionEvent e) {
    //Handle open button action.
    if (e.getSource() == openButton) {
        int returnVal = fc.showOpenDialog(FileChooserDemo.this);

        if (returnVal == JFileChooser.APPROVE_OPTION) {
            File file = fc.getSelectedFile();
            //This is where a real application would open the file.
            log.append("Opening: " + file.getName() + "." + newline);
        } else {
            log.append("Open command cancelled by user." + newline);
        }
   } ...
}

```

The `show**Xxx**Dialog` methods return an integer that indicates whether the user selected a file. Depending on how you use a file chooser, it is often sufficient to check whether the return value is `APPROVE_OPTION` and then not to change any other value. To get the chosen file (or directory, if you set up the file chooser to allow directory selections), call the `getSelectedFile` method on the file chooser. This method returns an instance of 
[`File`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html).

The example obtains the name of the file and uses it in the log message. You can call other methods on the `File` object, such as `getPath`, `isDirectory`, or `exists` to obtain information about the file. You can also call other methods such as `delete` and `rename` to change the file in some way. Of course, you might also want to open or save the file by using one of the reader or writer classes provided by the Java platform. See 
[Basic I/O](../../essential/io/index.html) for information about using readers and writers to read and write data to the file system.

The example program uses the same instance of the `JFileChooser` class to display a standard save dialog. This time the program calls `showSaveDialog`:

```

int returnVal = fc.showSaveDialog(FileChooserDemo.this);

```

By using the same file chooser instance to display its open and save dialogs, the program reaps the following benefits:

- The chooser remembers the current directory between uses, so the open and save versions automatically share the same current directory.
- You have to customize only one file chooser, and the customizations apply to both the open and save versions.

Finally, the example program has commented-out lines of code that let you change the file selection mode. For example, the following line of code makes the file chooser able to select only directories, and not files:

```

fc.setFileSelectionMode(JFileChooser.DIRECTORIES_ONLY);

```

Another possible selection mode is `FILES_AND_DIRECTORIES`. The default is `FILES_ONLY`. The following picture shows an open dialog with the file selection mode set to `DIRECTORIES_ONLY`. Note that, in the Java look and feel at least, only directories are visible &#151; not files.

If you want to create a file chooser for a task other than opening or saving, or if you want to customize the file chooser, keep reading. 
We will discuss the following topics:



-  [Another Example: FileChooserDemo2](#advancedexample)
-  [Using a File Chooser for a Custom Task](#customtask)
-  [Filtering the List of Files](#filters)
-  [Customizing the File View](#fileview)
-  [Providing an Accessory Component](#accessory)
-  [The File Chooser API](#api)
-  [Examples that Use File Choosers](#eg)

## <a name="advancedexample__1" id="advancedexample__1">Another Example: FileChooserDemo2</a>

Let us look at [`FileChooserDemo2`](../examples/components/index.html#FileChooserDemo2) example, a modified version of the previous demo program that uses more of the `JFileChooser` API. This example uses a file chooser that has been customized in several ways. Like the original example, the user invokes a file chooser with the push of a button. Here is a picture of the file chooser:

As the figure shows, this file chooser has been customized for a special task (Attach), provides a user-choosable file filter (Just Images), uses a special file view for image files, and has an accessory component that displays a thumbnail sketch of the currently selected image file.

The remainder of this section shows you the code that creates and customizes this file chooser. See the [example index](../examples/components/index.html#FileChooserDemo2) for links to all the files required by this example.

## <a name="customtask" id="customtask">Using a File Chooser for a Custom Task</a>

As you have seen, the `JFileChooser` class provides the `showOpenDialog` method for displaying an open dialog and the `showSaveDialog` method for displaying a save dialog.

The class has another method, `showDialog`, for displaying a file chooser for a custom task in a dialog. In the Java look and feel, the only difference between this dialog and the other file chooser dialogs is the title on the dialog window and the label on the approve button. Here is the code from `FileChooserDemo2` that brings up the file chooser dialog for the Attach task:

```

JFileChooser fc = new JFileChooser();
int returnVal = fc.showDialog(FileChooserDemo2.this, "Attach");

```

The first argument to the `showDialog` method is the parent component for the dialog. The second argument is a `String` object that provides both the title for the dialog window and the label for the approve button.

Once again, the file chooser doesn't do anything with the selected file. The program is responsible for implementing the custom task for which the file chooser was created.

## <a name="filters" id="filters">Filtering the List of Files</a>

By default, a file chooser displays all of the files and directories that it detects, except for hidden files. A program can apply one or more **file filters** to a file chooser so that the chooser shows only some files. The file chooser calls the filter's `accept` method for each file to determine whether it should be displayed. A file filter accepts or rejects a file based on criteria such as file type, size, ownership, and so on. Filters affect the list of files displayed by the file chooser. The user can enter the name of any file even if it is not displayed.

`JFileChooser` supports three different kinds of filtering. The filters are checked in the order listed here. For example, an application-controlled filter sees only those files accepted by the built-in filtering.

```

fc.addChoosableFileFilter(new ImageFilter());

```

```

fc.setAcceptAllFileFilterUsed(false);

```

```

public boolean accept(File f) {
    <strong>if (f.isDirectory()) {
        return true;
    }</strong>

    String extension = Utils.getExtension(f);
    if (extension != null) {
        if (extension.equals(Utils.tiff) ||
            extension.equals(Utils.tif) ||
            extension.equals(Utils.gif) ||
            extension.equals(Utils.jpeg) ||
            extension.equals(Utils.jpg) ||
            extension.equals(Utils.png)) {
                return true;
        } else {
            return false;
        }
    }

    return false;
}

```

The preceding code sample uses the `getExtension` method and several string constants from 
[`Utils.java`](../examples/components/FileChooserDemo2Project/src/components/Utils.java), shown here:

```

public class Utils {

    public final static String jpeg = "jpeg";
    public final static String jpg = "jpg";
    public final static String gif = "gif";
    public final static String tiff = "tiff";
    public final static String tif = "tif";
    public final static String png = "png";

    /*
     * Get the extension of a file.
     */  
    public static String getExtension(File f) {
        String ext = null;
        String s = f.getName();
        int i = s.lastIndexOf('.');

        if (i &gt; 0 &amp;&amp;  i &lt; s.length() - 1) {
            ext = s.substring(i+1).toLowerCase();
        }
        return ext;
    }
}

```

## <a name="fileview" id="fileview">Customizing the File View</a>

In the Java look and feel, the chooser's list shows each file's name and displays a small icon that represents whether the file is a true file or a directory. You can customize this **file view** by creating a custom subclass of 
[`FileView`](https://docs.oracle.com/javase/8/docs/api/javax/swing/filechooser/FileView.html) and using an instance of the class as an argument to the `setFileView` method. The example uses an instance of a custom class, implemented in 
[`ImageFileView.java`](../examples/components/FileChooserDemo2Project/src/components/ImageFileView.java), as the file chooser's file view.

```

fc.setFileView(new ImageFileView());

```

The `ImageFileView` class shows a different icon for each type of image accepted by the image filter described previously.

The `ImageFileView` class overrides the five abstract methods defined in the `FileView` as follows.

```

public String getTypeDescription(File f) {
    String extension = Utils.getExtension(f);
    String type = null;

    if (extension != null) {
        if (extension.equals(Utils.jpeg) ||
            extension.equals(Utils.jpg)) {
            type = "JPEG Image";
        } else if (extension.equals(Utils.gif)){
            type = "GIF Image";
        } else if (extension.equals(Utils.tiff) ||
                   extension.equals(Utils.tif)) {
            type = "TIFF Image";
        } else if (extension.equals(Utils.png)){
            type = "PNG Image";
        }
    }
    return type;
}

```

```

public Icon getIcon(File f) {
    String extension = Utils.getExtension(f);
    Icon icon = null;

    if (extension != null) {
        if (extension.equals(Utils.jpeg) ||
            extension.equals(Utils.jpg)) {
            icon = jpgIcon;
        } else if (extension.equals(Utils.gif)) {
            icon = gifIcon;
        } else if (extension.equals(Utils.tiff) ||
                   extension.equals(Utils.tif)) {
            icon = tiffIcon;
        } else if (extension.equals(Utils.png)) {
            icon = pngIcon;
        }
    }
    return icon;
}

```

## <a name="accessory" id="accessory">Providing an Accessory Component</a>

The customized file chooser in `FileChooserDemo2` has an accessory component. If the currently selected item is a PNG, JPEG, TIFF, or GIF image, the accessory component displays a thumbnail sketch of the image. Otherwise, the accessory component is empty. Aside from a previewer, probably the most common use for the accessory component is a panel with more controls on it such as checkboxes that toggle between features.

The example calls the `setAccessory` method to establish an instance of the `ImagePreview` class, implemented in 
[`ImagePreview.java`](../examples/components/FileChooserDemo2Project/src/components/ImagePreview.java), as the chooser's accessory component:

```

fc.setAccessory(new ImagePreview(fc));

```

Any object that inherits from the `JComponent` class can be an accessory component. The component should have a preferred size that looks good in the file chooser.

The file chooser fires a property change event when the user selects an item in the list. A program with an accessory component must register to receive these events to update the accessory component whenever the selection changes. In the example, the `ImagePreview` object itself registers for these events. This keeps all the code related to the accessory component together in one class.

Here is the example's implementation of the `propertyChange` method, which is the method called when a property change event is fired:

```

//**where member variables are declared**
File file = null;
...
public void propertyChange(PropertyChangeEvent e) {
    boolean update = false;
    String prop = e.getPropertyName();

    //If the directory changed, don't show an image.
    if (JFileChooser.DIRECTORY_CHANGED_PROPERTY.equals(prop)) {
        file = null;
        update = true;

    //If a file became selected, find out which one.
    } else if (JFileChooser.SELECTED_FILE_CHANGED_PROPERTY.equals(prop)) {
        file = (File) e.getNewValue();
        update = true;
    }

    //Update the preview accordingly.
    if (update) {
        thumbnail = null;
        if (isShowing()) {
            loadImage();
            repaint();
        }
    }
}

```

If `SELECTED_FILE_CHANGED_PROPERTY` is the property that changed, this method obtains a `File` object from the file chooser. The `loadImage` and `repaint` methods use the `File` object to load the image and repaint the accessory component.

## <a name="api" id="api">The File Chooser API</a>

The API for using file choosers falls into these categories:

- [Creating and Showing the File Chooser](#show)
- [Selecting Files and Directories](#selection)
- [Navigating the File Chooser's List](#navigation)
- [Customizing the File Chooser](#customization)
<th id="h1" align="left">Method or Constructor</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[JFileChooser()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#JFileChooser--)<br />[JFileChooser(File)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#JFileChooser-java.io.File-)<br />[JFileChooser(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#JFileChooser-java.lang.String-)</td><td headers="h2">Creates a file chooser instance. The `File` and `String` arguments, when present, provide the initial directory.</td>
<td headers="h1">[int showOpenDialog(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#showOpenDialog-java.awt.Component-)<br />[int showSaveDialog(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#showSaveDialog-java.awt.Component-)<br />[int showDialog(Component, String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#showDialog-java.awt.Component-java.lang.String-)</td><td headers="h2">Shows a modal dialog containing the file chooser. These methods return `APPROVE_OPTION` if the user approved the operation and `CANCEL_OPTION` if the user cancelled it. Another possible return value is `ERROR_OPTION`, which means an unanticipated error occurred.</td>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[void setSelectedFile(File)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setSelectedFile-java.io.File-)<br />[File getSelectedFile()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getSelectedFile--)</td><td headers="h102">Sets or obtains the currently selected file or (if directory selection has been enabled) directory.</td>
<td headers="h101">[void setSelectedFiles(File[])](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setSelectedFiles-java.io.File:A-)<br />[File[] getSelectedFiles()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getSelectedFiles--)</td><td headers="h102">Sets or obtains the currently selected files if the file chooser is set to allow multiple selection.</td>
<td headers="h101">[void setFileSelectionMode(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setFileSelectionMode-int-)<br />[void getFileSelectionMode()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getFileSelectionMode--)<br />[boolean isDirectorySelectionEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#isDirectorySelectionEnabled--)<br />[boolean isFileSelectionEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#isFileSelectionEnabled--)</td><td headers="h102">Sets or obtains the file selection mode. Acceptable values are `FILES_ONLY` (the default), `DIRECTORIES_ONLY`, and `FILES_AND_DIRECTORIES`.<br />Interprets whether directories or files are selectable according to the current selection mode.</td>
<td headers="h101">[void setMultiSelectionEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setMultiSelectionEnabled-boolean-)<br />[boolean isMultiSelectionEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#isMultiSelectionEnabled--)</td><td headers="h102">Sets or interprets whether multiple files can be selected at once. By default, a user can choose only one file.</td>
<td headers="h101">[void setAcceptAllFileFilterUsed(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setAcceptAllFileFilterUsed-boolean-)<br />[boolean isAcceptAllFileFilterUsed()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#isAcceptAllFileFilterUsed--)</td><td headers="h102">Sets or obtains whether the `AcceptAll` file filter is used as an allowable choice in the choosable filter list; the default value is `true`.</td>
<td headers="h101">[Dialog createDialog(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#createDialog-java.awt.Component-)</td><td headers="h102">Given a parent component, creates and returns a new dialog that contains this file chooser, is dependent on the parent's frame, and is centered over the parent.</td>
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[void ensureFileIsVisible(File)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#ensureFileIsVisible-java.io.File-)</td><td headers="h202">Scrolls the file chooser's list such that the indicated file is visible.</td>
<td headers="h201">[void setCurrentDirectory(File)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setCurrentDirectory-java.io.File-)<br />[File getCurrentDirectory()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getCurrentDirectory--)</td><td headers="h202">Sets or obtains the directory whose files are displayed in the file chooser's list.</td>
<td headers="h201">[void changeToParentDirectory()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#changeToParentDirectory--)</td><td headers="h202">Changes the list to display the current directory's parent.</td>
<td headers="h201">[void rescanCurrentDirectory()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#rescanCurrentDirectory--)</td><td headers="h202">Checks the file system and updates the chooser's list.</td>
<td headers="h201">[void setDragEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setDragEnabled-boolean-)<br />[boolean getDragEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getDragEnabled--)</td><td headers="h202">Sets or obtains the property that determines whether automatic drag handling is enabled. See [Drag and Drop and Data Transfer](../dnd/index.html) for more details.</td>
<th id="h301" align="left">Method</th><th id="h302" align="left">Purpose</th>
<td headers="h301">[void setAccessory(javax.swing.JComponent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setAccessory-javax.swing.JComponent-)<br />[JComponent getAccessory()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getAccessory--)</td><td headers="h302">Sets or obtains the file chooser's accessory component.</td>
<td headers="h301">[void setFileFilter(FileFilter)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setFileFilter-javax.swing.filechooser.FileFilter-)<br />[FileFilter getFileFilter()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getFileFilter--)</td><td headers="h302">Sets or obtains the file chooser's primary file filter.</td>
<td headers="h301">[void setFileView(FileView)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setFileView-javax.swing.filechooser.FileView-)<br />[FileView getFileView()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getFileView--)</td><td headers="h302">Sets or obtains the chooser's file view.</td>
<td headers="h301">[FileFilter[] getChoosableFileFilters()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getChoosableFileFilters--)<br />[void addChoosableFileFilter(FileFilter)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#addChoosableFileFilter-javax.swing.filechooser.FileFilter-)<br />[boolean removeChoosableFileFilter(FileFilter)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#removeChoosableFileFilter-javax.swing.filechooser.FileFilter-)<br />[void resetChoosableFileFilters()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#resetChoosableFileFilters--)<br />[FileFilter getAcceptAllFileFilter()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getAcceptAllFileFilter--)</td><td headers="h302">Sets, obtains, or modifies the list of user-choosable file filters.</td>
<td headers="h301">[void setFileHidingEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setFileHidingEnabled-boolean-)<br />[boolean isFileHidingEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#isFileHidingEnabled--)</td><td headers="h302">Sets or obtains whether hidden files are displayed.</td>
<td headers="h301">[void setControlButtonsAreShown(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#setControlButtonsAreShown-boolean-)<br />[boolean getControlButtonsAreShown()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFileChooser.html#getControlButtonsAreShown--)</td><td headers="h302">Sets or obtains the property that indicates whether the Approve and Cancel buttons are shown in the file chooser. This property is true by default.</td>

## <a name="eg" id="eg">Examples That Use File Choosers</a>

This table shows the examples that use file choosers and points to where those examples are described.
<th id="h401" align="left">Example</th><th id="h402" align="left">Where Described</th><th id="h403" align="left">Notes</th>
<td headers="h401">[`FileChooserDemo`](../examples/components/index.html#FileChooserDemo)</td><td headers="h402">This section</td><td headers="h403">Displays an open dialog and a save dialog.</td>
<td headers="h401">[`FileChooserDemo2`](../examples/components/index.html#FileChooserDemo2)</td><td headers="h402">This section</td><td headers="h403">Uses a file chooser with custom filtering, a custom file view, and an accessory component.</td>
<td headers="h401">[`JWSFileChooserDemo`](../examples/components/index.html#JWSFileChooserDemo)</td><td headers="h402">This section</td><td headers="h403">Uses the JNLP API to open and save files.</td>
