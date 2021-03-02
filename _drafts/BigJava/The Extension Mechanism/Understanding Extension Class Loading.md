
# Creating Extensible Applications

The following topics are covered:

  - [Introduction](#introduction)
  - [Dictionary Service Example](#dictionary-service-example)
  - [Running the DictionaryServiceDemo Sample](#running-the-dictionaryservicedemo-sample)
  - [Compiling and Running the DictionaryServiceDemo Sample](#compiling-and-running-the-dictionaryservicedemo-sample)
  <li>[Understanding the DictionaryServiceDemo Sample](#understanding-the-dictionaryservicedemo-sample)
    <ol>
      - [Define the Service Provider Interface](#define-the-service-provider-interface)
      <li>[Define the Service That Retrieves the Service Provider Implementations](#definte-the-service-that-retrieves-service-provider-implementations)
        <ul>
          - [The Singleton Design Pattern](#singleton)
        
      1. [Define the Service Provider Interface](#define-the-service-provider-interface)
      <li>[Define the Service That Retrieves the Service Provider Implementations](#definte-the-service-that-retrieves-service-provider-implementations)
        <ul>
          1. [The Singleton Design Pattern](#singleton)
        </ul>
      </li>
      1. [Implement the Service Provider](#implement-the-service-provider)
      1. [Register Service Providers](#register-service-providers)
      1. [Create a Client That Uses the Service and Service Providers](#create-a-client-that-uses-the-service-and-service-providers)
      <li>[Package the Service Providers, the Service, and the Service Client in JAR Files](#package-in-jar-files)
        <ul>
          1. [Packaging Service Providers in JAR Files](#packaging-service-providers-in-jar-files)
          1. [Packaging the Dictionary SPI and Dictionary Service in a JAR File](#packaging-the-dictionary-service-in-a-jar-file)
          1. [Packaging the Client in a JAR File](#packaging-the-client-in-a-jar-file)
        </ul>
      </li>
      1. [Run the Client](#run-the-client)
    
## <a name="introduction" id="introduction">Introduction</a>

An *extensible* application is one that you can extend without modifying its original code base. You can enhance its functionality with new plug-ins or modules. Developers, software vendors, and customers can add new functionality or application programming interfaces (APIs) by adding a new Java Archive (JAR) file onto the application class path or into an application-specific extension directory. 

This section describes how to create applications with extensible services, which enable you or others to provide service implementations that require no modifications to the original application. By designing an extensible application, you provide a way to upgrade or enhance specific parts of a product without changing the core application. 

One example of an extensible application is a word processor that allows the end user to add a new dictionary or spelling checker. In this example, the word processor provides a dictionary or spelling feature that other developers, or even customers, can extend by providing their own implementation of the feature.

The following are terms and definitions important to understand extensible applications:

## <a name="dictionary-service-example" id="dictionary-service-example">Dictionary Service Example</a>

Consider how you might design a dictionary service in a word processor or editor. One way is to define a service represented by a class named `DictionaryService` and a service provider interface named `Dictionary`. The `DictionaryService` provides a singleton `DictionaryService` object. (See the section [The Singleton Design Pattern](#singleton) for more information.) This object retrieves definitions of words from `Dictionary` providers. Dictionary service clients &#8212; your application code &#8212; retrieve an instance of this service, and the service will search, instantiate, and use `Dictionary` service providers.

Although the word-processor developer would most likely provide a basic, general dictionary with the original product, the customer might require a specialized dictionary, perhaps containing legal or technical terms. Ideally, the customer is able to create or purchase new dictionaries and add them to the existing application.


The `[DictionaryServiceDemo](examples/DictionaryServiceDemo/DictionaryServiceDemo.zip)` sample shows you how to implement a `Dictionary` service, create `Dictionary` service providers that add additional dictionaries, and create a simple `Dictionary` service client that tests the service. This sample, which is packaged in the zip file `[DictionaryServiceDemo.zip](examples/DictionaryServiceDemo/DictionaryServiceDemo.zip)`, consists of the following files:



  <li>
[`<code>build.xml`</code>](examples/DictionaryServiceDemo/build.xml)  </li>
  
  <li>`DictionaryDemo`
    <ul>
      <li>
[`<code>build.xml`</code>](examples/DictionaryServiceDemo/DictionaryDemo/build.xml)      </li>
      - `build`
      <li>`dist`
        <ul>
          - `DictionaryDemo.jar`
        
          <li>`dictionary`
            <ul>
              <li>
[`<code>DictionaryDemo.java`</code>](examples/DictionaryServiceDemo/DictionaryDemo/src/dictionary/DictionaryDemo.java)              </li>
            
      <li>
[`<code>build.xml`</code>](examples/DictionaryServiceDemo/DictionaryDemo/build.xml)      </li>
      - `build`
      <li>`dist`
        <ul>
          - `DictionaryServiceProvider.jar`
        
          <li>`dictionary`
            <ul>
              <li>
[`<code>DictionaryService.java`</code>](examples/DictionaryServiceDemo/DictionaryServiceProvider/src/dictionary/DictionaryService.java)              </li>
              <li>`spi`
                <ul>
                  <li>
[`<code>Dictionary.java`</code>](examples/DictionaryServiceDemo/DictionaryServiceProvider/src/dictionary/spi/Dictionary.java)                  </li>
                
      <li>
[`<code>build.xml`</code>](examples/DictionaryServiceDemo/ExtendedDictionary/build.xml)      </li>
      - `build`
      <li>`dist`
        <ul>
          - `ExtendedDictionary.jar`
        
          <li>`dictionary`
            <ul>
              <li>
[`<code>ExtendedDictionary.java`</code>](examples/DictionaryServiceDemo/ExtendedDictionary/src/dictionary/ExtendedDictionary.java)              </li>
            
              <li>`services`
                <ul>
                  <li>
[`<code>dictionary.spi.Dictionary`</code>](examples/DictionaryServiceDemo/ExtendedDictionary/src/META-INF/services/dictionary.spi.Dictionary)                  </li>
                
      <li>
[`<code>build.xml`</code>](examples/DictionaryServiceDemo/GeneralDictionary/build.xml)      </li>
      - `build`
      <li>`dist`
        <ul>
          - `GeneralDictionary.jar`
        
          <li>`dictionary`
            <ul>
              <li>
[`<code>GeneralDictionary.java`</code>](examples/DictionaryServiceDemo/GeneralDictionary/src/dictionary/GeneralDictionary.java)              </li>
            
              <li>`services`
                <ul>
                  <li>
[`<code>dictionary.spi.Dictionary`</code>](examples/DictionaryServiceDemo/GeneralDictionary/src/META-INF/services/dictionary.spi.Dictionary)                  </li>
                
**Note**: The `build` directories contain the compiled class files of the Java source files contained in the `src` directory in the same level.

## <a name="running-the-dictionaryservicedemo-sample" id="running-the-dictionaryservicedemo-sample">Running the DictionaryServiceDemo Sample</a>

Because the zip file `[DictionaryServiceDemo.zip](examples/DictionaryServiceDemo/DictionaryServiceDemo.zip)` contains compiled class files, you can unzip this file to your computer and run the sample without compiling it by following these steps:

  <li>                 
    <p>
    Download and unzip the sample code: Download and unzip the file `[DictionaryServiceDemo.zip](examples/DictionaryServiceDemo/DictionaryServiceDemo.zip)` to your computer. It is These steps assume that you unzipped the contents of this file into the directory `C:\DictionaryServiceDemo`.

    </p>
  </li>
  
  <li>
    Change the current directory to `C:\DictionaryServiceDemo\DictionaryDemo` and follow the step [Run the Client](#run-the-client).
  </li>

## <a name="compiling-and-running-the-dictionaryservicedemo-sample" id="compiling-and-running-the-dictionaryservicedemo-sample">Compiling and Running the DictionaryServiceDemo Sample</a>

The `DictionaryServiceDemo` sample includes Apache Ant build files, which are all named `build.xml`. The following steps show you how to use Apache Ant to compile, build, and run the `DictionaryServiceDemo` sample:


  <li>Install Apache Ant: Go to the following link to download and install Apache Ant:
  
  `[http://ant.apache.org/](http://ant.apache.org/)`
  
  <p>Ensure that the directory that contains the Apache Ant executable file is in your `PATH` environment variable so that you can run it from any directory. In addition, ensure that your JDK's `bin` directory, which contains the `java` and `javac` executables (`java.exe` and `javac.exe` for Microsoft Windows). is in your `PATH` environment variable. See
[PATH and CLASSPATH](../../essential/environment/paths.html)  for information about setting the `PATH` environment variable.</p></li>
  
  <li>
    <p>
    Download and unzip the sample code: Download and unzip the file `[DictionaryServiceDemo.zip](examples/DictionaryServiceDemo/DictionaryServiceDemo.zip)` to your computer. These steps assume that you unzipped the contents of this file into the directory `C:\DictionaryServiceDemo`.
  
    </p>
  </li>
  
  <li>
    Compile the code: Change the current directory to `C:\DictionaryServiceDemo` and run the following command:
  
    <pre class="codeblock">ant compile-all</code></pre>
  
    This command compiles the source code in the `src` directories contained in the directories `DictionaryDemo`, `DictionaryServiceProvider`, `ExtendedDictionary`, and `GeneralDictionary`, and puts the generated `class` files in the corresponding `build` directories.
  </li>
  
  <li>
    Package the compiled Java files into JAR files: Ensure the current directory is `C:\DictionaryServiceDemo` and run the following command:
  
    <pre class="codeblock">ant jar</code></pre>
  
    This command creates the following JAR files:
    
    <ul>
      1. `DictionaryDemo/dist/DictionaryDemo.jar`
      1. `DictionaryServiceProvider/dist/DictionaryServiceProvider.jar`
      1. `GeneralDictionary/dist/GeneralDictionary.jar`
      1. `ExtendedDictionary/dist/ExtendedDictionary.jar`
    </ul>
  </li>
  
  <li>
    <p>Run the sample: Ensure that the directory that contains the `java` executable is in your `PATH` environment variable. See
[PATH and CLASSPATH](../../essential/environment/paths.html)     for more information.</p>
     
    Change the current directory to `C:\DictionaryServiceDemo\DictionaryDemo` and run the following command:
    
    <pre class="codeblock">ant run</code></pre>
    
    The sample prints the following:
    
    `book: a set of written or printed pages, usually bound with a protective cover<br />editor: a person who edits<br />xml: a document standard often used in web services, among other things<br />REST: an architecture style for creating, reading, updating, and deleting data that attempts to use the common vocabulary of the HTTP protocol; Representational State Transfer`
    
  </li>

## <a name="understanding-the-dictionaryservicedemo-sample" id="understanding-the-dictionaryservicedemo-sample">Understanding the DictionaryServiceDemo Sample</a>


    The following steps show you how to re-create the contents of the file `[DictionaryServiceDemo.zip](examples/DictionaryServiceDemo/DictionaryServiceDemo.zip)`. These steps show you how the sample works and how to run it.
 


### <a name="define-the-service-provider-interface" id="define-the-service-provider-interface">1. Define the Service Provider Interface</a>

The `DictionaryServiceDemo` sample defines one SPI, the
[`<code>Dictionary.java`</code>](examples/DictionaryServiceDemo/DictionaryServiceProvider/src/dictionary/spi/Dictionary.java) interface. It contains only one method:

```
`DictionaryService.java`</code></a> class loads and accesses available `Dictionary` service providers on behalf of dictionary service clients:</p> 

<pre class="codeblock">

package dictionary;

import dictionary.spi.Dictionary;
import java.util.Iterator;
import java.util.ServiceConfigurationError;
import java.util.ServiceLoader;

public class DictionaryService {

    private static DictionaryService service;
    private ServiceLoader&lt;Dictionary&gt; loader;

    private DictionaryService() {
        loader = ServiceLoader.load(Dictionary.class);
    }

    public static synchronized DictionaryService getInstance() {
        if (service == null) {
            service = new DictionaryService();
        }
        return service;
    }


    public String getDefinition(String word) {
        String definition = null;

        try {
            Iterator&lt;Dictionary&gt; dictionaries = loader.iterator();
            while (definition == null &amp;&amp; dictionaries.hasNext()) {
                Dictionary d = dictionaries.next();
                definition = d.getDefinition(word);
            }
        } catch (ServiceConfigurationError serviceError) {
            definition = null;
            serviceError.printStackTrace();

        }
        return definition;
    }
}

```

The sample stores the compiled class file in the directory `DictionaryServiceProvider/build`.

The
`DictionaryService` class implements the singleton design pattern. This means that only a single instance of the `DictionaryService` class is ever created. See the section [The Singleton Design Pattern](#singleton) for more information.

The `DictionaryService` class is the dictionary service client's  entry point to using any installed `Dictionary` service provider. Use the
[`ServiceLoader.load`](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html#load-java.lang.Class-) method to retrieve the private static member `DictionaryService.service`, the singleton service entry point. Then the application can call the `getDefinition` method, which iterates through available `Dictionary` providers until it finds the targeted word. The `getDefinition` method returns null if no `Dictionary` instance contains the specified definition of the word.

The dictionary service uses the `ServiceLoader.load` method to find the target class. The SPI is defined by the interface `dictionary.spi.Dictionary`, so the example uses this class as the load method's argument. The default load method searches the application class path with the default class loader.

However, an overloaded version of this method enables you to specify 
custom class loaders if you wish. That enables you to do more 
sophisticated class searches. A particularly enthusiastic programmer 
might, for example, create a
[`ClassLoader`](https://docs.oracle.com/javase/8/docs/api/java/lang/ClassLoader.html) instance that can search in an application-specific subdirectory that contains 
provider JARs added during runtime. The result is an application 
that does not require a restart to access new provider classes.

After a loader for this class exists, you can use its iterator method to access and use each provider that it finds. The `getDefinition` method uses a `Dictionary` iterator to go through the providers until it finds a definition for the specified word. The iterator method caches `Dictionary` instances, so successive calls require little additional processing time. If new providers have been placed into service since the last invocation, the iterator method adds them to the list.

The
[`<code>DictionaryDemo.java`</code>](examples/DictionaryServiceDemo/DictionaryDemo/src/dictionary/DictionaryDemo.java) class uses this service. To use the service, the application obtains a `DictionaryService` instance and calls the `getDefinition` method. If a definition is available, the application prints it. If a definition is not available, the application prints a message stating that no available dictionary carries the word.

#### <a name="singleton" id="singleton">The Singleton Design Pattern</a>

A design pattern is a general solution to a common problem in software design. The idea is that the solution gets translated into code, and that code can be applied in different situations where the problem occurs. The singleton pattern describes a technique to ensure that only a single instance of a class is ever created. In essence, the technique takes the following approach: Do not let anyone outside the class create instances of the object.

For example, the
[`<code>DictionaryService`</code>](examples/DictionaryServiceDemo/DictionaryServiceProvider/src/dictionary/DictionaryService.java) class implements the singleton pattern as follows:

  
  - Declares the `DictionaryService` constructor as `private`, which prevents all other classes, except `DictionaryService`, from creating instances of it.
  
  - Defines the `DictionaryService` member variable `service` as `static`, which ensures only one instance of `DictionaryService` exists.
  
  - Defines the method `getInstance`, which enables other classes controlled access to the `DictionaryService` member variable `service`.

### <a name="implement-the-service-provider" id="implement-the-service-provider">3. Implement the Service Provider</a>

To provide this service, you must create a
[`<code>Dictionary.java`</code>](examples/DictionaryServiceDemo/DictionaryServiceProvider/src/dictionary/spi/Dictionary.java) implementation. To keep things simple, create a general dictionary that defines just a few words. You can implement the dictionary with a database, a set of property files, or any other technology. The easiest way to demonstrate the provider pattern is to include all the words and definitions within a single file. 

The following code shows an implementation of the `Dictionary` SPI, the
[`<code>GeneralDictionary.java`</code>](examples/DictionaryServiceDemo/GeneralDictionary/src/dictionary/GeneralDictionary.java) class. Notice that it provides a no-argument constructor and implements the `getDefinition` method defined by the SPI.

```
`ExtendedDictionary.java`</code></a> service provider is an extended dictionary containing technical terms familiar to most software developers. </p>

<pre class="codeblock">

package dictionary;

import dictionary.spi.Dictionary;
import java.util.SortedMap;
import java.util.TreeMap;

public class ExtendedDictionary implements Dictionary {

        private SortedMap&lt;String, String&gt; map;

    public ExtendedDictionary() {
        map = new TreeMap&lt;String, String&gt;();
        map.put(
            "xml",
            "a document standard often used in web services, among other " +
                "things");
        map.put(
            "REST",
            "an architecture style for creating, reading, updating, " +
                "and deleting data that attempts to use the common " +
                "vocabulary of the HTTP protocol; Representational State " +
                "Transfer");
    }

    @Override
    public String getDefinition(String word) {
        return map.get(word);
    }

}

```

The sample stores the compiled class file in the directory `ExtendedDictionary/build`. **Note**: You must compile the classes `dictionary.DictionaryService` and `dictionary.spi.Dictionary` before the class `ExtendedDictionary`.

It is easy to imagine customers using a complete set of `Dictionary` providers for their own special needs. The service loader API enables them to add new dictionaries to their application as their needs or preferences change. Because the underlying word-processor 
application is extensible, no additional coding is required for customers to use the new providers.

### <a name="register-service-providers" id="register-service-providers">4. Register Service Providers</a>

To register your service provider, you create a provider configuration file, which is stored in the `META-INF/services` directory of the service provider's JAR file. The name of the configuration file is the fully qualified class name of the service provider, in which each component of the name is separated by a period (`.`), and nested classes are separated by a dollar sign (`$`).

The provider configuration file contains the fully qualified class names of your service providers, one name per line. The file must be UTF-8 encoded. Additionally, you can include comments in the file by beginning the comment line with the number sign (`#`).

For example, to register the service provider `GeneralDictionary` create a text file named
[`<code>dictionary.spi.Dictionary`</code>](examples/DictionaryServiceDemo/GeneralDictionary/src/META-INF/services/dictionary.spi.Dictionary) . This file contains one line:

```
`dictionary.spi.Dictionary`</code></a> . This file contains one line:</p>

<pre class="codeblock">
dictionary.ExtendedDictionary

```

### <a name="create-a-client-that-uses-the-service-and-service-providers" id="create-a-client-that-uses-the-service-and-service-providers">5. Create a Client That Uses the Service and Service Providers</a>

Because developing a full word-processor application is a 
significant undertaking, this tutorial provides a simpler application that uses the `DictionaryService` and `Dictionary` SPI. The `DictionaryDemo` sample searches for the words **book**, **editor**, **xml**, and **REST** words from any `Dictionary` providers on the class path and retrieves their definitions.

The following is the
[`<code>DictionaryDemo`</code>](examples/DictionaryServiceDemo/DictionaryDemo/src/dictionary/DictionaryDemo.java) sample. It requests a definition of the target word from the `DictionaryService` instance, which passes the request to its known `Dictionary` providers.

```
META-INF</code>
    <ul>
      <li>`services`
        <ul>
          - `dictionary.spi.Dictionary`
        </ul>
      </li>
    </ul>
  </li>
  <li>`dictionary`
    <ul>
      - `GeneralDictionary.class`
    </ul>
  </li>
</ul>

Similarly, to package the `ExtendedDictionary` service provider, create a JAR file named `ExtendedDictionary/dist/ExtendedDictionary.jar` that contains the compiled class file of this service provider and the configuration file in the following directory structure:

<ul>
  <li>`META-INF`
    <ul>
      <li>`services`
        <ul>
          - `dictionary.spi.Dictionary`
        </ul>
      </li>
    </ul>
  </li>
  <li>`dictionary`
    <ul>
      - `ExtendedDictionary.class`
    </ul>
  </li>
</ul>

Note that the provider configuration file must be in the directory `META-INF/services` in the JAR file.

<h4><a name="packaging-the-dictionary-service-in-a-jar-file" id="packaging-the-dictionary-service-in-a-jar-file">Packaging the Dictionary SPI and Dictionary Service in a JAR File</a></h4>

Create a JAR file named `DictionaryServiceProvider/dist/DictionaryServiceProvider.jar` that contains the following files:

<ul>
  <li>`dictionary`
    <ul>
      - `DictionaryService.class`
      <li>`spi`
        <ul>
          - `Dictionary.class`
        </ul>
      </li>
    </ul>
  </li>
</ul>
        
<h4><a name="packaging-the-client-in-a-jar-file" id="packaging-the-client-in-a-jar-file">Packaging the Client in a JAR File</a></h4>

Create a JAR file named `DictionaryDemo/dist/DictionaryDemo.jar` that contains the following file:

<ul>
  <li>`dictionary`
    <ul>
      - `DictionaryDemo.class`
    </ul>
  </li>
</ul>
 
<h3><a name="run-the-client" id="run-the-client">7. Run the Client</a></h3>

The following command runs the `DictionaryDemo` sample with the `GeneralDictionary` service provider:

**Linux and Solaris:**
`java -Djava.ext.dirs=../DictionaryServiceProvider/dist:../GeneralDictionary/dist -cp dist/DictionaryDemo.jar dictionary.DictionaryDemo`

**Windows:**
`java -Djava.ext.dirs=..\DictionaryServiceProvider\dist;..\GeneralDictionary\dist -cp dist\DictionaryDemo.jar dictionary.DictionaryDemo`

When using this command, the following is assumed:

<ul>
  - The current directory is `DictionaryDemo`.
  <li>The following JAR files exist:
    <ul>
      - `DictionaryDemo/dist/DictionaryDemo.jar`: Contains the `DictionaryDemo` class
      - `DictionaryServiceProvider/dist/DictionaryServiceProvider.jar`: Contains the `Dictionary` SPI and the `DictionaryService` class
      - `GeneralDictionary/dist/GeneralDictionary.jar`: Contains the `GeneralDictionary` service provider and configuration file
    </ul>
  </li>
</ul>


The command prints the following:

<pre class="codeblock">
book: a set of written or printed pages, usually bound with a protective cover
editor: a person who edits
xml: Cannot find definition for this word.
REST: Cannot find definition for this word.

```

      - `GeneralDictionary.class`
    
      - `ExtendedDictionary.class`
    
#### <a name="packaging-the-dictionary-service-in-a-jar-file" id="packaging-the-dictionary-service-in-a-jar-file">Packaging the Dictionary SPI and Dictionary Service in a JAR File</a>

  <li>`dictionary`
    <ul>
      - `DictionaryDemo.class`
    
### <a name="run-the-client" id="run-the-client">7. Run the Client</a>

Suppose you run the following command and `ExtendedDictionary/dist/ExtendedDictionary.jar` exists:

**Linux and Solaris:**

`java -Djava.ext.dirs=../DictionaryServiceProvider/dist:../ExtendedDictionary/dist -cp dist/DictionaryDemo.jar dictionary.DictionaryDemo`

**Windows:**

`java -Djava.ext.dirs=..\DictionaryServiceProvider\dist;..\ExtendedDictionary\dist -cp dist\DictionaryDemo.jar dictionary.DictionaryDemo`

The command prints the following:

## <a name="the-serviceloader-class" id="the-serviceloader-class">The ServiceLoader Class</a>

The `java.util.ServiceLoader` class helps you find, load, and use service providers. It searches for service providers on your application's class path or in your runtime environment's extensions directory. It loads them and enables your application to use the provider's APIs. If you add new providers to the class path or runtime extension directory, the `ServiceLoader` class finds them. If your application knows the provider interface, it can find and use different implementations of that interface. You can use the first loadable instance of the interface or iterate through all the available interfaces.

The `ServiceLoader` class is final, which means that you cannot make it a subclass or override its loading algorithms. You cannot, for example, change its algorithm to search for services from a different location.

From the perspective of the `ServiceLoader` class, all services have a single type, which is usually a single interface or abstract class. The provider itself contains one or more concrete classes that extend the service type with an implementation specific to its purpose. The `ServiceLoader` class requires that the single exposed provider type has a default constructor, which requires no arguments. This enables the `ServiceLoader` class to easily instantiate the service providers that it finds.

Providers are located and instantiated on demand. A service loader maintains a cache of the providers that were loaded. Each invocation of the loader's `iterator` method returns an iterator that first yields all of the elements of the cache, in instantiation order. The service loader then locates and instantiates any new providers, adding each one to the cache in turn. You can clear the provider cache with the `reload` method.

To create a loader for a specific class, provide the class itself to the `load` or `loadInstalled` method. You can use default class loaders or provide your own `ClassLoader` subclass.

The `loadInstalled` method searches the runtime environment's extension directory of installed runtime providers. The default extension location is your runtime environment's `jre/lib/ext` directory. You should use the extension location only for well-known, trusted providers because this location becomes part of the class path for all applications. In this article, providers do not use the extension directory but will instead depend on an application-specific class path.

## <a name="limitations-of-the-service-loader-api" id="limitations-of-the-service-loader-api">Limitations of the ServiceLoader API</a>

The `ServiceLoader` API is useful, but it has limitations. For example, it is impossible to derive a class from the `ServiceLoader` class, so you cannot modify its behavior. You can use custom `ClassLoader` subclasses to change how classes are found, but `ServiceLoader` itself cannot be extended. Also, the current `ServiceLoader`
 class cannot tell your application when new providers are available at 
runtime. Additionally, you cannot add change-listeners to the loader to 
find out whether a new provider was placed into an 
application-specific extension directory. 

The public `ServiceLoader` API is available in Java SE 6. 
Although the loader service existed as early as JDK 1.3, the API was 
private and only available to internal Java runtime code. 

## <a name="summary" id="summary">Summary</a>

Extensible applications provide service points that can be extended 
by service providers. The easiest way to create an extensible 
application is to use the `ServiceLoader`, which is available for Java SE 6 and later. Using this class, you can add provider 
implementations to the application class path to make new functionality 
available. The `ServiceLoader` class is final, so you cannot modify its abilities.
