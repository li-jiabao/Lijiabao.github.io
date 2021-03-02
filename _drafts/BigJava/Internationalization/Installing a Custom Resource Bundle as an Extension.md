
# Installing a Custom Resource Bundle as an Extension

The section  
[Customizing Resource Bundle Loading](../../i18n/resbundle/control.html) shows you how to change how resource bundles are loaded. This involves deriving a new class from the class
[`ResourceBundle.Control`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.Control.html), then retrieving the resource bundle by invoking the following method:

```

ResourceBundle getBundle(
  String baseName,
  Locale targetLocale,
  ResourceBundle.Control control)

```

The parameter `control` is your implementation of `ResourceBundle.Control`.

The 
[`java.util.spi.ResourceBundleControlProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/ResourceBundleControlProvider.html) interface enables you to change how the following method loads resource bundles:

```

ResourceBundle getBundle(
  String baseName,
  Locale targetLocale)

```

Note that this version of the
[`ResourceBundle.getBundle`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html#getBundle-java.lang.String-java.util.Locale-) method does not require an instance of the `ResourceBundle.Control` class. `ResourceBundleControlProvider` is a service provider interface (SPI). SPIs enable you to create extensible applications, which are those that you can extend easily without modifying their original code base. See 
[Creating Extensible Applications](../../ext/basics/spi.html) for more information.

To use SPIs, you first create a service provider by implementing an SPI like `ResourceBundleControlProvider`. When you implement an SPI, you specify how it will provide the service. The service that the `ResourceBundleControlProvider` SPI provides is to obtain an appropriate `ResourceBundle.Control` instance when your application invokes the method `ResourceBundle.getBundle(String baseName, Locale targetLocale)`. You package the service provider with the
[Java Extension Mechanism](../../ext/index.html) as an installed extension. When you run your application, you do not name your extensions in your class path; the runtime environment finds and loads these extensions.

An installed implementation of the `ResourceBundleControlProvider` SPI replaces the default `ResourceBundle.Control` class (which defines the default bundle loading process). Consequently, the `ResourceBundleControlProvider` interface enables you to use any of the custom `ResourceBundle.Control` classes without any additional changes to the source code of your application. In addition, this interface enables you to write applications without having to refer to any of your custom `ResourceBundle.Control` classes.

The
[`<code>RBCPTest.java`</code>](examples/rbcpsample/src/RBCPTest.java) sample shows how to implement the `ResourceBundleControlProvider` interface and how to package it as an installed extension. This sample, which is packaged in the zip file `[RBCPTest.zip](examples/rbcpsample/RBCPTest.zip)`, consists of the following files:

  <li>`src`
<ul>
  <li>
[`<code>java.util.spi.ResourceBundleControlProvider`</code>](examples/rbcpsample/src/java.util.spi.ResourceBundleControlProvider)  </li>
  <li>
[`<code>RBCPTest.java`</code>](examples/rbcpsample/src/RBCPTest.java)</li>
  <li>`rbcp`
    <ul>
      <li>
[`<code>PropertiesResourceBundleControl.java`</code>](examples/rbcpsample/src/rbcp/PropertiesResourceBundleControl.java)      </li>
      <li>
[`<code>PropertiesResourceBundleControlProvider.java`</code>](examples/rbcpsample/src/rbcp/PropertiesResourceBundleControlProvider.java)      </li>
      <li>
[`<code>XMLResourceBundleControl.java`</code>](examples/rbcpsample/src/rbcp/XMLResourceBundleControl.java)      </li>          
      <li>
[`<code>XMLResourceBundleControlProvider.java`</code>](examples/rbcpsample/src/rbcp/XMLResourceBundleControlProvider.java)      </li>    
    
      <li>
[`<code>RBControl.properties`</code>](examples/rbcpsample/src/resources/RBControl.properties)      </li>
      <li>
[`<code>RBControl_zh.properties`</code>](examples/rbcpsample/src/resources/RBControl_zh.properties)      </li>
      <li>
[`<code>RBControl_zh_CN.properties`</code>](examples/rbcpsample/src/resources/RBControl_zh_CN.properties)      </li>
      <li>
[`<code>RBControl_zh_HK.properties`</code>](examples/rbcpsample/src/resources/RBControl_zh_HK.properties)      </li>
      <li>
[`<code>RBControl_zh_TW.properties`</code>](examples/rbcpsample/src/resources/RBControl_zh_TW.properties)      </li>
      <li>
[`<code>XmlRB.xml`</code>](examples/rbcpsample/src/resources/XmlRB.xml)      </li>
      <li>
[`<code>XmlRB_ja.xml`</code>](examples/rbcpsample/src/resources/XmlRB_ja.xml)      </li>
    
      - [`rbcontrolprovider.jar`](#package-provider)
    
The following steps show you how to re-create the contents of the file `RBCPTest.zip`, how the `RBCPTest` sample works, and how to run it:

  1. [Create implementations of the ResourceBundle.Control class.](#create-implementations-of-resourcebundle.control)
  1. [Implement the ResourceBundleControlProvider interface.](#implement-resourcebundlecontrolprovider)
  1. [In your application, invoke the method ResourceBundle.getBundle.](#invoke-resourcebundle.getbundle)
  1. [Register the service provider by creating a configuration file.](#register-service-provider)
  1. [Package the provider, its required classes, and the configuration file in a JAR file.](#package-provider)
  1. [Run the RBCPTest program.](#run-rbcptest)

## <a name="create-implementations-of-resourcebundle.control" id="create-implementations-of-resourcebundle.control">1. Create implementations of the ResourceBundle.Control class.</a>

The
[`<code>RBCPTest.java`</code>](examples/rbcpsample/src/RBCPTest.java) sample uses two implementations of `ResourseBundle.Control`:

<li><p>
[`<code>PropertiesResourceBundleControlProvider.java`</code>](examples/rbcpsample/src/rbcp/PropertiesResourceBundleControlProvider.java): This is the same `ResourceBundle.Control` implementation that is defined in
[Customizing Resource Bundle Loading](../resbundle/control.html).</p></li>

<li><p>
[`<code>XMLResourceBundleControl.java`</code>](examples/rbcpsample/src/rbcp/XMLResourceBundleControl.java): This `ResourceBundle.Control` implementation loads XML-based bundles with the method
[`Properties.loadFromXML`](https://docs.oracle.com/javase/8/docs/api/java/util/Properties.html#loadFromXML-java.io.InputStream-).</p>
</li>

### XML Properties Files

As described in the section
[Backing a ResourceBundle with Properties Files](../resbundle/propfile.html), properties files are simple text files. They contain one key-value pair on each line. XML properties files are just like properties files: they contain key-value pairs except they have an XML structure. The following is the XML properties file `XmlRB.xml`:

```
Properties</code></a> class for more information about XML properties files.</p>
 
<!-- ********************************************************************* -->

<h2><a name="implement-resourcebundlecontrolprovider" id="implement-resourcebundlecontrolprovider">2. Implement the ResourceBundleControlProvider interface.</a></h2>

<p>This interface contains one method, the
[`ResourceBundle.Control getControl(String baseName)`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/ResourceBundleControlProvider.html#getControl-java.lang.String-) method. The parameter `baseName` is the name of the resource bundle. In the method definition of `getBundle`, specify the instance of `ResourceBundle.Control` that should be returned given the name of the resource bundle.</p>

<p>The `RBCPTest` sample contains two implementations of the `ResourceBundleControlProvider` interface,
[`<code>PropertiesResourceBundleControlProvider.java`</code>](examples/rbcpsample/src/rbcp/PropertiesResourceBundleControlProvider.java) and
[`<code>XMLResourceBundleControlProvider.java`</code>](examples/rbcpsample/src/rbcp/XMLResourceBundleControlProvider.java). The method `PropertiesResourceBundleControlProvider.getBundle` returns an instance of `PropertiesResourceBundleControl` if the base name of the resource bundle starts with `resources.RBControl` (in this example, all the resource files are contained in the package `resources`):</p>

<pre class="codeblock">


package rbcp;

import java.util.ResourceBundle;
import java.util.spi.ResourceBundleControlProvider;

public class PropertiesResourceBundleControlProvider
    implements ResourceBundleControlProvider {
    static final ResourceBundle.Control PROPERTIESCONTROL =
        new PropertiesResourceBundleControl();

    public ResourceBundle.Control getControl(String baseName) {
        System.out.println("Class: " + getClass().getName() + ".getControl");
        System.out.println("    called for " + baseName);

        // Throws a NPE if baseName is null.
        if (baseName.startsWith("resources.RBControl")) {
            System.out.println("    returns " + PROPERTIESCONTROL);
            return PROPERTIESCONTROL;
        }
        System.out.println("    returns null");
        System.out.println();
        return null;
    }
}

```

Similarly, the method `XMLResourceBundleControlProvider.getControl` returns an instance of `XMLResourceBundleControl` if the base name of the resource bundle starts with `resources.Xml`.

**Note**: You can create one implementation of the `ResourceBundleControlProvider` interface that returns either an instance of `PropertiesResourceBundleControl` or `XMLResourceBundleControl` depending on the base name.

## <a name="invoke-resourcebundle.getbundle" id="invoke-resourcebundle.getbundle">3. In your application, invoke the method ResourceBundle.getBundle.</a>

The class `RBCPTest` retrieves resource bundles with the method
[`ResourceBundle.getBundle`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html#getBundle-java.lang.String-java.util.Locale-):

```
ServiceLoader</code></a>class. Using this class means that you have to register the service provider with a configuration file, which is described in the next step.</p>

<!-- ********************************************************************* -->

<h2><a name="register-service-provider" id="register-service-provider">4. Register the service provider by creating a configuration file.</a></h2>

<p>The name of the configuration file is the fully qualified name of the interface or class that the provider implemented. The configuration file contains the fully qualified class name of your provider. The file 
[`<code>java.util.spi.ResourceBundleControlProvider`</code>](examples/rbcpsample/src/java.util.spi.ResourceBundleControlProvider) contains the fully qualified names of `PropertiesResourceBundleControlProvider` and `XMLResourceBundleControlProvider`, one name per line:</p>

<pre class="codeblock">
rbcp.XMLResourceBundleControlProvider
rbcp.PropertiesResourceBundleControlProvider

```

## <a name="package-provider" id="package-provider">5. Package the provider, its required classes, and the configuration file in a JAR file.</a>

Compile the source files. From the directory that contains the file `build.xml`, run the following command:

```
META-INF</code>
    <ul>
      <li>`services`
        <ul>
          - `java.util.spi.ResourceBundleControlProvider`
        </ul>
      </li>
    </ul>
  </li>
  <li>`rbcp`
    <ul>
      - `PropertiesResourceBundleControl.class`
      - `PropertiesResourceBundleControlProvider.class`
      - `XMLResourceBundleControl.class`
      - `XMLResourceBundleControlProvider.class`
    </ul>
  </li>
  <li>`resources`
    <ul>
      - `RBControl.properties`
      - `RBControl_zh.properties`
      - `RBControl_zh_CN.properties`
      - `RBControl_zh_HK.properties`
      - `RBControl_zh_TW.properties`
      - `XmlRB.xml`
      - `XmlRB_ja.xml`                  
    </ul>
  </li>
</ul>

Note that the configuration file `java.util.spi.ResourceBundleControlProvider` must be packaged in the directory `/META-INF/services`. This sample packages these files in the JAR file `rbcontrolprovider.jar` in the `lib` directory.

<p>See
[Packaging Programs in JAR Files](../../deployment/jar/index.html) for more information about creating JAR files.</p>  

Alternatively, download and install [Apache Ant](http://ant.apache.org/), which is a tool that enables you to automate build processes, such as compiling Java files and creating JAR files. Ensure that the Apache Ant executable file is in your `PATH` environment variable so that you can run it from any directory. Once you have installed Apache Ant, follow these steps:

<ol>
  <li><p>Edit the file
[`<code>build.xml`</code>](examples/rbcpsample/build.xml) and change `${JAVAC}` to the full path name of your Java compiler, `javac`, and `${JAVA}` to the full path name of your Java runtime executable, `java`.</p></li>
  <li>Run the following command from the same directory that contains the file `build.xml`:
  <pre class="codeblock">ant jar
```

      - `PropertiesResourceBundleControl.class`
      - `PropertiesResourceBundleControlProvider.class`
      - `XMLResourceBundleControl.class`
      - `XMLResourceBundleControlProvider.class`
    
  <li><p>Edit the file
[`<code>build.xml`</code>](examples/rbcpsample/build.xml) and change `${JAVAC}` to the full path name of your Java compiler, `javac`, and `${JAVA}` to the full path name of your Java runtime executable, `java`.</p></li>
  <li>Run the following command from the same directory that contains the file `build.xml`:
  <pre class="codeblock">ant jar</code></pre>
  This command compiles the Java source files and packages them, along with the required resource and configuration files, into the JAR file `rbcontrolprovider.jar` in the `lib` directory.</li>

## <a name="run-rbcptest" id="run-rbcptest">6. Run the RBCPTest program.</a>

At a command prompt, run the following command from the directory that contains the `build.xml` file:

This command assumes the following:

  - The JAR file that contains the compiled code of the RBCPTest sample is in the directory `lib`.
  - The compiled class, `RBCPTest.class`, is in the `build` directory.

Alternatively, use Apache Ant and run the following command from the directory that contains the `build.xml` file:

When you install a Java extension, you typically put the JAR file of the extension in the `lib/ext` directory of your JRE. However, this command specifies the directory that contains Java extensions with the system property `java.ext.dirs`.

The `RBCPTest` program first attempts to retrieve resource bundles with the base name `resources.XmlRB` and the locales `Locale.ROOT` and `Local.JAPAN`. The output of the program retrieving these resource bundles is similar to the following:

The program successfully obtains an instance of `XMLResourceBundleControl` and accesses the properties files `XmlRB.xml` and `XmlRB_ja.xml`.

When the `RBCPTest` program tries to retrieve a resource bundle, it calls all the classes defined in the configuration file `java.util.spi.ResourceBundleControlProvider`. For example, when the program retrieves the resource bundle with the base name `resources.RBControl` and the locale `Locale.CHINA`, it prints the following output:
