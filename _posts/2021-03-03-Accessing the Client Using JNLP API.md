---
title: Accessing the Client Using JNLP API
author: lijiabao
date: 2020-12-06 12:43:39.056476700 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Accessing the Client Using JNLP API

When launched by using the Java Network Launch Protocol (JNLP), rich Internet applications (RIAs) can access the client with the user's permission. Consider the Text Editor applet example to understand how to use JNLP API based services. The Text Editor has a text area and buttons labeled Open, Save, and SaveAs. The Text Editor can be used to open an existing text file, edit it, and save it back to disk.

The Text Editor applet is shown next.

The 
[`TextEditor`](examples/applet_JNLP_API/src/TextEditor.java) and 
[`TextEditorApplet`](examples/applet_JNLP_API/src/TextEditor.java) classes lay out the user interface and display it as an applet. The 
[`FileHandler`](examples/applet_JNLP_API/src/FileHandler.java) class contains the core functionality with respect to using JNLP API based services.

Remember, the techniques described in this topic apply to Java Web Start applications as well.

To make use of a JNLP service, first retrieve a reference to the service. The `initialize` method of the `FileHandler` class retrieves references to JNLP services as shown in the following code snippet:

```

private static synchronized void initialize() {
    ...
    try {
        <b>fos = (FileOpenService)
            ServiceManager.lookup("javax.jnlp.FileOpenService");
        fss = (FileSaveService)
            ServiceManager.lookup("javax.jnlp.FileSaveService");</b>
    } catch (UnavailableServiceException e) {
        ...
    }
}

```

After you have a reference to the required services, invoke methods on the service to perform the necessary operations. The `open` method of the `FileHandler` class invokes the `openFileDialog` method of the 
[`FileOpenService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/FileOpenService.html) class to display a file chooser. The `open` method returns the contents of the selected file.

```

public static String open() {
    initialize();
    try {
        **fc = fos.openFileDialog(null, null);**
        return readFromFile(fc);
    } catch (IOException ioe) {
        ioe.printStackTrace(System.out);
        return null;
    }
}

```

Similarly, the `save` and `saveAs` methods of the `FileHandler` class invoke corresponding methods of the 
[`FileSaveService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/FileSaveService.html) class to enable the user to select a file name and save the contents of the text area to disk.

```

public static void saveAs(String txt) {
    initialize();
    try {
        if (fc == null) {
            // If not already saved.
            // Save-as is like save
            save(txt);
        } else {
            <b>fc = fss.saveAsFileDialog(null, null,
                                         fc);</b>
            save(txt);
        }
    } catch (IOException ioe) {
        ioe.printStackTrace(System.out);
    }
}

```

At runtime, when the RIA attempts to open or save a file, users see a security dialog asking them if they want to allow the action. The operation will proceed only if users allow the RIA to access their environment.

The complete source of the 
[`FileHandler`](examples/applet_JNLP_API/src/FileHandler.java) class is shown next.

```


// add javaws.jar to the classpath during compilation 
import javax.jnlp.FileOpenService;
import javax.jnlp.FileSaveService;
import javax.jnlp.FileContents;
import javax.jnlp.ServiceManager;
import javax.jnlp.UnavailableServiceException;
import java.io.*;

public class FileHandler {

    static private FileOpenService fos = null;
    static private FileSaveService fss = null;
    static private FileContents fc = null;

    // retrieves a reference to the JNLP services
    private static synchronized void initialize() {
        if (fss != null) {
            return;
        }
        try {
            fos = (FileOpenService) ServiceManager.lookup("javax.jnlp.FileOpenService");
            fss = (FileSaveService) ServiceManager.lookup("javax.jnlp.FileSaveService");
        } catch (UnavailableServiceException e) {
            fos = null;
            fss = null;
        }
    }

    // displays open file dialog and reads selected file using FileOpenService
    public static String open() {
        initialize();
        try {
            fc = fos.openFileDialog(null, null);
            return readFromFile(fc);
        } catch (IOException ioe) {
            ioe.printStackTrace(System.out);
            return null;
        }
    }

    // displays saveFileDialog and saves file using FileSaveService
    public static void save(String txt) {
        initialize();
        try {
            // Show save dialog if no name is already given
            if (fc == null) {
                fc = fss.saveFileDialog(null, null,
                        new ByteArrayInputStream(txt.getBytes()), null);
                // file saved, done
                return;
            }
            // use this only when filename is known
            if (fc != null) {
                writeToFile(txt, fc);
            }
        } catch (IOException ioe) {
            ioe.printStackTrace(System.out);
        }
    }

    // displays saveAsFileDialog and saves file using FileSaveService
    public static void saveAs(String txt) {
        initialize();
        try {
            if (fc == null) {
                // If not already saved. Save-as is like save
                save(txt);
            } else {
                fc = fss.saveAsFileDialog(null, null, fc);
                save(txt);
            }
        } catch (IOException ioe) {
            ioe.printStackTrace(System.out);
        }
    }

    private static void writeToFile(String txt, FileContents fc) throws IOException {
        int sizeNeeded = txt.length() * 2;
        if (sizeNeeded &gt; fc.getMaxLength()) {
            fc.setMaxLength(sizeNeeded);
        }
        BufferedWriter os = new BufferedWriter(new OutputStreamWriter(fc.getOutputStream(true)));
        os.write(txt);
        os.close();
    }

    private static String readFromFile(FileContents fc) throws IOException {
        if (fc == null) {
            return null;
        }
        BufferedReader br = new BufferedReader(new InputStreamReader(fc.getInputStream()));
        StringBuffer sb = new StringBuffer((int) fc.getLength());
        String line = br.readLine();
        while (line != null) {
            sb.append(line);
            sb.append("\n");
            line = br.readLine();
        }
        br.close();
        return sb.toString();
    }
}

```


[Download source code](examplesIndex.html#AppletJNLPAPI) for the **Text Editor Applet** example to experiment further.
