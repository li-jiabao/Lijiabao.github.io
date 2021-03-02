
# Overview of the Packages

The SAX and DOM APIs are defined by the XML-DEV group and by the W3C, respectively. The libraries that define those APIs are as follows:

<li>
<tt>javax.xml.parsers</tt>: The JAXP APIs, which provide a common interface for different vendors' SAX and DOM parsers.
</li>
<li>
<tt>org.w3c.dom</tt>: Defines the <tt>Document</tt> class (a DOM) as well as classes for all the components of a DOM.
</li>
<li>
<tt>org.xml.sax</tt>: Defines the basic SAX APIs.
</li>
<li>
<tt>javax.xml.transform</tt>: Defines the XSLT APIs that let you transform XML into other forms.
</li>
<li>
<tt>javax.xml.stream</tt>: Provides StAX-specific transformation APIs.
</li>

The Simple API for XML (SAX) is the event-driven, serial-access mechanism that does element-by-element processing. The API for this level reads and writes XML to a data repository or the web. For server-side and high-performance applications, you will want to fully understand this level. But for many applications, a minimal understanding will suffice.

The DOM API is generally an easier API to use. It provides a familiar tree structure of objects. You can use the DOM API to manipulate the hierarchy of application objects it encapsulates. The DOM API is ideal for interactive applications because the entire object model is present in memory, where it can be accessed and manipulated by the user.

On the other hand, constructing the DOM requires reading the entire XML structure and holding the object tree in memory, so it is much more CPU- and memory-intensive. For that reason, the SAX API tends to be preferred for server-side applications and data filters that do not require an in-memory representation of the data.

The XSLT APIs defined in <tt>javax.xml.transform</tt> let you write XML data to a file or convert it into other forms. As shown in the XSLT section of this tutorial, you can even use it in conjunction with the SAX APIs to convert legacy data to XML.

Finally, the StAX APIs defined in <tt>javax.xml.stream</tt> provide a streaming Java technology-based, event-driven, pull-parsing API for reading and writing XML documents. StAX offers a simpler programming model than SAX and more efficient memory management than DOM.
