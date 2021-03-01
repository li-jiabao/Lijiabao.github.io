
# Extensible Stylesheet Language Transformations APIs

The following [Figure&#160;](#gceys) shows the XSLT APIs in action.

<a name="gceys" id="gceys"></a>Figure&#160; XSLT APIs

<img alt="XSLT APIs" src="../../figures/jaxp/intro/jaxpintro-xsltApi.gif" width="330" height="300" />

A <tt>TransformerFactory</tt> object is instantiated and used to create a <tt>Transformer</tt>. The source object is the input to the transformation process. A source object can be created from a SAX reader, from a DOM, or from an input stream.

Similarly, the result object is the result of the transformation process. That object can be a SAX event handler, a DOM, or an output stream.

When the transformer is created, it can be created from a set of transformation instructions, in which case the specified transformations are carried out. If it is created without any specific instructions, then the transformer object simply copies the source to the result.

## <a name="gcezs" id="gcezs"></a>XSLT Packages

The XSLT APIs are defined in the packages shown in [Table&#160;](#gcfbf).

<a name="gcfbf" id="gcfbf"></a>Table&#160; XSLT Packages
<th id="h1" align="left" valign="top" scope="col">Package</th><th id="h2" align="left" valign="top" scope="col">Description</th>

Description
<td headers="h1" align="left" valign="top" scope="row"><tt>javax.xml.transform</tt></td><td headers="h2" align="left" valign="top" scope="row">Defines the <tt>TransformerFactory</tt> and <tt>Transformer</tt> classes, which you use to get an object capable of doing transformations. After creating a transformer object, you invoke its <tt>transform()</tt> method, providing it with an input (source) and output (result).</td>

Defines the <tt>TransformerFactory</tt> and <tt>Transformer</tt> classes, which you use to get an object capable of doing transformations. After creating a transformer object, you invoke its <tt>transform()</tt> method, providing it with an input (source) and output (result).
<td headers="h1" align="left" valign="top" scope="row"><tt>javax.xml.transform.dom</tt></td><td headers="h2" align="left" valign="top" scope="row">Classes to create input (source) and output (result) objects from a DOM.</td>

Classes to create input (source) and output (result) objects from a DOM.
<td headers="h1" align="left" valign="top" scope="row"><tt>javax.xml.transform.sax</tt></td><td headers="h2" align="left" valign="top" scope="row">Classes to create input (source) objects from a SAX parser and output (result) objects from a SAX event handler.</td>

Classes to create input (source) objects from a SAX parser and output (result) objects from a SAX event handler.
<td headers="h1" align="left" valign="top" scope="row"><tt>javax.xml.transform.stream</tt></td><td headers="h2" align="left" valign="top" scope="row">Classes to create input (source) objects and output (result) objects from an I/O stream.</td>

Classes to create input (source) objects and output (result) objects from an I/O stream.
