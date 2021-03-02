
# Download Extensions

Download extensions are sets of classes (and related resources) in JAR files. A JAR file's manifest can contain headers that refer to one or more download extensions. The extensions can be referenced in one of two ways:

- by a <tt>Class-Path</tt> header
- by an <tt>Extension-List</tt> header

Note that at most one of each is allowed in a manifest. Download extensions indicated by a <tt>Class-Path</tt> header are downloaded only for the lifetime of the application that downloads them, such as a web browser. Their advantage is that nothing is installed on the client; their disadvantage is that they are downloaded each time they are needed. Download extensions that are downloaded by an <tt>Extension-List</tt> header are installed into the <tt>/lib/ext</tt> directory of the JRE that downloads them. Their advantage is that they are downloaded the first time they're needed; subsequently they can be used without downloading. But, as shown later in this tutorial, they are more complex to deploy.

Since download extensions that use the <tt>Class-Path</tt> headers are simpler, let's consider them first. Assume for example that <tt>a.jar</tt> and <tt>b.jar</tt> are two JAR files in the same directory, and that the manifest of <tt>a.jar</tt> contains this header:

```

Class-Path: b.jar

```

Then the classes in <tt>b.jar</tt> serve as extension classes for purposes of the classes in <tt>a.jar</tt>. The classes in <tt>a.jar</tt> can invoke classes in <tt>b.jar</tt> without <tt>b.jar</tt>'s classes having to be named on the class path. <tt>a.jar</tt> may or may not itself be an extension. If <tt>b.jar</tt> weren't in the same directory as <tt>a.jar</tt>, then the value of the <tt>Class-Path</tt> header should be set to the relative pathname of <tt>b.jar</tt>.

There's nothing special about the classes that are playing the role of a download extension. They are treated as extensions solely because they're referenced by the manifest of some other JAR file.

To get a better understanding of how download extensions work, let's create one and put it to use.

## An Example

Suppose you want to create an applet that makes use of the `RectangleArea` class of the previous section:

```

public final class RectangleArea {  
    public static int area(java.awt.Rectangle r) {
        return r.width * r.height;
    }
}

```

In the previous section, you made the <tt>RectangleArea</tt> class into an installed extension by placing the JAR file containing it into the <tt>lib/ext</tt> directory of the JRE. By making it an installed extension, you enabled any application to use the <tt>RectangleArea</tt> class as if it were part of the Java platform.

If you want to be able to use the <tt>RectangleArea</tt> class from an applet, the situation is a little different. Suppose, for example, that you have an applet, `AreaApplet`, that makes use of class <tt>RectangleArea</tt>:

```

import java.applet.Applet;
import java.awt.*;

public class AreaApplet extends Applet {
    Rectangle r;

    public void init() {    
        int width = 10;
        int height = 5;

        r = new Rectangle(width, height);
    }

    public void paint(Graphics g) {
        g.drawString("The rectangle's area is " 
                      + RectangleArea.area(r), 10, 10);
    }
}

```

This applet instantiates a 10 <var>x</var> 5 rectangle and then displays the rectangle's area by using the <tt>RectangleArea.area</tt> method.

However, you can't assume that everyone who downloads and uses your applet is going to have the <tt>RectangleArea</tt> class available on their system, as an installed extension or otherwise. One way around that problem is to make the <tt>RectangleArea</tt> class available from the server side, and you can do that by using it as a download extension.

To see how that's done, let's assume that you've bundled `AreaApplet` in a JAR file called <tt>AreaApplet.jar</tt> and that the class <tt>RectangleArea</tt> is bundled in <tt>RectangleArea.jar</tt>. In order for <tt>RectangleArea.jar</tt> to be treated as a download extension, <tt>RectangleArea.jar</tt> must be listed in the <tt>Class-Path</tt> header in <tt>AreaApplet.jar</tt>'s manifest. <tt>AreaApplet.jar</tt>'s manifest might look like this, for example:

```

Manifest-Version: 1.0
Class-Path: RectangleArea.jar

```

The value of the <tt>Class-Path</tt> header in this manifest is <tt>RectangleArea.jar</tt> with no path specified, indicating that <tt>RectangleArea.jar</tt> is located in the same directory as the applet's JAR file.

## More about the Class-Path Header

If an applet or application uses more than one extension, you can list multiple URLs in a manifest. For example, the following is a valid header:

```

Class-Path: area.jar servlet.jar images/

```

In the <tt>Class-Path</tt> header any URLs listed that don't end with '<tt>/</tt>' are assumed to be JAR files. URLs ending in '<tt>/</tt>' indicate directories. In the preceding example, <tt>images/</tt> might be a directory containing resources needed by the applet or the application.

Note that only one <tt>Class-Path</tt> header is allowed in a manifest file, and that each line in a manifest must be no more than 72 characters long. If you need to specify more class path entries than will fit on one line, you can extend them onto subsequent continuation lines. Begin each continuation line with two spaces. For example:

```

Class-Path: area.jar servlet.jar monitor.jar datasource.jar
  provider.jar gui.jar

```

A future release may remove the limitation of having only one instance of each header, and of limiting lines to only 72 characters.

Download extensions can be "daisy chained", meaning that the manifest of one download extension can have a <tt>Class-Path</tt> header that refers to a second extension, which can refer to a third extension, and so on.

## Installing Download Extensions

In the above example, the extension downloaded by the applet is available only while the browser which loaded the applet is still running. However, applets can trigger installation of extensions, if additional information is included in the manifests of both the applet and the extension.

Since this mechanism extends the platform's core API, its use should be judiciously applied. It is rarely appropriate for interfaces used by a single, or small set of applications. All visible symbols should follow reverse domain name and class hierarchy conventions.

The basic requirements are that both the applet and the extensions it uses provide version information in their manifests, and that they be signed. The version information allows Java Plug-in to ensure that the extension code has the version expected by the applet. For example, the <tt>AreaApplet</tt> could specify an <tt>areatest</tt> extension in its manifest:

```

Manifest-Version: 1.0
Extension-List: areatest
areatest-Extension-Name: area
areatest-Specification-Version: 1.1
areatest-Implementation-Version: 1.1.2
areatest-Implementation-Vendor-Id: com.example
areatest-Implementation-URL: http://www.example.com/test/area.jar

```

The manifest in <tt>area.jar</tt> would provide corresponding information:

```

Manifest-Version: 1.0
Extension-Name: area
Specification-Vendor: Example Tech, Inc
Specification-Version: 1.1
Implementation-Vendor-Id: com.example
Implementation-Vendor: Example Tech, Inc
Implementation-Version: 1.1.2

```

Both the applet and the extension must be signed, by the same signer. Signing the jar files will modify them in-place, providing more information in their manifest files. Signing helps ensure that only trusted code gets installed. A simple way to sign jar files is to first create a keystore, and then use that to hold certificates for the applet and extension. For example:

```

keytool -genkey -dname "cn=Fred" -alias test  -validity 180

```

You will be prompted for the keystore and key passwords. After generating a key, the jar files can be signed:

```

jarsigner AreaApplet.jar test
jarsigner area.jar test

```

You will be prompted for the keystore and key passwords. More information on <tt>keytool</tt>, <tt>jarsigner</tt>, and other security tools is at the 
[Summary of Tools for the Java 2 Platform Security](https://docs.oracle.com/javase/8/docs/technotes/guides/security/SecurityToolsSummary.html).

Here is <tt>AreaDemo.html</tt>, which loads the applet and causes the extension code to be downloaded and installed:

```

&lt;html&gt;
&lt;body&gt;
  &lt;applet code="AreaApplet.class" archive="AreaApplet.jar"/&gt;
&lt;/body&gt;
&lt;/html&gt;

```

When the page is loaded for the first time, the user is told that the applet requires installation of the extension. A subsequent dialog informs the user about the signed applet. Accepting both installs the extension in the <tt>lib/ext</tt> folder of the JRE and runs the applet.

After restarting the web browser and load the same web page, only the dialog about the applet's signer is presented, because <tt>area.jar</tt> is already installed. This is also true if <tt>AreaDemo.html</tt> is opened in a different web browser (assuming both browsers use the same JRE).
