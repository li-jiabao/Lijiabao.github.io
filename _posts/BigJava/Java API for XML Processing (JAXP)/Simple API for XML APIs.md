
# Simple API for XML APIs

The basic outline of the SAX parsing APIs is shown in [Figure&#160;1-1](#gcezl). To start the process, an instance of the <tt>SAXParserFactory</tt> class is used to generate an instance of the parser.

<a name="gcezl" id="gcezl"></a>Figure&#160;1-1 SAX APIs

<img src="../../figures/jaxp/intro/jaxpintro-saxApi.gif" alt="The SAX APIs" width="319" height="272" />


<center><img src="../../figures/jaxp/jaxpintro-saxApi.gif" width="319" height="272" align="bottom" alt="The SAX APIs" /></center>

The parser wraps a <tt>SAXReader</tt> object. When the parser's <tt>parse()</tt> method is invoked, the reader invokes one of several callback methods implemented in the application. Those methods are defined by the interfaces <tt>ContentHandler</tt>, <tt>ErrorHandler</tt>, <tt>DTDHandler</tt>, and <tt>EntityResolver</tt>.

Here is a summary of the key SAX APIs:

A <tt>SAXParserFactory</tt> object creates an instance of the parser determined by the system property, <tt>javax.xml.parsers.SAXParserFactory</tt>.

The <tt>SAXParser</tt> interface defines several kinds of <tt>parse()</tt> methods. In general, you pass an XML data source and a <tt>DefaultHandler</tt> object to the parser, which processes the XML and invokes the appropriate methods in the handler object.

The <tt>SAXParser</tt> wraps a <tt>SAXReader</tt>. Typically, you do not care about that, but every once in a while you need to get hold of it using <tt>SAXParser</tt>'s <tt>getXMLReader()</tt> so that you can configure it. It is the <tt>SAXReader</tt> that carries on the conversation with the SAX event handlers you define.

Not shown in the diagram, a <tt>DefaultHandler</tt> implements the <tt>ContentHandler</tt>, <tt>ErrorHandler</tt>, <tt>DTDHandler</tt>, and <tt>EntityResolver</tt> interfaces (with null methods), so you can override only the ones you are interested in.

Methods such as <tt>startDocument</tt>, <tt>endDocument</tt>, <tt>startElement</tt>, and <tt>endElement</tt> are invoked when an XML tag is recognized. This interface also defines the methods <tt>characters()</tt> and <tt>processingInstruction()</tt>, which are invoked when the parser encounters the text in an XML element or an inline processing instruction, respectively.

Methods <tt>error()</tt>, <tt>fatalError()</tt>, and <tt>warning()</tt> are invoked in response to various parsing errors. The default error handler throws an exception for fatal errors and ignores other errors (including validation errors). This is one reason you need to know something about the SAX parser, even if you are using the DOM. Sometimes, the application may be able to recover from a validation error. Other times, it may need to generate an exception. To ensure the correct handling, you will need to supply your own error handler to the parser.

Defines methods you will generally never be called upon to use. Used when processing a DTD to recognize and act on declarations for an unparsed entity.

The <tt>resolveEntity</tt> method is invoked when the parser must identify data identified by a URI. In most cases, a URI is simply a URL, which specifies the location of a document, but in some cases the document may be identified by a URN - a public identifier, or name, that is unique in the web space. The public identifier may be specified in addition to the URL. The <tt>EntityResolver</tt> can then use the public identifier instead of the URL to find the document-for example, to access a local copy of the document if one exists.

A typical application implements most of the <tt>ContentHandler</tt> methods, at a minimum. Because the default implementations of the interfaces ignore all inputs except for fatal errors, a robust implementation may also want to implement the <tt>ErrorHandler</tt> methods.

<a name="gceyt" id="gceyt"></a>

## SAX Packages

The SAX parser is defined in the packages listed in the following [Table&#160;](#gceyy).

<a name="gceyy" id="gceyy"></a>Table&#160; SAX Packages
<th id="h1" align="left" valign="top" scope="col">Packages</th><th id="h2" align="left" valign="top" scope="col">Description</th>

Description
<td headers="h1" align="left" valign="top" scope="row"><tt>org.xml.sax</tt></td><td headers="h2" align="left" valign="top" scope="row">Defines the SAX interfaces. The name <tt>org.xml</tt> is the package prefix that was settled on by the group that defined the SAX API.</td>

Defines the SAX interfaces. The name <tt>org.xml</tt> is the package prefix that was settled on by the group that defined the SAX API.
<td headers="h1" align="left" valign="top" scope="row"><tt>org.xml.sax.ext</tt></td><td headers="h2" align="left" valign="top" scope="row">Defines SAX extensions that are used for doing more sophisticated SAX processing-for example, to process a document type definition (DTD) or to see the detailed syntax for a file.</td>

Defines SAX extensions that are used for doing more sophisticated SAX processing-for example, to process a document type definition (DTD) or to see the detailed syntax for a file.
<td headers="h1" align="left" valign="top" scope="row"><tt>org.xml.sax.helpers</tt></td><td headers="h2" align="left" valign="top" scope="row">Contains helper classes that make it easier to use SAX-for example, by defining a default handler that has null methods for all the interfaces, so that you only need to override the ones you actually want to implement.</td>

Contains helper classes that make it easier to use SAX-for example, by defining a default handler that has null methods for all the interfaces, so that you only need to override the ones you actually want to implement.
<td headers="h1" align="left" valign="top" scope="row"><tt>javax.xml.parsers</tt></td><td headers="h2" align="left" valign="top" scope="row">Defines the <tt>SAXParserFactory</tt> class, which returns the <tt>SAXParser</tt>. Also defines exception classes for reporting errors.</td>

Defines the <tt>SAXParserFactory</tt> class, which returns the <tt>SAXParser</tt>. Also defines exception classes for reporting errors.
