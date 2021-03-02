
# StAX API

<a name="indexterm-13" id="indexterm-13"></a>

The StAX API exposes methods for iterative, event-based processing of XML documents. XML documents are treated as a filtered series of events, and infoset states can be stored in a procedural fashion. Moreover, unlike SAX, the StAX API is bidirectional, enabling both reading and writing of XML documents.

The StAX API is really two distinct API sets: a **cursor** API and an **iterator** API. These two API sets are explained in greater detail later in this lesson, but their main features are briefly described below.

<a name="bnbed" id="bnbed"></a>

## Cursor API

<a name="indexterm-14" id="indexterm-14"></a><a name="indexterm-15" id="indexterm-15"></a>

As the name implies, the StAX **cursor** API represents a cursor with which you can walk an XML document from beginning to end. This cursor can point to one thing at a time, and always moves forward, never backward, usually one infoset element at a time.

The two main cursor interfaces are <tt>XMLStreamReader</tt> and <tt>XMLStreamWriter</tt>. <tt>XMLStreamReader</tt> includes accessor methods for all possible information retrievable from the XML Information model, including document encoding, element names, attributes, namespaces, text nodes, start tags, comments, processing instructions, document boundaries, and so forth; for example:

```

public interface XMLStreamReader {
    public int next() throws XMLStreamException;
    public boolean hasNext() throws XMLStreamException;

    public String getText();
    public String getLocalName();
    public String getNamespaceURI();
    // ... other methods not shown
}

```

You can call methods on <tt>XMLStreamReader</tt>, such as <tt>getText</tt> and <tt>getName</tt>, to get data at the current cursor location. <tt>XMLStreamWriter</tt> provides methods that correspond to <tt>StartElement</tt> and <tt>EndElement</tt> event types; for example:

```

public interface XMLStreamWriter {
    public void writeStartElement(String localName) throws XMLStreamException;
    public void writeEndElement() throws XMLStreamException;
    public void writeCharacters(String text) throws XMLStreamException;
    // ... other methods not shown
}

```

The cursor API mirrors SAX in many ways. For example, methods are available for directly accessing string and character information, and integer indexes can be used to access attribute and namespace information. As with SAX, the cursor API methods return XML information as strings, which minimizes object allocation requirements.

<a name="bnbee" id="bnbee"></a>

## Iterator API

<a name="indexterm-16" id="indexterm-16"></a><a name="indexterm-17" id="indexterm-17"></a>

The StAX **iterator** API represents an XML document stream as a set of discrete event objects. These events are pulled by the application and provided by the parser in the order in which they are read in the source XML document.

The base iterator interface is called <tt>XMLEvent</tt>, and there are subinterfaces for each event type listed in [ XMLEvent table](#indexterm-18). The primary parser interface for reading iterator events is <tt>XMLEventReader</tt>, and the primary interface for writing iterator events is <tt>XMLEventWriter</tt>. The <tt>XMLEventReader</tt> interface contains five methods, the most important of which is <tt>nextEvent</tt>, which returns the next event in an XML stream. <tt>XMLEventReader</tt> implements <tt>java.util.Iterator</tt>, which means that returns from <tt>XMLEventReader</tt> can be cached or passed into routines that can work with the standard Java Iterator; for example:

```

public interface XMLEventReader extends Iterator {
    public XMLEvent nextEvent() throws XMLStreamException;
    public boolean hasNext();
    public XMLEvent peek() throws XMLStreamException;
    // ...
}

```

Similarly, on the output side of the iterator API, you have:

```

public interface XMLEventWriter {
    public void flush() throws XMLStreamException;
    public void close() throws XMLStreamException;
    public void add(XMLEvent e) throws XMLStreamException;
    public void add(Attribute attribute) throws XMLStreamException;
    // ...
}

```

<a name="bnbef" id="bnbef"></a>

### Iterator Event Types

<a name="indexterm-18" id="indexterm-18"></a>
<th id="h1" align="left" valign="top" scope="col">Event Type</th><th id="h2" align="left" valign="top" scope="col">Description</th>

Description
<td headers="h1" align="left" valign="top" scope="row"><tt>StartDocument</tt></td><td headers="h2" align="left" valign="top" scope="row">Reports the beginning of a set of XML events, including encoding, XML version, and standalone properties.</td>

Reports the beginning of a set of XML events, including encoding, XML version, and standalone properties.
<td headers="h1" align="left" valign="top" scope="row"><tt>StartElement</tt></td><td headers="h2" align="left" valign="top" scope="row">Reports the start of an element, including any attributes and namespace declarations; also provides access to the prefix, namespace URI, and local name of the start tag.</td>

Reports the start of an element, including any attributes and namespace declarations; also provides access to the prefix, namespace URI, and local name of the start tag.
<td headers="h1" align="left" valign="top" scope="row"><tt>EndElement</tt></td><td headers="h2" align="left" valign="top" scope="row">Reports the end tag of an element. Namespaces that have gone out of scope can be recalled here if they have been explicitly set on their corresponding <tt>StartElement</tt>.</td>

Reports the end tag of an element. Namespaces that have gone out of scope can be recalled here if they have been explicitly set on their corresponding <tt>StartElement</tt>.
<td headers="h1" align="left" valign="top" scope="row"><tt>Characters</tt></td><td headers="h2" align="left" valign="top" scope="row">Corresponds to XML <tt>CData</tt> sections and <tt>CharacterData</tt> entities. Note that ignorable white space and significant white space are also reported as <tt>Character</tt> events.</td>

Corresponds to XML <tt>CData</tt> sections and <tt>CharacterData</tt> entities. Note that ignorable white space and significant white space are also reported as <tt>Character</tt> events.
<td headers="h1" align="left" valign="top" scope="row"><tt>EntityReference</tt></td><td headers="h2" align="left" valign="top" scope="row">Character entities can be reported as discrete events, which an application developer can then choose to resolve or pass through unresolved. By default, entities are resolved. Alternatively, if you do not want to report the entity as an event, replacement text can be substituted and reported as <tt>Characters</tt>.</td>

Character entities can be reported as discrete events, which an application developer can then choose to resolve or pass through unresolved. By default, entities are resolved. Alternatively, if you do not want to report the entity as an event, replacement text can be substituted and reported as <tt>Characters</tt>.
<td headers="h1" align="left" valign="top" scope="row"><tt>ProcessingInstruction</tt></td><td headers="h2" align="left" valign="top" scope="row">Reports the target and data for an underlying processing instruction.</td>

Reports the target and data for an underlying processing instruction.
<td headers="h1" align="left" valign="top" scope="row"><tt>Comment</tt></td><td headers="h2" align="left" valign="top" scope="row">Returns the text of a comment.</td>

Returns the text of a comment.
<td headers="h1" align="left" valign="top" scope="row"><tt>EndDocument</tt></td><td headers="h2" align="left" valign="top" scope="row">Reports the end of a set of XML events.</td>

Reports the end of a set of XML events.
<td headers="h1" align="left" valign="top" scope="row"><tt>DTD</tt></td><td headers="h2" align="left" valign="top" scope="row">Reports as <tt>java.lang.String</tt> information about the DTD, if any, associated with the stream, and provides a method for returning custom objects found in the DTD.</td>

Reports as <tt>java.lang.String</tt> information about the DTD, if any, associated with the stream, and provides a method for returning custom objects found in the DTD.
<td headers="h1" align="left" valign="top" scope="row"><tt>Attribute</tt></td><td headers="h2" align="left" valign="top" scope="row">Attributes are generally reported as part of a <tt>StartElement</tt> event. However, there are times when it is desirable to return an attribute as a standalone <tt>Attribute</tt> event; for example, when a namespace is returned as the result of an <tt>XQuery</tt> or <tt>XPath</tt> expression.</td>

Attributes are generally reported as part of a <tt>StartElement</tt> event. However, there are times when it is desirable to return an attribute as a standalone <tt>Attribute</tt> event; for example, when a namespace is returned as the result of an <tt>XQuery</tt> or <tt>XPath</tt> expression.
<td headers="h1" align="left" valign="top" scope="row"><tt>Namespace</tt></td><td headers="h2" align="left" valign="top" scope="row">As with attributes, namespaces are usually reported as part of a <tt>StartElement</tt>, but there are times when it is desirable to report a namespace as a discrete <tt>Namespace</tt> event.</td>

As with attributes, namespaces are usually reported as part of a <tt>StartElement</tt>, but there are times when it is desirable to report a namespace as a discrete <tt>Namespace</tt> event.

Note that the <tt>DTD</tt>, <tt>EntityDeclaration</tt>, <tt>EntityReference</tt>, <tt>NotationDeclaration</tt>, and <tt>ProcessingInstruction</tt> events are only created if the document being processed contains a DTD.

<a name="bnbeh" id="bnbeh"></a>

### Example of Event Mapping

<a name="indexterm-19" id="indexterm-19"></a><a name="indexterm-20" id="indexterm-20"></a>

As an example of how the event iterator API maps an XML stream, consider the following XML document:

```

&lt;?xml version="1.0"?&gt;
&lt;BookCatalogue xmlns="http://www.publishing.org"&gt;
    &lt;Book&gt;
        &lt;Title&gt;Yogasana Vijnana: the Science of Yoga&lt;/Title&gt;
        &lt;ISBN&gt;81-40-34319-4&lt;/ISBN&gt;
        &lt;Cost currency="INR"&gt;11.50&lt;/Cost&gt;
    &lt;/Book&gt;
&lt;/BookCatalogue&gt;

```

This document would be parsed into eighteen primary and secondary events, as shown in the following table. Note that secondary events, shown in curly braces (<tt>{}</tt>), are typically accessed from a primary event rather than directly.

<a name="bnbei" id="bnbei"></a>
<th id="h101" align="left" valign="top" scope="col">#</th><th id="h102" align="left" valign="top" scope="col">Element/Attribute</th><th id="h103" align="left" valign="top" scope="col">Event</th>

Element/Attribute
<td headers="h101" align="left" valign="top" scope="row">1</td><td headers="h102" align="left" valign="top" scope="row">`version="1.0"`</td><td headers="h103" align="left" valign="top" scope="row"><tt>StartDocument</tt></td>

<tt>StartDocument</tt>
<td headers="h101" align="left" valign="top" scope="row">2</td><td headers="h102" align="left" valign="top" scope="row">`isCData = false data = "\n" IsWhiteSpace = true`</td><td headers="h103" align="left" valign="top" scope="row"><tt>Characters</tt></td>

<tt>Characters</tt>
<td headers="h101" align="left" valign="top" scope="row">3</td><td headers="h102" align="left" valign="top" scope="row">`qname = BookCatalogue:http://www.publishing.org attributes = null namespaces = {BookCatalogue" -&gt; http://www.publishing.org"}`</td><td headers="h103" align="left" valign="top" scope="row"><tt>StartElement</tt></td>

<tt>StartElement</tt>
<td headers="h101" align="left" valign="top" scope="row">4</td><td headers="h102" align="left" valign="top" scope="row">`qname = Book attributes = null namespaces = null`</td><td headers="h103" align="left" valign="top" scope="row"><tt>StartElement</tt></td>

<tt>StartElement</tt>
<td headers="h101" align="left" valign="top" scope="row">5</td><td headers="h102" align="left" valign="top" scope="row">`qname = Title attributes = null namespaces = null`</td><td headers="h103" align="left" valign="top" scope="row"><tt>StartElement</tt></td>

<tt>StartElement</tt>
<td headers="h101" align="left" valign="top" scope="row">6</td><td headers="h102" align="left" valign="top" scope="row">`isCData = false data = "Yogasana Vijnana: the Science of Yoga\n\t" IsWhiteSpace = false`</td><td headers="h103" align="left" valign="top" scope="row"><tt>Characters</tt></td>

<tt>Characters</tt>
<td headers="h101" align="left" valign="top" scope="row">7</td><td headers="h102" align="left" valign="top" scope="row">`qname = Title namespaces = null`</td><td headers="h103" align="left" valign="top" scope="row"><tt>EndElement</tt></td>

<tt>EndElement</tt>
<td headers="h101" align="left" valign="top" scope="row">8</td><td headers="h102" align="left" valign="top" scope="row">`qname = ISBN attributes = null namespaces = null`</td><td headers="h103" align="left" valign="top" scope="row"><tt>StartElement</tt></td>

<tt>StartElement</tt>
<td headers="h101" align="left" valign="top" scope="row">9</td><td headers="h102" align="left" valign="top" scope="row">`isCData = false data = "81-40-34319-4\n\t" IsWhiteSpace = false`</td><td headers="h103" align="left" valign="top" scope="row"><tt>Characters</tt></td>

<tt>Characters</tt>
<td headers="h101" align="left" valign="top" scope="row">10</td><td headers="h102" align="left" valign="top" scope="row">`qname = ISBN namespaces = null`</td><td headers="h103" align="left" valign="top" scope="row"><tt>EndElement</tt></td>

<tt>EndElement</tt>
<td headers="h101" align="left" valign="top" scope="row">11</td><td headers="h102" align="left" valign="top" scope="row">`qname = Cost attributes = {"currency" -&gt; INR} namespaces = null`</td><td headers="h103" align="left" valign="top" scope="row"><tt>StartElement</tt></td>

<tt>StartElement</tt>
<td headers="h101" align="left" valign="top" scope="row">12</td><td headers="h102" align="left" valign="top" scope="row">`isCData = false data = "11.50\n\t" IsWhiteSpace = false`</td><td headers="h103" align="left" valign="top" scope="row"><tt>Characters</tt></td>

<tt>Characters</tt>
<td headers="h101" align="left" valign="top" scope="row">13</td><td headers="h102" align="left" valign="top" scope="row">`qname = Cost namespaces = null`</td><td headers="h103" align="left" valign="top" scope="row"><tt>EndElement</tt></td>

<tt>EndElement</tt>
<td headers="h101" align="left" valign="top" scope="row">14</td><td headers="h102" align="left" valign="top" scope="row">`isCData = false data = "\n" IsWhiteSpace = true`</td><td headers="h103" align="left" valign="top" scope="row"><tt>Characters</tt></td>

<tt>Characters</tt>
<td headers="h101" align="left" valign="top" scope="row">15</td><td headers="h102" align="left" valign="top" scope="row">`qname = Book namespaces = null`</td><td headers="h103" align="left" valign="top" scope="row"><tt>EndElement</tt></td>

<tt>EndElement</tt>
<td headers="h101" align="left" valign="top" scope="row">16</td><td headers="h102" align="left" valign="top" scope="row">`isCData = false data = "\n" IsWhiteSpace = true`</td><td headers="h103" align="left" valign="top" scope="row"><tt>Characters</tt></td>

<tt>Characters</tt>
<td headers="h101" align="left" valign="top" scope="row">17</td><td headers="h102" align="left" valign="top" scope="row">`qname = BookCatalogue:http://www.publishing.org namespaces = {BookCatalogue" -&gt; http://www.publishing.org"}`</td><td headers="h103" align="left" valign="top" scope="row"><tt>EndElement</tt></td>

<tt>EndElement</tt>
<td headers="h101" align="left" valign="top" scope="row">18</td><td headers="h102" align="left" valign="top" scope="row"></td><td headers="h103" align="left" valign="top" scope="row"><tt>EndDocument</tt></td>

<tt>EndDocument</tt>

There are several important things to note in this example:

<li>
The events are created in the order in which the corresponding XML elements are encountered in the document, including nesting of elements, opening and closing of elements, attribute order, document start and document end, and so forth.
</li>
<li>
As with proper XML syntax, all container elements have corresponding start and end events; for example, every <tt>StartElement</tt> has a corresponding <tt>EndElement</tt>, even for empty elements.
</li>
<li>
<tt>Attribute</tt> events are treated as secondary events, and are accessed from their corresponding <tt>StartElement</tt> event.
</li>
<li>
Similar to <tt>Attribute</tt> events, <tt>Namespace</tt> events are treated as secondary, but appear twice and are accessible twice in the event stream, first from their corresponding <tt>StartElement</tt> and then from their corresponding <tt>EndElement</tt>.
</li>
<li>
<tt>Character</tt> events are specified for all elements, even if those elements have no character data. Similarly, <tt>Character</tt> events can be split across events.
</li>
<li>
The StAX parser maintains a namespace stack, which holds information about all XML namespaces defined for the current element and its ancestors. The namespace stack, which is exposed through the <tt>javax.xml.namespace.NamespaceContext</tt> interface, can be accessed by namespace prefix or URI.
</li>

<a name="bnbej" id="bnbej"></a>

## Choosing between Cursor and Iterator APIs

<a name="indexterm-21" id="indexterm-21"></a>

It is reasonable to ask at this point, &#8220;What API should I choose? Should I create instances of <tt>XMLStreamReader</tt> or <tt>XMLEventReader</tt>? Why are there two kinds of APIs anyway?&#8221;

<a name="bnbek" id="bnbek"></a>

### Development Goals

The authors of the StAX specification targeted three types of developers:

<li>
**Library and infrastructure developers**: Create application servers, JAXM, JAXB, JAX-RPC and similar implementations; need highly efficient, low-level APIs with minimal extensibility requirements.
</li>
<li>
**Java ME developers**: Need small, simple, pull-parsing libraries, and have minimal extensibility needs.
</li>
<li>
**Java Platform, Enterprise Edition (Java EE) and Java Platform, Standard Edition (Java SE) developers**: Need clean, efficient pull-parsing libraries, plus need the flexibility to both read and write XML streams, create new event types, and extend XML document elements and attributes.
</li>

Given these wide-ranging development categories, the StAX authors felt it was more useful to define two small, efficient APIs rather than overloading one larger and necessarily more complex API.

<a name="bnbel" id="bnbel"></a>

### Comparing Cursor and Iterator APIs

Before choosing between the cursor and iterator APIs, you should note a few things that you can do with the iterator API that you cannot do with the cursor API:

<li>
Objects created from the <tt>XMLEvent</tt> subclasses are immutable, and can be used in arrays, lists, and maps, and can be passed through your applications even after the parser has moved on to subsequent events.
</li>
<li>
You can create subtypes of <tt>XMLEvent</tt> that are either completely new information items or extensions of existing items but with additional methods.
</li>
<li>
You can add and remove events from an XML event stream in much simpler ways than with the cursor API.
</li>

Similarly, keep some general recommendations in mind when making your choice:

<li>
If you are programming for a particularly memory-constrained environment, like Java ME, you can make smaller, more efficient code with the cursor API.
</li>
<li>
If performance is your highest priority&#8212;for example, when creating low-level libraries or infrastructure&#8212;the cursor API is more efficient.
</li>
<li>
If you want to create XML processing pipelines, use the iterator API.
</li>
<li>
If you want to modify the event stream, use the iterator API.
</li>
<li>
If you want your application to be able to handle pluggable processing of the event stream, use the iterator API.
</li>
<li>
In general, if you do not have a strong preference one way or the other, using the iterator API is recommended because it is more flexible and extensible, thereby &#8220;future-proofing&#8221; your applications.
</li>
