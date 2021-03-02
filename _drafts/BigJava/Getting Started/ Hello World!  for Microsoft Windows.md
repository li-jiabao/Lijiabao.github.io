
# "Hello World!" for Microsoft Windows

<a name="win32-top" id="win32-top"></a>

It's time to write your first application! The following instructions are for users of Windows Vista, Windows 7, and Windows 8. Instructions for other platforms are in 
["Hello World!" for Solaris OS and Linux](unix.html) and 
["Hello World!" for the NetBeans IDE](netbeans.html).

If you encounter problems with the instructions on this page, consult the 
[Common Problems (and Their Solutions)](../problems/index.html). 


  - [A Checklist](#win32-1)
  <li>[Creating Your First Application](#win32-2)
    <ul>
    - [Create a Source File](#win32-2a)
    - [Compile the Source File into a `.class` File](#win32-2b)
    - [Run the Program](#win32-2c)
    
## <a name="win32-1" id="win32-1"></a> A Checklist&#160; 
<img src="../../figures/getStarted/check.gif" width="56" height="68" alt="a checkmark" />

To write your first program, you'll need:

<li>
The Java SE Development Kit 8 (JDK 8)
<p>You can 
[download the Windows version now](http://www.oracle.com/technetwork/java/javase/downloads/index.html). (Make sure you download the **JDK**, *not* the JRE.) Consult the 
[installation instructions](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html).</p>
</li>
<li>
A text editor
In this example, we'll use Notepad, a simple editor included with the Windows platforms. You can easily adapt these instructions if you use a different text editor.
</li>

These two items are all you'll need to write your first application.


          

## <a name="win32-2" id="win32-2"></a> Creating Your First Application




Your first application, `HelloWorldApp`, will simply display the greeting "Hello world!". To create this program, you will:&#160;

<li>
Create a source file
A source file contains code, written in the Java programming language, that you and other programmers can understand. You can use any text editor to create and edit source files.
</li>
<li>
Compile the source file into a .class file
The Java programming language *compiler* (`javac`) takes your source file and translates its text into instructions that the Java virtual machine can understand. The instructions contained within this file are known as *bytecodes*.
</li>
<li>
Run the program
The Java application *launcher tool* (`java`) uses the Java virtual machine to run your application.
</li>

### <a name="win32-2a" id="win32-2a"></a> Create a Source File

To create a source file, you have two options:

<li>
<p>You can save the file <code>
[`HelloWorldApp.java`](../application/examples/HelloWorldApp.java)</code> on your computer and avoid a lot of typing. Then, you can go straight to [Compile the Source File into a `.class` File](#win32-2b).</p>
</li>
<li>
Or, you can use the following (longer) instructions.
</li>

First, start your editor. You can launch the Notepad editor from the ** Start** menu by selecting ** Programs &gt; Accessories &gt; Notepad**. In a new document, type in the following code:

```

/**
&#160;* The HelloWorldApp class implements an application that
&#160;* simply prints "Hello World!" to standard output.
&#160;*/
class HelloWorldApp {
&#160;&#160;&#160;&#160;public static void main(String[] args) {
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;System.out.println("Hello World!"); // Display the string.
&#160;&#160;&#160;&#160;}
}

```

**Be Careful When You Type**
<img src="../../figures/getStarted/typeA.gif" width="51" height="36" alt="uppercase letter A" />&#160;&#160;
<img src="../../figures/getStarted/typea2.gif" width="51" height="36" alt="lowercase letter A" />

Save the code in a file with the name `HelloWorldApp.java`. To do this in Notepad, first choose the ** File &gt; Save As** menu item. Then, in the ** Save As** dialog box:

1. Using the ** Save in** combo box, specify the folder (directory) where you'll save your file. In this example, the directory is `myapplication` on the `C` drive.
1. In the ** File name** text field, type `"HelloWorldApp.java"`, including the quotation marks.
1. From the ** Save as type** combo box, choose ** Text Documents (*.txt)**.
1. In the ** Encoding** combo box, leave the encoding as ANSI.

When you're finished, the dialog box should look like 
this.

The Save As dialog just before you click ** Save**.

Now click ** Save**, and exit Notepad.

### <a name="win32-2b" id="win32-2b"></a>Compile the Source File into a .class File

Bring up a shell, or "command," window. You can do this from the ** Start** menu by choosing ** Run...** and then entering `cmd`. The shell window should look similar to 
the following figure.

A shell window.

The prompt shows your *current directory*. When you bring up the prompt, your current directory is usually your home directory for Windows XP (as shown in the preceding figure.

To compile your source file, change your current directory to the directory where your file is located. For example, if your source directory is `myapplication` on the `C` drive, type the following command at the prompt and press ** Enter**:

```

cd C:\myapplication

```

Now the prompt should change to `C:\myapplication&gt;`.

To change to a directory on a different drive, you must type an extra command: the name of the drive. For example, to change to the `myapplication` directory on the `D` drive, you must enter `D:`, as follows:

```

javac HelloWorldApp.java

```

The compiler has generated a bytecode file, `HelloWorldApp.class`. At the prompt, type `dir` to see the new file that was generated as follows:

```

java -cp . HelloWorldApp

```

You should see the following on your screen:

Congratulations! Your program works!

If you encounter problems with the instructions in this step, consult the 
[Common Problems (and Their Solutions)](../problems/index.html).
