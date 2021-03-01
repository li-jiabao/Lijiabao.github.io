
# Installed Extensions

Installed extensions are JAR files in the <tt>lib/ext</tt> directory of the Java Runtime Environment (JRE&#8482;) software. As its name implies, the JRE is the runtime portion of the Java Development Kit containing the platform's core API but without development tools such as compilers and debuggers. The JRE is available either by itself or as part of the Java Development Kit.

The JRE is a strict subset of the JDK software. A subset of the JDK software directory tree looks like this:

The JRE consists of those directories within the highlighted box in the diagram. Whether your JRE is stand-alone or part of the JDK software, any JAR file in the <tt>lib/ext</tt> of the JRE directory is automatically treated by the runtime environment as an extension.

Since installed extensions extend the platform's core API, use them judiciously. They are rarely appropriate for interfaces used by a single, or small set of applications.

Furthermore, since the symbols defined by installed extensions will be visible in all Java processes, care should be taken to ensure that all visible symbols follow the appropriate "reverse domain name" and "class hierarchy" conventions. For example, <tt>com.mycompany.MyClass</tt>.

As of Java 6, extension JAR files may also be placed in a location that is independent of any particular JRE, so that extensions can be shared by all JREs that are installed on a system. Prior to Java 6, the value of <tt>java.ext.dirs</tt> referred to a single directory, but as of Java 6 it is a list of directories (like <tt>CLASSPATH</tt>) that specifies the locations in which extensions are searched for. The first element of the path is always the <tt>lib/ext</tt> directory of the JRE. The second element is a directory outside of the JRE. This other location allows extension JAR files to be installed once and used by several JREs installed on that system. The location varies depending on the operating system:

- Solaris&#8482; Operating System: <tt>/usr/jdk/packages/lib/ext</tt>
- Linux: <tt>/usr/java/packages/lib/ext</tt>
- Microsoft Windows: <tt>%SystemRoot%\Sun\Java\lib\ext</tt>

Note that an installed extension placed in one of the above directories extends the platform of **each** of the JREs (Java 6 or later) on that system.

## A Simple Example

Let's create a simple installed extension. Our extension consists of one class, <tt>RectangleArea</tt>, that computes the areas of rectangles:

```

public final class RectangleArea {
    public static int area(java.awt.Rectangle r) {
        return r.width * r.height;
    }
}

```

This class has a single method, <tt>area</tt>, that takes an instance of <tt>java.awt.Rectangle</tt> and returns the rectangle's area.

Suppose that you want to test <tt>RectangleArea</tt> with an application called `AreaApp`:

```

import java.awt.*;

public class AreaApp {
    public static void main(String[] args) {
        int width = 10;
        int height = 5;

        Rectangle r = new Rectangle(width, height);
        System.out.println("The rectangle's area is " 
                           + RectangleArea.area(r));
    }
}

```

This application instantiates a 10 <var>x</var> 5 rectangle, and then prints out the rectangle's area using the <tt>RectangleArea.area</tt> method.

## Running AreaApp Without the Extension Mechanism

Let's first review how you would run the `AreaApp` application without using the extension mechanism. We'll assume that the <tt>RectangleArea</tt> class is bundled in a JAR file named <tt>area.jar</tt>.

The <tt>RectangleArea</tt> class is not part of the Java platform, of course, so you would need to place the <tt>area.jar</tt> file on the class path in order to run `AreaApp` without getting a runtime exception. If <tt>area.jar</tt> was in the directory <tt>/home/user</tt>, for example, you could use this command:

```

java -classpath .:/home/user/area.jar AreaApp 

```

The class path specified in this command contains both the current directory, containing <tt>AreaApp.class</tt>, and the path to the JAR file containing the <tt>RectangleArea</tt> package. You would get the desired output by running this command:

```

The rectangle's area is 50

```

## Running AreaApp by Using the Extension Mechanism

Now let's look at how you would run `AreaApp` by using the <tt>RectangleArea</tt> class as an extension.

To make the <tt>RectangleArea</tt> class into an extension, you place the file <tt>area.jar</tt> in the <tt>lib/ext</tt> directory of the JRE. Doing so automatically gives the <tt>RectangleArea</tt> the status of being an installed extension.

With <tt>area.jar</tt> installed as an extension, you can run `AreaApp` without needing to specify the class path:

```

java AreaApp 

```

Because you're using <tt>area.jar</tt> as an installed extension, the runtime environment will be able to find and to load the `RectangleArea` class even though you haven't specified it on the class path. Similarly, any applet or application being run by any user on your system would be able to find and use the <tt>RectangleArea</tt> class.

If there are multiple JREs (Java 6 or later) installed on a system and want the <tt>RectangleArea</tt> class to be available as an extension to all of them, instead of installing it in the <tt>lib/ext</tt> directory of a particular JRE, install it in the system-wide location. For example, on system running Linux, install <tt>area.jar</tt> in the directory <tt>/usr/java/packages/lib/ext</tt>. Then <tt>AreaApp</tt> can run using different JREs that are installed on that system, for example if different browsers are configured to use different JREs.
