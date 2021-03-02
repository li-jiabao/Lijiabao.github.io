
# "Hello World!" for the NetBeans IDE

<a name="netbeans-top" id="netbeans-top"></a>

It's time to write your first application! These detailed instructions are for users of the NetBeans IDE. The NetBeans IDE runs on the Java platform, which means that you can use it with any operating system for which there is a JDK available. These operating systems include Microsoft Windows, Solaris OS, Linux, and Mac OS X.





  - [A Checklist](#netbeans-1)
  <li>[Creating Your First Application](#netbeans-2)
    <ul>
      - [Create an IDE Project](#netbeans-2a)
      - [Add JDK 8 to the Platform List (if necessary)](#netbeans-jdk)
      - [Add Code to the Generated Source File](#netbeans-2b)
      - [Compile the Source File](#netbeans-2c)
      - [Run the Program](#netbeans-2d)
    
## <a name="netbeans-1" id="netbeans-1"></a> A Checklist&#160; 
<img src="../../figures/getStarted/check.gif" width="56" height="68" alt="a checkmark" />

To write your first program, you'll need:

<li>
The Java SE Development Kit (JDK 7 has been selected in this example)
<ul>
<li>For Microsoft Windows, Solaris OS, and Linux: 
[Java SE Downloads Index](http://www.oracle.com/technetwork/java/javase/downloads/index.html) page</li>
<li>For Mac OS X: 
[developer.apple.com](https://developer.apple.com/)</li>
</ul>
</li>
<li>
The NetBeans IDE
<ul>
<li>For all platforms: 
[NetBeans IDE Downloads Index ](http://netbeans.org/downloads/index.html)page</li>
</ul>
</li>

<li>For all platforms: 
[NetBeans IDE Downloads Index ](http://netbeans.org/downloads/index.html)page</li>


          

## <a name="netbeans-2" id="netbeans-2"></a>Creating Your First Application

Your first application, `HelloWorldApp`, will simply display the greeting "Hello World!" To create this program, you will:

<li>
Create an IDE project
When you create an IDE project, you create an environment in which to build and run your applications. Using IDE projects eliminates configuration issues normally associated with developing on the command line. You can build or run your application by choosing a single menu item within the IDE.
</li>
<li>
Add code to the generated source file
A source file contains code, written in the Java programming language, that you and other programmers can understand. As part of creating an IDE project, a skeleton source file will be automatically generated. You will then modify the source file to add the "Hello World!" message.
</li>
<li>
Compile the source file into a .class file
The IDE invokes the Java programming language *compiler* `(javac)`, which takes your source file and translates its text into instructions that the Java virtual machine can understand. The instructions contained within this file are known as *bytecodes*.
</li>
<li>
Run the program
The IDE invokes the Java application *launcher tool* (`java`), which uses the Java virtual machine to run your application.
</li>

### <a name="netbeans-2a" id="netbeans-2a"></a>Create an IDE Project

To create an IDE project:

<li>
Launch the NetBeans IDE.
<ul>
<li>
On Microsoft Windows systems, you can use the NetBeans IDE item in the Start menu.
</li>
<li>
On Solaris OS and Linux systems, you execute the IDE launcher script by navigating to the IDE's `bin` directory and typing `./netbeans.`
</li>
<li>
On Mac OS X systems, click the NetBeans IDE application icon.
</li>
</ul>
</li>
<li>
In the NetBeans IDE, choose **File** | **New Project...**.
<center><img src="../../figures/getStarted/nb-javatutorial-newprojectmenu.png" width="794" height="292" align="bottom" alt="NetBeans IDE with the File | New Project menu item selected." />NetBeans IDE with the File | New Project menu item selected.</center></li>
<li>
In the **New Project** wizard, expand the **Java** category and select **Java Application** as shown in the following figure:
<center><img src="../../figures/getStarted/nb-javatutorial-project1.png" width="729" height="511" align="bottom" alt="NetBeans IDE, New Project wizard, Choose Project page." />NetBeans IDE, New Project wizard, Choose Project page.</center></li>
<li>
In the **Name and Location** page of the wizard, do the following (as shown in the figure below):
<ul>
<li>
In the **Project Name** field, type `Hello World App`.
</li>
<li>
In the **Create Main Class** field, type `helloworldapp.HelloWorldApp`.
</li>
</ul>
<center><img src="../../figures/getStarted/nb-javatutorial-project2.png" width="765" height="511" align="bottom" alt="NetBeans IDE, New Project wizard, Name and Location page." />NetBeans IDE, New Project wizard, Name and Location page.</center></li>
<li>
Click Finish.
</li>

<li>
In the **Project Name** field, type `Hello World App`.
</li>
<li>
In the **Create Main Class** field, type `helloworldapp.HelloWorldApp`.
</li>

The project is created and opened in the IDE. You should see the following components:

<li>
The **Projects** window, which contains a tree view of the components of the project, including source files, libraries that your code depends on, and so on.
</li>
<li>
The **Source Editor** window with a file called `HelloWorldApp.java` open.
</li>
<li>
The **Navigator** window, which you can use to quickly navigate between elements within the selected class.
<center><img src="../../figures/getStarted/nb-javatutorial-project-opened.png" width="810" height="631" align="bottom" alt="NetBeans IDE with the HelloWorldApp project open." />NetBeans IDE with the HelloWorldApp project open.</center></li>




### <a name="netbeans-jdk" id="netbeans-jdk"></a> Add JDK 8 to the Platform List (if necessary)

It may be necessary to add JDK 8 to the IDE's list of available platforms. To do this, choose Tools | Java Platforms as shown in the following figure:

Selecting the Java Platform Manager from the Tools Menu

If you don't see JDK 8 (which might appear as 1.8 or 1.8.0) in the list of installed platforms, click **Add Platform**, navigate to your JDK 8 install directory, and click **Finish**. You should now see this newly added platform:

The Java Platform Manager

To set this JDK as the default for all projects, you can run the IDE with the `--jdkhome` switch on the command line, or by entering the path to the JDK in the `netbeans_j2sdkhome` property of your `INSTALLATION_DIRECTORY/etc/netbeans.conf` file.

To specify this JDK for the current project only, select **Hello World App** in the **Projects** pane, choose **File** | **Project Properties (Hello World App)**, click **Libraries**, then select **JDK 1.8** in the **Java Platform** pulldown menu. You should see a screen similar to the following:



The IDE is now configured for JDK 8.

### <a name="netbeans-2b" id="netbeans-2b"></a>Add Code to the Generated Source File

When you created this project, you left the **Create Main Class** checkbox selected in the **New Project** wizard. The IDE has therefore created a skeleton class for you. You can add the "Hello World!" message to the skeleton code by replacing the line:

```

// TODO code application logic here

```

with the line:

```

System.out.println("Hello World!"); // Display the string.

```

Optionally, you can replace these four lines of generated code:

```

/**
 *
 * @author
 */

```

with these lines:

```

/**
 * The HelloWorldApp class implements an application that
 * simply prints "Hello World!" to standard output.
 */

```

These four lines are a code comment and do not affect how the program runs. 
Later sections of this tutorial explain the use and format of code comments. 


**Be Careful When You Type**
<img src="../../figures/getStarted/typeA.gif" width="51" height="36" alt="uppercase letter A" />&#160;&#160;
<img src="../../figures/getStarted/typea2.gif" width="51" height="36" alt="lowercase letter A" />

Save your changes by choosing **File** | **Save**.

The file should look something like the following:

```

/*
 * To change this template, choose Tools | Templates
 * and open the template in the editor.
 */

package helloworldapp;

/**
 * The HelloWorldApp class implements an application that
 * simply prints "Hello World!" to standard output.
 */
public class HelloWorldApp {

   
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }

}

```

### <a name="netbeans-2c" id="netbeans-2c"></a>Compile the Source File into a .class File

To compile your source file, choose **Run** | **Build Project (Hello World App)** from the IDE's main menu.

The Output window opens and displays output similar to what you see in the following figure:

Output window showing results of building the HelloWorld project.

If the build output concludes with the statement `BUILD SUCCESSFUL`, congratulations! You have successfully compiled your program!

If the build output concludes with the statement `BUILD FAILED`, you probably have a syntax error in your code. Errors are reported in the Output window as hyperlinked text. You double-click such a hyperlink to navigate to the source of an error. You can then fix the error and once again choose **Run** | **Build Project**.

When you build the project, the bytecode file `HelloWorldApp.class` is generated. You can see where the new file is generated by opening the **Files** window and expanding the **Hello&#160;World&#160;App/build/classes/helloworldapp** node as shown in the following figure.

Files window, showing the generated .class file.

Now that you have built the project, you can run your program.

### <a name="netbeans-2d" id="netbeans-2d"></a> Run the Program

From the IDE's menu bar, choose **Run** | **Run Main Project**.

The next figure shows what you should now see.

The program prints "Hello World!" to the Output window (along with other output from the build script).

Congratulations! Your program works!

## <a name="netbeans-3" id="netbeans-3"></a>Continuing the Tutorial with the NetBeans IDE

The next few pages of the tutorial will explain the code in this simple application. After that, the lessons go deeper into core language features and provide many more examples. Although the rest of the tutorial does not give specific instructions about using the NetBeans IDE, you can easily use the IDE to write and run the sample code. The following are some tips on using the IDE and explanations of some IDE behavior that you are likely to see:

<li>
Once you have created a project in the IDE, you can add files to the project using the **New File** wizard. Choose **File** | **New File**, and then select a template in the wizard, such as the Empty Java File template.
</li>
<li>
You can compile and run an individual file (as opposed to a whole project) using the IDE's **Compile File** (F9) and **Run File** (Shift-F6) commands. If you use the **Run Main Project** command, the IDE will run the file that the IDE associates as the main class of the main project. Therefore, if you create an additional class in your HelloWorldApp project and then try to run that file with the **Run Main Project** command, the IDE will run the `HelloWorldApp` file instead.
</li>
<li>
You might want to create separate IDE projects for sample applications that include more than one source file.
</li>
<li>
As you are typing in the IDE, a code completion box might periodically appear. You can either ignore the code completion box and keep typing, or you can select one of the suggested expressions. If you would prefer not to have the code completion box automatically appear, you can turn off the feature. Choose **Tools** | **Options** | **Editor**, click the **Code Completion** tab and clear the **Auto Popup Completion Window** checkbox.
</li>
<li>
If you want to rename the node for a source file in the **Projects** window, choose **Refactor** from IDE's main menu. The IDE prompts you with the **Rename** dialog box to lead you through the options of renaming the class and the updating of code that refers to that class. Make the changes and click **Refactor** to apply the changes. This sequence of clicks might seem unnecessary if you have just a single class in your project, but it is very useful when your changes affect other parts of your code in larger projects.
</li>
<li>
<p>For a more thorough guide to the features of the NetBeans IDE, see the 
[NetBeans Documentation](https://netbeans.org/kb/) page.</p>
</li>
