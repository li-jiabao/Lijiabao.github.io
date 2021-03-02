
# Using StAX

<a name="indexterm-22" id="indexterm-22"></a>

In general, StAX programmers create XML stream readers, writers, and events by using the <tt>XMLInputFactory</tt>, <tt>XMLOutputFactory</tt>, and <tt>XMLEventFactory</tt> classes. Configuration is done by setting properties on the factories, whereby implementation-specific settings can be passed to the underlying implementation using the <tt>setProperty</tt> method on the factories. Similarly, implementation-specific settings can be queried using the <tt>getProperty</tt> factory method.

The <tt>XMLInputFactory</tt>, <tt>XMLOutputFactory</tt>, and <tt>XMLEventFactory</tt> classes are described below, followed by discussions of resource allocation, namespace and attribute management, error handling, and then finally reading and writing streams using the cursor and iterator APIs.

<a name="bnben" id="bnben"></a>

## StAX Factory Classes

<a name="indexterm-23" id="indexterm-23"></a><a name="indexterm-24" id="indexterm-24"></a>

The StAX factory classes. <tt>XMLInputFactory</tt>, <tt>XMLOutputFactory</tt>, and <tt>XMLEventFactory</tt>, let you define and configure implementation instances of XML stream reader, stream writer, and event classes.

<a name="bnbeo" id="bnbeo"></a>

### XMLInputFactory

<a name="indexterm-25" id="indexterm-25"></a><a name="indexterm-26" id="indexterm-26"></a>

The <tt>XMLInputFactory</tt> class lets you configure implementation instances of XML stream reader processors created by the factory. New instances of the abstract class <tt>XMLInputFactory</tt> are created by calling the <tt>newInstance</tt> method on the class. The static method <tt>XMLInputFactory.newInstance</tt> is then used to create a new factory instance.

Deriving from JAXP, the <tt>XMLInputFactory.newInstance</tt> method determines the specific <tt>XMLInputFactory</tt> implementation class to load by using the following lookup procedure:

<li>
Use the <tt>javax.xml.stream.XMLInputFactory</tt> system property.
</li>
<li>
Use the <tt>lib/xml.stream.properties</tt> file in the Java SE platform's Java Runtime Environment (JRE) directory.
</li>
<li>
Use the Services API, if available, to determine the classname by looking in the <tt>META-INF/services/javax.xml.stream.XMLInputFactory</tt> files in JAR files available to the JRE.
</li>
<li>
Use the platform default <tt>XMLInputFactory</tt> instance.
</li>

After getting a reference to an appropriate <tt>XMLInputFactory</tt>, an application can use the factory to configure and create stream instances. The following table lists the properties supported by <tt>XMLInputFactory</tt>. See the StAX specification for a more detailed listing.

<a name="bnbep" id="bnbep"></a>
<th id="h1" align="left" valign="top">Property</th><th id="h2" align="left" valign="top">Description</th>

Description
<td headers="h1" align="left" valign="top"><tt>isValidating</tt></td><td headers="h2" align="left" valign="top">Turns on implementation-specific validation.</td>

Turns on implementation-specific validation.
<td headers="h1" align="left" valign="top"><tt>isCoalescing</tt></td><td headers="h2" align="left" valign="top">**(Required)** Requires the processor to coalesce adjacent character data.</td>

**(Required)** Requires the processor to coalesce adjacent character data.
<td headers="h1" align="left" valign="top"><tt>isNamespaceAware</tt></td><td headers="h2" align="left" valign="top">Turns off namespace support. All implementations must support namespaces. Support for non-namespace-aware documents is optional.</td>

Turns off namespace support. All implementations must support namespaces. Support for non-namespace-aware documents is optional.
<td headers="h1" align="left" valign="top"><tt>isReplacingEntityReferences</tt></td><td headers="h2" align="left" valign="top">**(Required)** Requires the processor to replace internal entity references with their replacement value and report them as characters or the set of events that describe the entity.</td>

**(Required)** Requires the processor to replace internal entity references with their replacement value and report them as characters or the set of events that describe the entity.
<td headers="h1" align="left" valign="top"><tt>isSupportingExternalEntities</tt></td><td headers="h2" align="left" valign="top">**(Required)** Requires the processor to resolve external parsed entities.</td>

**(Required)** Requires the processor to resolve external parsed entities.
<td headers="h1" align="left" valign="top"><tt>reporter</tt></td><td headers="h2" align="left" valign="top">**(Required)** Sets and gets the implementation of the <tt>XMLReporter</tt> interface.</td>

**(Required)** Sets and gets the implementation of the <tt>XMLReporter</tt> interface.
<td headers="h1" align="left" valign="top"><tt>resolver</tt></td><td headers="h2" align="left" valign="top">**(Required)** Sets and gets the implementation of the <tt>XMLResolver</tt> interface.</td>

**(Required)** Sets and gets the implementation of the <tt>XMLResolver</tt> interface.
<td headers="h1" align="left" valign="top"><tt>allocator</tt></td><td headers="h2" align="left" valign="top">**(Required)** Sets and gets the implementation of the <tt>XMLEventAllocator</tt> interface.</td>

**(Required)** Sets and gets the implementation of the <tt>XMLEventAllocator</tt> interface.

<a name="bnbeq" id="bnbeq"></a>

### XMLOutputFactory

<a name="indexterm-27" id="indexterm-27"></a><a name="indexterm-28" id="indexterm-28"></a>

New instances of the abstract class <tt>XMLOutputFactory</tt> are created by calling the <tt>newInstance</tt> method on the class. The static method <tt>XMLOutputFactory.newInstance</tt> is then used to create a new factory instance. The algorithm used to obtain the instance is the same as for <tt>XMLInputFactory</tt> but references the <tt>javax.xml.stream.XMLOutputFactory</tt> system property.

<tt>XMLOutputFactory</tt> supports only one property, <tt>javax.xml.stream.isRepairingNamespaces</tt>. This property is required, and its purpose is to create default prefixes and associate them with Namespace URIs. See the StAX specification for more information.

<a name="bnber" id="bnber"></a>

### XMLEventFactory

<a name="indexterm-29" id="indexterm-29"></a><a name="indexterm-30" id="indexterm-30"></a>

New instances of the abstract class <tt>XMLEventFactory</tt> are created by calling the <tt>newInstance</tt> method on the class. The static method <tt>XMLEventFactory.newInstance</tt> is then used to create a new factory instance. This factory references the <tt>javax.xml.stream.XMLEventFactory</tt> property to instantiate the factory. The algorithm used to obtain the instance is the same as for <tt>XMLInputFactory</tt> and <tt>XMLOutputFactory</tt> but references the <tt>javax.xml.stream.XMLEventFactory</tt> system property.

There are no default properties for <tt>XMLEventFactory</tt>.

<a name="bnbes" id="bnbes"></a>

## Resources, Namespaces, and Errors

<a name="indexterm-31" id="indexterm-31"></a>

The StAX specification handles resource resolution, attributes and namespace, and errors and exceptions as described below.

<a name="bnbet" id="bnbet"></a>

### Resource Resolution

The <tt>XMLResolver</tt> interface provides a means to set the method that resolves resources during XML processing. An application sets the interface on <tt>XMLInputFactory</tt>, which then sets the interface on all processors created by that factory instance.

<a name="bnbeu" id="bnbeu"></a>

### Attributes and Namespaces

Attributes are reported by a StAX processor using lookup methods and strings in the cursor interface, and <tt>Attribute</tt> and <tt>Namespace</tt> events in the iterator interface. Note here that namespaces are treated as attributes, although namespaces are reported separately from attributes in both the cursor and iterator APIs. Note also that namespace processing is optional for StAX processors. See the StAX specification for complete information about namespace binding and optional namespace processing.

<a name="bnbev" id="bnbev"></a>

### Error Reporting and Exception Handling

All fatal errors are reported by way of the <tt>javax.xml.stream.XMLStreamException</tt> interface. All nonfatal errors and warnings are reported using the <tt>javax.xml.stream.XMLReporter</tt> interface.

<a name="bnbew" id="bnbew"></a>

## Reading XML Streams

<a name="indexterm-32" id="indexterm-32"></a><a name="indexterm-33" id="indexterm-33"></a>

As described earlier in this lesson, the way you read XML streams with a StAX processor&#8212;and more importantly, what you get back&#8212;varies significantly depending on whether you are using the StAX cursor API or the event iterator API. The following two sections describe how to read XML streams with each of these APIs.

<a name="bnbex" id="bnbex"></a>

### Using XMLStreamReader

<a name="indexterm-34" id="indexterm-34"></a><a name="indexterm-35" id="indexterm-35"></a>

The <tt>XMLStreamReader</tt> interface in the StAX cursor API lets you read XML streams or documents in a forward direction only, one item in the infoset at a time. The following methods are available for pulling data from the stream or skipping unwanted events:

<li>
Get the value of an attribute
</li>
<li>
Read XML content
</li>
<li>
Determine whether an element has content or is empty
</li>
<li>
Get indexed access to a collection of attributes
</li>
<li>
Get indexed access to a collection of namespaces
</li>
<li>
Get the name of the current event (if applicable)
</li>
<li>
Get the content of the current event (if applicable)
</li>

Instances of <tt>XMLStreamReader</tt> have at any one time a single current event on which its methods operate. When you create an instance of <tt>XMLStreamReader</tt> on a stream, the initial current event is the <tt>START_DOCUMENT</tt> state. The <tt>XMLStreamReader.next</tt> method can then be used to step to the next event in the stream.

<a name="bnbey" id="bnbey"></a>

### Reading Properties, Attributes, and Namespaces

The <tt>XMLStreamReader.next</tt> method loads the properties of the next event in the stream. You can then access those properties by calling the <tt>XMLStreamReader.getLocalName</tt> and <tt>XMLStreamReader.getText</tt> methods.

When the <tt>XMLStreamReader</tt> cursor is over a <tt>StartElement</tt> event, it reads the name and any attributes for the event, including the namespace. All attributes for an event can be accessed using an index value, and can also be looked up by namespace URI and local name. Note, however, that only the namespaces declared on the current <tt>StartEvent</tt> are available; previously declared namespaces are not maintained, and redeclared namespaces are not removed.

<a name="bnbez" id="bnbez"></a>

### XMLStreamReader Methods

<tt>XMLStreamReader</tt> provides the following methods for retrieving information about namespaces and attributes:

```

int getAttributeCount();
String getAttributeNamespace(int index);
String getAttributeLocalName(int index);
String getAttributePrefix(int index);
String getAttributeType(int index);
String getAttributeValue(int index);
String getAttributeValue(String namespaceUri, String localName);
boolean isAttributeSpecified(int index);

```

Namespaces can also be accessed using three additional methods:

```

int getNamespaceCount();
String getNamespacePrefix(int index);
String getNamespaceURI(int index);

```

<a name="bnbfa" id="bnbfa"></a>

### Instantiating an XMLStreamReader

This example, taken from the StAX specification, shows how to instantiate an input factory, create a reader, and iterate over the elements of an XML stream:

```

XMLInputFactory f = XMLInputFactory.newInstance();
XMLStreamReader r = f.createXMLStreamReader( ... );
while(r.hasNext()) {
    r.next();
}

```

<a name="bnbfb" id="bnbfb"></a>

### Using XMLEventReader

<a name="indexterm-36" id="indexterm-36"></a><a name="indexterm-37" id="indexterm-37"></a>

The <tt>XMLEventReader</tt> API in the StAX event iterator API provides the means to map events in an XML stream to allocated event objects that can be freely reused, and the API itself can be extended to handle custom events.

<tt>XMLEventReader</tt> provides four methods for iteratively parsing XML streams:

<li>
<tt>next</tt>: Returns the next event in the stream
</li>
<li>
<tt>nextEvent</tt>: Returns the next typed XMLEvent
</li>
<li>
<tt>hasNext</tt>: Returns true if there are more events to process in the stream
</li>
<li>
<tt>peek</tt>: Returns the event but does not iterate to the next event
</li>

For example, the following code snippet illustrates the <tt>XMLEventReader</tt> method declarations:

```

package javax.xml.stream;
import java.util.Iterator;

public interface XMLEventReader extends Iterator {
    public Object next();
    public XMLEvent nextEvent() throws XMLStreamException;
    public boolean hasNext();
    public XMLEvent peek() throws XMLStreamException;
    // ...
}

```

To read all events on a stream and then print them, you could use the following:

```

while(stream.hasNext()) {
    XMLEvent event = stream.nextEvent();
    System.out.print(event);
}

```

<a name="bnbfc" id="bnbfc"></a>

### Reading Attributes

You can access attributes from their associated <tt>javax.xml.stream.StartElement</tt>, as follows:

```

public interface StartElement extends XMLEvent {
    public Attribute getAttributeByName(QName name);
    public Iterator getAttributes();
}

```

You can use the <tt>getAttributes</tt> method on the <tt>StartElement</tt> interface to use an <tt>Iterator</tt> over all the attributes declared on that <tt>StartElement</tt>.

<a name="bnbfd" id="bnbfd"></a>

### Reading Namespaces

Similar to reading attributes, namespaces are read using an <tt>Iterator</tt> created by calling the <tt>getNamespaces</tt> method on the <tt>StartElement</tt> interface. Only the namespace for the current <tt>StartElement</tt> is returned, and an application can get the current namespace context by using <tt>StartElement.getNamespaceContext</tt>.

<a name="bnbfe" id="bnbfe"></a>

## Writing XML Streams

<a name="indexterm-38" id="indexterm-38"></a><a name="indexterm-39" id="indexterm-39"></a>

StAX is a bidirectional API, and both the cursor and event iterator APIs have their own set of interfaces for writing XML streams. As with the interfaces for reading streams, there are significant differences between the writer APIs for cursor and event iterator. The following sections describe how to write XML streams using each of these APIs.

<a name="bnbff" id="bnbff"></a>

### Using XMLStreamWriter

<a name="indexterm-40" id="indexterm-40"></a><a name="indexterm-41" id="indexterm-41"></a>

The <tt>XMLStreamWriter</tt> interface in the StAX cursor API lets applications write back to an XML stream or create entirely new streams. XMLStreamWriter has methods that let you:

<li>
Write well-formed XML
</li>
<li>
Flush or close the output
</li>
<li>
Write qualified names
</li>

Note that <tt>XMLStreamWriter</tt> implementations are not required to perform well-formedness or validity checks on input. While some implementations may perform strict error checking, others may not. The rules you implement are applied to properties defined in the <tt>XMLOutputFactory</tt> class.

The <tt>writeCharacters</tt> method is used to escape characters such as <tt>&amp;</tt>, <tt>&lt;</tt>, <tt>&gt;</tt>, and <tt>"</tt>. Binding prefixes can be handled by either passing the actual value for the prefix, by using the <tt>setPrefix</tt> method, or by setting the property for defaulting namespace declarations.

The following example, taken from the StAX specification, shows how to instantiate an output factory, create a writer, and write XML output:

```

XMLOutputFactory output = XMLOutputFactory.newInstance();
XMLStreamWriter writer = output.createXMLStreamWriter( ... );

writer.writeStartDocument();
writer.setPrefix("c","http://c");
writer.setDefaultNamespace("http://c");

writer.writeStartElement("http://c","a");
writer.writeAttribute("b","blah");
writer.writeNamespace("c","http://c");
writer.writeDefaultNamespace("http://c");

writer.setPrefix("d","http://c");
writer.writeEmptyElement("http://c","d");
writer.writeAttribute("http://c", "chris","fry");
writer.writeNamespace("d","http://c");
writer.writeCharacters("Jean Arp");
writer.writeEndElement();

writer.flush();

```

This code generates the following XML (new lines are non-normative):

```

&lt;?xml version=&#8217;1.0&#8217; encoding=&#8217;utf-8&#8217;?&gt;
&lt;a b="blah" xmlns:c="http://c" xmlns="http://c"&gt;
&lt;d:d d:chris="fry" xmlns:d="http://c"/&gt;Jean Arp&lt;/a&gt;

```

<a name="bnbfg" id="bnbfg"></a>

### Using XMLEventWriter

<a name="indexterm-42" id="indexterm-42"></a><a name="indexterm-43" id="indexterm-43"></a>

The <tt>XMLEventWriter</tt> interface in the StAX event iterator API lets applications write back to an XML stream or create entirely new streams. This API can be extended, but the main API is as follows:

```

public interface XMLEventWriter {
    public void flush() throws XMLStreamException;
    public void close() throws XMLStreamException;
    public void add(XMLEvent e) throws XMLStreamException;
    // ... other methods not shown.
}

```

Instances of <tt>XMLEventWriter</tt> are created by an instance of <tt>XMLOutputFactory</tt>. Stream events are added iteratively, and an event cannot be modified after it has been added to an event writer instance.

<a name="bnbfh" id="bnbfh"></a>

### Attributes, Escaping Characters, Binding Prefixes

StAX implementations are required to buffer the last <tt>StartElement</tt> until an event other than <tt>Attribute</tt> or <tt>Namespace</tt> is added or encountered in the stream. This means that when you add an <tt>Attribute</tt> or a <tt>Namespace</tt> to a stream, it is appended the current <tt>StartElement</tt> event.

You can use the <tt>Characters</tt> method to escape characters like <tt>&amp;</tt>, <tt>&lt;</tt>, <tt>&gt;</tt>, and <tt>"</tt>.

The <tt>setPrefix(...)</tt> method can be used to explicitly bind a prefix for use during output, and the <tt>getPrefix(...)</tt> method can be used to get the current prefix. Note that by default, <tt>XMLEventWriter</tt> adds namespace bindings to its internal namespace map. Prefixes go out of scope after the corresponding <tt>EndElement</tt> for the event in which they are bound.
