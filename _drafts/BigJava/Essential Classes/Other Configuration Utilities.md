
# Other Configuration Utilities

Here is a summary of some other configuration utilities.

The *Preferences API* allows applications to store and retrieve configuration data in an implementation-dependent backing store. Asynchronous updates are supported, and the same set of preferences can be safely updated by multiple threads and even multiple applications. For more information, refer to the 
[Preferences API Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/preferences/index.html).

An application deployed in a *JAR archive* uses a *manifest* to describe the contents of the archive. For more information, refer to the 
[Packaging Programs in JAR Files](../../deployment/jar/index.html) lesson.

The configuration of a *Java Web Start application* is contained in a *JNLP file*. For more information, refer to the 
[Java Web Start](../../deployment/webstart/index.html) lesson.

The configuration of a *Java Plug-in applet* is partially determined by the HTML tags used to embed the applet in the web page. Depending on the applet and the browser, these tags can include `&lt;applet&gt;`, `&lt;object&gt;`, `&lt;embed&gt;`, and `&lt;param&gt;`. For more information, refer to the 
[Java Applets](../../deployment/applet/index.html) lesson.

The class 
[`java.util.ServiceLoader`](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html) provides a simple *service provider* facility. A service provider is an implementation of a *service* &#151; a well-known set of interfaces and (usually abstract) classes. The classes in a service provider typically implement the interfaces and subclass the classes defined in the service. Service providers can be installed as extensions (see 
[The Extension Mechanism](../../ext/index.html)). Providers can also be made available by adding them to the class path or by some other platform-specific means.
