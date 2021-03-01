
# Example Code

<a name="indexterm-48" id="indexterm-48"></a><a name="indexterm-49" id="indexterm-49"></a>

This section steps through the example StAX code included in the JAXP reference implementation bundle. All example directories used in this section are located in the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/stax</tt> directory.

The topics covered in this section are as follows:

<li>
[Example Code Organization](#bnbfm)
</li>
<li>
[Example XML Document](#bnbfn)
</li>
<li>
[Cursor Example](#bnbfo)
</li>
<li>
[Cursor-to-Event Example](#bnbft)
</li>
<li>
[Event Example](#bnbfz)
</li>
<li>
[Filter Example](#bnbgh)
</li>
<li>
[Read-and-Write Example](#bnbgq)
</li>
<li>
[Writer Example](#bnbgx)
</li>

<a name="bnbfm" id="bnbfm"></a>

## Example Code Organization

The *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/stax</tt> directory contains the six StAX example directories:

<li>
**Cursor example**: The <tt>cursor</tt> directory contains <tt>CursorParse.java</tt>, which illustrates how to use the <tt>XMLStreamReader</tt> (cursor) API to read an XML file.
</li>
<li>
**Cursor-to-Event example**: The <tt>cursor2event</tt> directory contains <tt>CursorApproachEventObject.java</tt>, which illustrates how an application can get information as an <tt>XMLEvent</tt> object when using the cursor API.
</li>
<li>
**Event example**: The <tt>event</tt> directory contains <tt>EventParse.java</tt>, which illustrates how to use the <tt>XMLEventReader</tt> (event iterator) API to read an XML file.
</li>
<li>
**Filter example**: The <tt>filter</tt> directory contains <tt>MyStreamFilter.java</tt>, which illustrates how to use the StAX Stream Filter APIs. In this example, the filter accepts only <tt>StartElement</tt> and <tt>EndElement</tt> events, and filters out the remainder of the events.
</li>
<li>
**Read-and-Write example**: The <tt>readnwrite</tt> directory contains <tt>EventProducerConsumer.java</tt>, which illustrates how the StAX producer/consumer mechanism can be used to simultaneously read and write XML streams.
</li>
<li>
**Writer example**: The <tt>writer</tt> directory contains <tt>CursorWriter.java</tt>, which illustrates how to use <tt>XMLStreamWriter</tt> to write an XML file programatically.
</li>

All the StAX examples except for the Writer example use an example XML document, <tt>BookCatalog.xml</tt>.

<a name="bnbfn" id="bnbfn"></a>

## Example XML Document

The example XML document, <tt>BookCatalog.xml</tt>, used by most of the StAX example classes, is a simple book catalog based on the common <tt>BookCatalogue</tt> namespace. The contents of <tt>BookCatalog.xml</tt> are listed below:

```

&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;BookCatalogue xmlns="http://www.publishing.org"&gt;
&lt;Book&gt;
    &lt;Title&gt;Yogasana Vijnana: the Science of Yoga&lt;/Title&gt;
    &lt;author&gt;Dhirendra Brahmachari&lt;/Author&gt;
    &lt;Date&gt;1966&lt;/Date&gt;
    &lt;ISBN&gt;81-40-34319-4&lt;/ISBN&gt;
    &lt;Publisher&gt;Dhirendra Yoga Publications&lt;/Publisher&gt;
    &lt;Cost currency="INR"&gt;11.50&lt;/Cost&gt;
&lt;/Book&gt;
&lt;Book&gt;
    &lt;Title&gt;The First and Last Freedom&lt;/Title&gt;
    &lt;Author&gt;J. Krishnamurti&lt;/Author&gt;
    &lt;Date&gt;1954&lt;/Date&gt;
    &lt;ISBN&gt;0-06-064831-7&lt;/ISBN&gt;
    &lt;Publisher&gt;Harper &amp;amp; Row&lt;/Publisher&gt;
    &lt;Cost currency="USD"&gt;2.95&lt;/Cost&gt;
&lt;/Book&gt;
&lt;/BookCatalogue&gt;

```

<a name="bnbfo" id="bnbfo"></a>

## Cursor Example

<a name="indexterm-50" id="indexterm-50"></a><a name="indexterm-51" id="indexterm-51"></a>

Located in the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/stax/cursor/</tt> directory, <tt>CursorParse.java</tt> demonstrates using the StAX cursor API to read an XML document. In the Cursor example, the application instructs the parser to read the next event in the XML input stream by calling <tt>next()</tt>.

Note that <tt>next()</tt> just returns an integer constant corresponding to an underlying event where the parser is positioned. The application needs to call the relevant function to get more information related to the underlying event.

You can imagine this approach as a virtual cursor moving across the XML input stream. There are various accessor methods which can be called when that virtual cursor is at a particular event.

<a name="bnbfp" id="bnbfp"></a>

### Stepping through Events

In this example, the client application pulls the next event in the XML stream by calling the <tt>next</tt> method on the parser; for example:

```

try {
    for (int i = 0 ; i &lt; count ; i++) {
        // pass the file name.. all relative entity
        // references will be resolved against this 
        // as base URI.
        XMLStreamReader xmlr = xmlif.createXMLStreamReader(filename,
                                   new FileInputStream(filename));

        // when XMLStreamReader is created, 
        // it is positioned at START_DOCUMENT event.
        int eventType = xmlr.getEventType();
        printEventType(eventType);
        printStartDocument(xmlr);

        // check if there are more events 
        // in the input stream
        while(xmlr.hasNext()) {
            eventType = xmlr.next();
            printEventType(eventType);

            // these functions print the information 
            // about the particular event by calling 
            // the relevant function
            printStartElement(xmlr);
            printEndElement(xmlr);
            printText(xmlr);
            printPIData(xmlr);
            printComment(xmlr);
        }
    }
}

```

Note that <tt>next</tt> just returns an integer constant corresponding to the event underlying the current cursor location. The application calls the relevant function to get more information related to the underlying event. There are various accessor methods which can be called when the cursor is at a particular event.

<a name="bnbfq" id="bnbfq"></a>

### Returning String Representations

Because the <tt>next</tt> method only returns integers corresponding to underlying event types, you typically need to map these integers to string representations of the events; for example:

```

public final static String getEventTypeString(int eventType) {
    switch (eventType) {
        case XMLEvent.START_ELEMENT:
            return "START_ELEMENT";

        case XMLEvent.END_ELEMENT:
            return "END_ELEMENT";

        case XMLEvent.PROCESSING_INSTRUCTION:
            return "PROCESSING_INSTRUCTION";

        case XMLEvent.CHARACTERS:
            return "CHARACTERS";

        case XMLEvent.COMMENT:
            return "COMMENT";

        case XMLEvent.START_DOCUMENT:
            return "START_DOCUMENT";

        case XMLEvent.END_DOCUMENT:
            return "END_DOCUMENT";

        case XMLEvent.ENTITY_REFERENCE:
            return "ENTITY_REFERENCE";

        case XMLEvent.ATTRIBUTE:
            return "ATTRIBUTE";

        case XMLEvent.DTD:
            return "DTD";

        case XMLEvent.CDATA:
            return "CDATA";

        case XMLEvent.SPACE:
            return "SPACE";
    }
    return "UNKNOWN_EVENT_TYPE , " + eventType;
}

```

<a name="gfyfg" id="gfyfg"></a>

### To Run the Cursor Example

<li>**To compile and run the cursor example, in a terminal window, go to the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/</tt> directory and type the following:**
<pre><code>
javac stax/cursor/*.java
</code></pre>
</li>
<li>**Run the <tt>CursorParse</tt> sample on the <tt>BookCatalogue.xml</tt> file, with the following command.**
<tt>CursorParse</tt> will print out each element of the <tt>BookCatalogue.xml</tt> file.
</li>

```

java stax/event/CursorParse stax/data/BookCatalogue.xml

```

<a name="bnbft" id="bnbft"></a>

## Cursor-to-Event Example

<a name="indexterm-52" id="indexterm-52"></a><a name="indexterm-53" id="indexterm-53"></a>

Located in the *tut-install*<tt>/javaeetutorial5/examples/stax/cursor2event/</tt> directory, <tt>CursorApproachEventObject.java</tt> demonstrates how to get information returned by an <tt>XMLEvent</tt> object even when using the cursor API.

The idea here is that the cursor API&#8217;s <tt>XMLStreamReader</tt> returns integer constants corresponding to particular events, while the event iterator API&#8217;s <tt>XMLEventReader</tt> returns immutable and persistent event objects. <tt>XMLStreamReader</tt> is more efficient, but <tt>XMLEventReader</tt> is easier to use, because all the information related to a particular event is encapsulated in a returned <tt>XMLEvent</tt> object. However, the disadvantage of the event approach is the extra overhead of creating objects for every event, which consumes both time and memory.

With this mind, <tt>XMLEventAllocator</tt> can be used to get event information as an <tt>XMLEvent</tt> object, even when using the cursor API.

<a name="bnbfu" id="bnbfu"></a>

### Instantiating an XMLEventAllocator

<a name="indexterm-54" id="indexterm-54"></a>

The first step is to create a new <tt>XMLInputFactory</tt> and instantiate an <tt>XMLEventAllocator</tt>:

```

XMLInputFactory xmlif = XMLInputFactory.newInstance();
System.out.println("FACTORY: " + xmlif);
xmlif.setEventAllocator(new XMLEventAllocatorImpl());
allocator = xmlif.getEventAllocator();
XMLStreamReader xmlr = xmlif.createXMLStreamReader(filename,
                           new FileInputStream(filename));

```

<a name="bnbfv" id="bnbfv"></a>

### Creating an Event Iterator

The next step is to create an event iterator:

```

int eventType = xmlr.getEventType();

while (xmlr.hasNext()) {
    eventType = xmlr.next();
    // Get all "Book" elements as XMLEvent object
    if (eventType == XMLStreamConstants.START_ELEMENT 
        &amp;&amp; xmlr.getLocalName().equals("Book")) {
        // get immutable XMLEvent
        StartElement event = getXMLEvent(xmlr).asStartElement();
        System.out.println ("EVENT: " + event.toString());
    }
}

```

<a name="bnbfw" id="bnbfw"></a>

### Creating the Allocator Method

The final step is to create the <tt>XMLEventAllocator</tt> method:

```

private static XMLEvent getXMLEvent(XMLStreamReader reader)
    throws XMLStreamException {
    return allocator.allocate(reader);
}

```

<a name="gfyeb" id="gfyeb"></a>

### To Run the Cursor-to-Event Example

<li>**To compile and run the cursor&#8212;to&#8212;event example, in a terminal window, go to the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/</tt> directory and type the following:**
<pre><code>
javac -classpath ../lib/jaxp-ri.jar stax/cursor2event/*.java
</code></pre>
</li>
<li>**Run the <tt>CursorApproachEventObject</tt> sample on the <tt>BookCatalogue.xml</tt> file, with the following command.**
<pre><code>
java stax/cursor2event/CursorApproachEventObject stax/data/BookCatalogue.xml
</code></pre>
<tt>CursorApproachEventObject</tt> will print out the list of events defined by the <tt>BookCatalogue.xml</tt> file.
</li>

<a name="bnbfz" id="bnbfz"></a>

## Event Example

<a name="indexterm-55" id="indexterm-55"></a><a name="indexterm-56" id="indexterm-56"></a>

Located in the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/stax/event/</tt> directory, <tt>EventParse.java</tt> demonstrates how to use the StAX event API to read an XML document.

<a name="bnbga" id="bnbga"></a>

### Creating an Input Factory

The first step is to create a new instance of <tt>XMLInputFactory</tt>:

```

XMLInputFactory factory = XMLInputFactory.newInstance();
System.out.println("FACTORY: " + factory);

```

<a name="bnbgb" id="bnbgb"></a>

### Creating an Event Reader

The next step is to create an instance of <tt>XMLEventReader</tt>:

```

XMLEventReader r = factory.createXMLEventReader
                       (filename, new FileInputStream(filename));

```

<a name="bnbgc" id="bnbgc"></a>

### Creating an Event Iterator

The third step is to create an event iterator:

```

XMLEventReader r = factory.createXMLEventReader
                       (filename, new FileInputStream(filename));
while (r.hasNext()) {
    XMLEvent e = r.nextEvent();
    System.out.println(e.toString());
}

```

<a name="bnbgd" id="bnbgd"></a>

### Getting the Event Stream

The final step is to get the underlying event stream:

```

public final static String getEventTypeString(int eventType) {
    switch (eventType) {
        case XMLEvent.START_ELEMENT:
            return "START_ELEMENT";

        case XMLEvent.END_ELEMENT:
            return "END_ELEMENT";

        case XMLEvent.PROCESSING_INSTRUCTION:
            return "PROCESSING_INSTRUCTION";

        case XMLEvent.CHARACTERS:
            return "CHARACTERS";

        case XMLEvent.COMMENT:
            return "COMMENT";

        case XMLEvent.START_DOCUMENT:
            return "START_DOCUMENT";

        case XMLEvent.END_DOCUMENT:
            return "END_DOCUMENT";

        case XMLEvent.ENTITY_REFERENCE:
            return "ENTITY_REFERENCE";

        case XMLEvent.ATTRIBUTE:
            return "ATTRIBUTE";

        case XMLEvent.DTD:
            return "DTD";

        case XMLEvent.CDATA:
            return "CDATA";

        case XMLEvent.SPACE:
            return "SPACE";
    }
    return "UNKNOWN_EVENT_TYPE," + eventType;
}

```

<a name="bnbge" id="bnbge"></a>

### Returning the Output

When you run the Event example, the <tt>EventParse</tt> class is compiled, and the XML stream is parsed as events and returned to <tt>STDOUT</tt>. For example, an instance of the <tt>Author</tt> element is returned as:

```

&lt;[&#8217;http://www.publishing.org&#8217;]::Author&gt;
    Dhirendra Brahmachari
&lt;/[&#8217;http://www.publishing.org&#8217;]::Author&gt;

```

Note in this example that the event comprises an opening and closing tag, both of which include the namespace. The content of the element is returned as a string within the tags.

Similarly, an instance of the <tt>Cost</tt> element is returned as:

```

&lt;[&#8217;http://www.publishing.org&#8217;]::Cost currency=&#8217;INR&#8217;&gt;
    11.50
&lt;/[&#8217;http://www.publishing.org&#8217;]::Cost

```

In this case, the <tt>currency</tt> attribute and value are returned in the opening tag for the event.

<a name="gfyek" id="gfyek"></a>

### To Run the Event Example

<li>**To compile and run the event example, in a terminal window, go to the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/</tt> directory and type the following:**
<pre><code>
javac -classpath ../lib/jaxp-ri.jar stax/event/*.java
</code></pre>
</li>
<li>**Run the <tt>EventParse</tt> sample on the <tt>BookCatalogue.xml</tt> file, with the following command.**
<pre><code>
java stax/event/EventParse stax/data/BookCatalogue.xml
</code></pre>
<tt>EventParse</tt> will print out the data from all the elements defined by the <tt>BookCatalogue.xml</tt> file.
</li>

<a name="bnbgh" id="bnbgh"></a>

## Filter Example

<a name="indexterm-57" id="indexterm-57"></a><a name="indexterm-58" id="indexterm-58"></a>

Located in the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/stax/filter/</tt> directory, <tt>MyStreamFilter.java</tt> demonstrates how to use the StAX stream filter API to filter out events not needed by your application. In this example, the parser filters out all events except <tt>StartElement</tt> and <tt>EndElement</tt>.

<a name="bnbgi" id="bnbgi"></a>

### Implementing the StreamFilter Class

The <tt>MyStreamFilter</tt> class implements <tt>javax.xml.stream.StreamFilter</tt>:

```

public class MyStreamFilter implements javax.xml.stream.StreamFilter {
    // ...
}

```

<a name="bnbgj" id="bnbgj"></a>

### Creating an Input Factory

The next step is to create an instance of <tt>XMLInputFactory</tt>. In this case, various properties are also set on the factory:

```

XMLInputFactory xmlif = null ;

try {
    xmlif = XMLInputFactory.newInstance();
    xmlif.setProperty(
        XMLInputFactory.IS_REPLACING_ENTITY_REFERENCES,
        Boolean.TRUE);

    xmlif.setProperty(
        XMLInputFactory.IS_SUPPORTING_EXTERNAL_ENTITIES,
        Boolean.FALSE);

    xmlif.setProperty(
        XMLInputFactory.IS_NAMESPACE_AWARE,
        Boolean.TRUE);

    xmlif.setProperty(
        XMLInputFactory.IS_COALESCING,
        Boolean.TRUE);
} 
catch (Exception ex) {
    ex.printStackTrace();
}

System.out.println("FACTORY: " + xmlif);
System.out.println("filename = "+ filename);

```

<a name="bnbgk" id="bnbgk"></a>

### Creating the Filter

The next step is to instantiate a file input stream and create the stream filter:

```

FileInputStream fis = new FileInputStream(filename);
XMLStreamReader xmlr = xmlif.createFilteredReader(
                           xmlif.createXMLStreamReader(fis), 
                           new MyStreamFilter());

int eventType = xmlr.getEventType();
printEventType(eventType);

while (xmlr.hasNext()) {
    eventType = xmlr.next();
    printEventType(eventType);
    printName(xmlr,eventType);
    printText(xmlr);

    if (xmlr.isStartElement()) {
        printAttributes(xmlr);
    }
    printPIData(xmlr);
    System.out.println("-----------------------");
}

```

<a name="bnbgl" id="bnbgl"></a>

### Capturing the Event Stream

The next step is to capture the event stream. This is done in basically the same way as in the Event example.

<a name="bnbgm" id="bnbgm"></a>

### Filtering the Stream

The final step is to filter the stream:

```

public boolean accept(XMLStreamReader reader) {
    if (!reader.isStartElement() &amp;&amp; !reader.isEndElement())
        return false;
    else
        return true;
}

```

<a name="bnbgn" id="bnbgn"></a>

### Returning the Output

When you run the Filter example, the <tt>MyStreamFilter</tt> class is compiled, and the XML stream is parsed as events and returned to <tt>STDOUT</tt>. For example, an <tt>Author</tt> event is returned as follows:

```

EVENT TYPE(1):START_ELEMENT
HAS NAME: Author
HAS NO TEXT
HAS NO ATTRIBUTES
-----------------------------
EVENT TYPE(2):END_ELEMENT
HAS NAME: Author
HAS NO TEXT
-----------------------------

```

Similarly, a <tt>Cost</tt> event is returned as follows:

```

EVENT TYPE(1):START_ELEMENT
HAS NAME: Cost
HAS NO TEXT

HAS ATTRIBUTES:
 ATTRIBUTE-PREFIX:
 ATTRIBUTE-NAMESP: null
ATTRIBUTE-NAME:   currency
ATTRIBUTE-VALUE: USD
ATTRIBUTE-TYPE:  CDATA

-----------------------------
EVENT TYPE(2):END_ELEMENT
HAS NAME: Cost
HAS NO TEXT
-----------------------------

```

See 
[Iterator API](api.html#bnbee) and 
[Reading XML Streams](using.html#bnbew) for a more detailed discussion of StAX event parsing.

<a name="gfygs" id="gfygs"></a>

### To Run the Filter Example

<li>**To compile and run the Filter example, in a terminal window, go to the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/</tt> directory and type the following:**
<pre><code>
javac -classpath ../lib/jaxp-ri.jar stax/filter/*.java
</code></pre>
</li>
<li>**Run the <tt>MyStreamFilter</tt> sample on the <tt>BookCatalogue.xml</tt> file, with the following command. This example requires the <tt>java.endorsed.dirs</tt> system property to be set, to point to the <tt>samples/lib</tt> directory.**
<pre><code>
java -Djava.endorsed.dirs=../lib stax/filter/MyStreamFilter -f stax/data/BookCatalogue.xml
</code></pre>
<tt>MyStreamFilter</tt> will print out the events defined by the <tt>BookCatalogue.xml</tt> file as an XML stream.
</li>

<a name="bnbgq" id="bnbgq"></a>

## Read-and-Write Example

<a name="indexterm-59" id="indexterm-59"></a><a name="indexterm-60" id="indexterm-60"></a>

Located in the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/stax/readnwrite/</tt> directory, <tt>EventProducerConsumer.java</tt> demonstrates how to use a StAX parser simultaneously as both a producer and a consumer.

The StAX <tt>XMLEventWriter</tt> API extends from the <tt>XMLEventConsumer</tt> interface, and is referred to as an **event consumer**. By contrast, <tt>XMLEventReader</tt> is an **event producer**. StAX supports simultaneous reading and writing, such that it is possible to read from one XML stream sequentially and simultaneously write to another stream.

The Read-and-Write example shows how the StAX producer/consumer mechanism can be used to read and write simultaneously. This example also shows how a stream can be modified and how new events can be added dynamically and then written to a different stream.

<a name="bnbgr" id="bnbgr"></a>

### Creating an Event Producer/Consumer

The first step is to instantiate an event factory and then create an instance of an event producer/consumer:

```

XMLEventFactory m_eventFactory = XMLEventFactory.newInstance();

public EventProducerConsumer() {
    // ...
    try {
        EventProducerConsumer ms = new EventProducerConsumer();
    
        XMLEventReader reader = XMLInputFactory.newInstance().
        createXMLEventReader(new java.io.FileInputStream(args[0]));
    
        XMLEventWriter writer = 
            XMLOutputFactory.newInstance().createXMLEventWriter(System.out);
    }
    // ...
}

```

<a name="bnbgs" id="bnbgs"></a>

### Creating an Iterator

The next step is to create an iterator to parse the stream:

```

while (reader.hasNext()) {
    XMLEvent event = (XMLEvent)reader.next();
    if (event.getEventType() == event.CHARACTERS) {
        writer.add(ms.getNewCharactersEvent(event.asCharacters()));
    }
    else {
        writer.add(event);
    }
}
writer.flush();

```

<a name="bnbgt" id="bnbgt"></a>

### Creating a Writer

The final step is to create a stream writer in the form of a new <tt>Character</tt> event:

```

Characters getNewCharactersEvent(Characters event) {
    if (event.getData().equalsIgnoreCase("Name1")) {
        return m_eventFactory.createCharacters(
                   Calendar.getInstance().getTime().toString());
    }
    // else return the same event
    else {
        return event;
    }
}

```

<a name="bnbgu" id="bnbgu"></a>

### Returning the Output

When you run the Read-and-Write example, the <tt>EventProducerConsumer</tt> class is compiled, and the XML stream is parsed as events and written back to <tt>STDOUT</tt>. The output is the contents of the <tt>BookCatalog.xml</tt> file described in [Example XML Document](#bnbfn).

<a name="gfyfk" id="gfyfk"></a>

### To Run the Read-and-Write Example

<li>**To compile and run the Read&#8212;and&#8212;Write example, in a terminal window, go to the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/</tt> directory and type the following:**
<pre><code>
javac -classpath ../lib/jaxp-ri.jar stax/readnwrite/*.java
</code></pre>
</li>
<li>**Run the <tt>EventProducerConsumer</tt> sample on the <tt>BookCatalogue.xml</tt> file, with the following command.**
<pre><code>
java stax/readnwrite/EventProducerConsumer stax/data/BookCatalogue.xml
</code></pre>
<tt>EventProducerConsumer</tt> will print out the content of the <tt>BookCatalogue.xml</tt> file.
</li>

<a name="bnbgx" id="bnbgx"></a>

## Writer Example

<a name="indexterm-61" id="indexterm-61"></a><a name="indexterm-62" id="indexterm-62"></a>

Located in the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/stax/writer/</tt> directory, <tt>CursorWriter.java</tt> demonstrates how to use the StAX cursor API to write an XML stream.

<a name="bnbgy" id="bnbgy"></a>

### Creating the Output Factory

The first step is to create an instance of <tt>XMLOutputFactory</tt>:

```

XMLOutputFactory xof =  XMLOutputFactory.newInstance();

```

<a name="bnbgz" id="bnbgz"></a>

### Creating a Stream Writer

The next step is to create an instance of <tt>XMLStreamWriter</tt>:

```

XMLStreamWriter xtw = null;

```

<a name="bnbha" id="bnbha"></a>

### Writing the Stream

The final step is to write the XML stream. Note that the stream is flushed and closed after the final <tt>EndDocument</tt> is written:

```

xtw = xof.createXMLStreamWriter(new FileWriter(fileName));

xtw.writeComment("all elements here are explicitly in the HTML namespace");
xtw.writeStartDocument("utf-8","1.0");
xtw.setPrefix("html", "http://www.w3.org/TR/REC-html40");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40","html");
xtw.writeNamespace("html", "http://www.w3.org/TR/REC-html40");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "head");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "title");
xtw.writeCharacters("Frobnostication");
xtw.writeEndElement();
xtw.writeEndElement();

xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "body");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "p");
xtw.writeCharacters("Moved to");
xtw.writeStartElement("http://www.w3.org/TR/REC-html40", "a");
xtw.writeAttribute("href","http://frob.com");
xtw.writeCharacters("here");
xtw.writeEndElement();
xtw.writeEndElement();
xtw.writeEndElement();
xtw.writeEndElement();
xtw.writeEndDocument();

xtw.flush();
xtw.close();

```

<a name="bnbhb" id="bnbhb"></a>

### Returning the Output

When you run the Writer example, the <tt>CursorWriter</tt> class is compiled, and the XML stream is parsed as events and written to a file named <tt>dist/CursorWriter-Output</tt>:

```

&lt;!--all elements here are explicitly in the HTML namespace--&gt;
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;html:html xmlns:html="http://www.w3.org/TR/REC-html40"&gt;
&lt;html:head&gt;
&lt;html:title&gt;Frobnostication&lt;/html:title&gt;&lt;/html:head&gt;
&lt;html:body&gt;
&lt;html:p&gt;Moved to &lt;html:a href="http://frob.com"&gt;here&lt;/html:a&gt;
&lt;/html:p&gt;
&lt;/html:body&gt;
&lt;/html:html&gt;

```

In the actual <tt>dist/CursorWriter-Output</tt> file, this stream is written without any line breaks; the breaks have been added here to make the listing easier to read. In this example, as with the object stream in the Event example, the namespace prefix is added to both the opening and closing HTML tags. Adding this prefix is not required by the StAX specification, but it is good practice when the final scope of the output stream is not definitively known.

<a name="gfyez" id="gfyez"></a>

### To Run the Writer Example

<li>**To compile and run the Writer example, in a terminal window, go to the *INSTALL_DIR*<tt>/jaxp-</tt>*version*<tt>/samples/</tt> directory and type the following:**
<pre><code>
javac -classpath \
    ../lib/jaxp-ri.jar stax/writer/*.java
</code></pre>
</li>
<li>**Run the <tt>CursorWriter</tt> sample, specifying the name of the file the output should be written to.**
<pre><code>
java stax/writer/CursorWriter -f *output_file*
</code></pre>
<tt>CursorWriter</tt> will create an output file of the appropriate name, which contains the data shown in [Returning the Output](#bnbhb).
</li>
