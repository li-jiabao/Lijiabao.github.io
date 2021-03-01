
# "Hello World!" for Solaris OS and Linux

<a name="unix-top" id="unix-top"></a>

It's time to write your first application! These detailed instructions are for users of Solaris OS and Linux. Instructions for other platforms are in 
["Hello World!" for Microsoft Windows](win32.html) and 
["Hello World!" for the NetBeans IDE](netbeans.html).

If you encounter problems with the instructions on this page, consult the 
[Common Problems (and Their Solutions)](../problems/index.html). 



- [A Checklist](#unix-1)
<li>[Creating Your First Application](#unix-2)
    <ul>
      - [Create a Source File](#unix-2a)
      - [Compile the Source File into a `.class` File](#unix-2b)
      - [Run the Program](#unix-2c)
    
## <a name="unix-1" id="unix-1"></a> A Checklist&#160; 
<img src="../../figures/getStarted/check.gif" width="56" height="68" alt="a checkmark" />

To write your first program, you'll need:

<li>
The Java SE Development Kit 8 (JDK 8)
<p>You can 
[download the Solaris OS or Linux version now](http://www.oracle.com/technetwork/java/javase/downloads/index.html). (Make sure you download the **JDK**, *not* the JRE.) Consult the 
[installation instructions](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html).</p>
</li>
<li>
A text editor
In this example, we'll use Pico, an editor available for many UNIX-based platforms. You can easily adapt these instructions if you use a different text editor, such as `vi` or `emacs`.
</li>

These two items are all you'll need to write your first application.


          

## <a name="unix-2" id="unix-2"></a> Creating Your First Application

Your first application, `HelloWorldApp`, will simply display the greeting "Hello world!". To create this program, you will:

<li>
Create a source file
A source file contains code, written in the Java programming language, that you and other programmers can understand. You can use any text editor to create and edit source files.
</li>
<li>
Compile the source file into a .class file
The Java programming language *compiler* (`javac`) takes your source file and translates its text into instructions that the Java virtual machine can understand. The instructions contained within this `.class` file are known as **bytecodes**.
</li>
<li>
Run the program
The Java application *launcher tool* (`java`) uses the Java virtual machine to run your application.
</li>

### <a name="unix-2a" id="unix-2a"></a> Create a Source File

To create a source file, you have two options:

<li>
<p>You can save the file <code>
[`HelloWorldApp.java`](../application/examples/HelloWorldApp.java)</code> on your computer and avoid a lot of typing. Then, you can go straight to [Compile the Source File](#unix-2b).</p>
</li>
<li>
Or, you can use the following (longer) instructions.
</li>

First, open a shell, or "terminal," window.

A new terminal window.

When you first bring up the prompt, your *current directory* will usually be your *home directory*. You can change your current directory to your home directory at any time by typing `cd` at the prompt and then pressing ** Return**.

The source files you create should be kept in a separate directory. You can create a directory by using the command `mkdir`. For example, to create the directory `examples/java` in the <tt>/tmp</tt> directory, use the following commands:

```

cd /tmp
mkdir examples
cd examples
mkdir java

```

To change your current directory to this new directory, you then enter:

```

cd /tmp/examples/java

```

Now you can start creating your source file.

Start the Pico editor by typing `pico` at the prompt and pressing ** Return**. If the system responds with the message `pico: command not found`, then Pico is most likely unavailable. Consult your system administrator for more information, or use another editor.

When you start Pico, it'll display a new, blank *buffer*. This is the area in which you will type your code. 


Type the following code into the new buffer:

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

Save the code in a file with the name `HelloWorldApp.java`. In the Pico editor, you do this by typing ** Ctrl-O** and then, at the bottom where you see the prompt `File Name to write:`, entering the directory in which you wish to create the file, followed by `HelloWorldApp.java`. For example, if you wish to save `HelloWorldApp.java` in the directory `/tmp/examples/java`, then you type `/tmp/examples/java/HelloWorldApp.java` and press ** Return**.

You can type ** Ctrl-X** to exit Pico.

### <a name="unix-2b" id="unix-2b"></a>Compile the Source File into a `.class` File

Bring up another shell window. To compile your source file, change your current directory to the directory where your file is located. For example, if your source directory is `/tmp/examples/java`, type the following command at the prompt and press ** Return**:

```

cd /tmp/examples/java

```

If you enter `pwd` at the prompt, you should see the current directory, which in this example has been changed to `/tmp/examples/java`.

If you enter `ls` at the prompt, you should see your file.

Results of the `ls` command, showing the `.java` source file.

Now are ready to compile the source file. At the prompt, type the following command and press ** Return**.

```

javac HelloWorldApp.java

```

The compiler has generated a bytecode file, `HelloWorldApp.class`. At the prompt, type `ls` to see the new file that was generated: 
the following figure.

Results of the `ls` command, showing the generated `.class` file.

Now that you have a `.class` file, you can run your program.

If you encounter problems with the instructions in this step, consult the 
[Common Problems (and Their Solutions)](../problems/index.html).

### <a name="unix-2c" id="unix-2c"></a> Run the Program

In the same directory, enter at the prompt:

```

java HelloWorldApp

```


The next figure shows what you should now see.

The output prints "Hello World!" to the screen.

Congratulations! Your program works!

If you encounter problems with the instructions in this step, consult the 
[Common Problems (and Their Solutions)](../problems/index.html).
