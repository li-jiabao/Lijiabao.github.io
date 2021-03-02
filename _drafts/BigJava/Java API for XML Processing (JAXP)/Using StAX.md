
# Oracle's Streaming XML Parser Implementation

<a name="indexterm-44" id="indexterm-44"></a><a name="indexterm-45" id="indexterm-45"></a>

Application Server 9.1 includes Sun Microsystems&#8217; JSR 173 (StAX) implementation, called the Sun Java Streaming XML Parser (referred to as Streaming XML Parser). The Streaming XML Parser is a high-speed, non-validating, W3C XML 1.0 and Namespace 1.0-compliant streaming XML pull parser built upon the Xerces2 codebase.

In Sun&#8217;s Streaming XML Parser implementation, the Xerces2 lower layers, particularly the Scanner and related classes, have been redesigned to behave in a pull fashion. In addition to the changes in the lower layers, the Streaming XML Parser includes additional StAX-related functionality and many performance-enhancing improvements. The Streaming XML Parser is implemented in the <tt>appserv-ws.jar</tt> and <tt>javaee.jar</tt> files, both of which are located in the *install_dir*<tt>/lib/</tt> directory.

Included with the JAXP reference implementation are StAX code examples, located in the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/stax</tt> directory, that illustrate how Sun&#8217;s Streaming XML Parser implementation works. These examples are described in 
[Example Code](example.html).

Before you proceed with the example code, there are two aspects of the Streaming XML Parser of which you should be aware:

<li>
[Reporting CDATA Events](#bnbfj)
</li>
<li>
[Streaming XML Parser Factories Implementation](#bnbfk)
</li>

These topics are discussed below.

<a name="bnbfj" id="bnbfj"></a>

## Reporting CDATA Events

<a name="indexterm-46" id="indexterm-46"></a><a name="indexterm-47" id="indexterm-47"></a>

The <tt>javax.xml.stream.XMLStreamReader</tt> implemented in the Streaming XML Parser does not report CDATA events. If you have an application that needs to receive such events, configure the <tt>XMLInputFactory</tt> to set the following implementation-specific <tt>report-cdata-event</tt> property:

```

XMLInputFactory factory = XMLInptuFactory.newInstance();
factory.setProperty("report-cdata-event", Boolean.TRUE);

```

<a name="bnbfk" id="bnbfk"></a>

## Streaming XML Parser Factories Implementation

Most applications do not need to know the factory implementation class name. Just adding the <tt>javaee.jar</tt> and <tt>appserv-ws.jar</tt> files to the classpath is sufficient for most applications because these two jars supply the factory implementation classname for various Streaming XML Parser properties under the <tt>META-INF/services</tt> directory&#8212;for example, <tt>javax.xml.stream.XMLInputFactory</tt>, <tt>javax.xml.stream.XMLOutputFactory</tt>, and <tt>javax.xml.stream.XMLEventFactory</tt>&#8212;which is the third step of a lookup operation when an application asks for the factory instance. See the Javadoc for the <tt>XMLInputFactory.newInstance</tt> method for more information about the lookup mechanism.

However, there may be scenarios when an application would like to know about the factory implementation class name and set the property explicitly. These scenarios could include cases where there are multiple JSR 173 implementations in the classpath and the application wants to choose one, perhaps one that has superior performance, contains a crucial bug fix, or suchlike.

If an application sets the <tt>SystemProperty</tt>, it is the first step in a lookup operation, and so obtaining the factory instance would be fast compared to other options; for example:

```

javax.xml.stream.XMLInputFactory --&gt;
  com.sun.xml.stream.ZephyrParserFactory

javax.xml.stream.XMLOutputFactory --&gt;
  com.sun.xml.stream.ZephyrWriterFactor

javax.xml.stream.XMLEventFactory --&gt;
  com.sun.xml.stream.events.ZephyrEventFactory

```
