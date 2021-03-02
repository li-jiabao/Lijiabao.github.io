
# Streaming API for XML APIs

StAX is the latest API in the JAXP family, and provides an alternative to SAX, DOM, TrAX, and DOM for developers looking to do high-performance stream filtering, processing, and modification, particularly with low memory and limited extensibility requirements.

To summarize, StAX provides a standard, bidirectional **pull parser** interface for streaming XML processing, offering a simpler programming model than SAX and more efficient memory management than DOM. StAX enables developers to parse and modify XML streams as events, and to extend XML information models to allow application-specific additions. More detailed comparisons of StAX with several alternative APIs are provided in 
[Streaming API for XML](../stax/index.html), in 
[Comparing StAX to Other JAXP APIs](../stax/why.html#bnbea).

<a name="gfonu" id="gfonu"></a>

## StAX Packages

The StAX APIs are defined in the packages shown in [Table&#160;1-4](#gfoor).

<a name="gfoor" id="gfoor"></a>Table&#160;1-4 StAX Packages
<th id="h1" align="left" valign="top" scope="col">Package</th><th id="h2" align="left" valign="top" scope="col">Description</th>

Description
<td headers="h1" align="left" valign="top" scope="row"><tt>javax.xml.stream</tt></td><td headers="h2" align="left" valign="top" scope="row">Defines the <tt>XMLStreamReader</tt> interface, which is used to iterate over the elements of an XML document. The <tt>XMLStreamWriter</tt> interface specifies how the XML should be written.</td>

Defines the <tt>XMLStreamReader</tt> interface, which is used to iterate over the elements of an XML document. The <tt>XMLStreamWriter</tt> interface specifies how the XML should be written.
<td headers="h1" align="left" valign="top" scope="row"><tt>javax.xml.transform.stax</tt></td><td headers="h2" align="left" valign="top" scope="row">Provides StAX-specific transformation APIs.</td>

Provides StAX-specific transformation APIs.
