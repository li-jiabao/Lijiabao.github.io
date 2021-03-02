
# Document Object Model APIs

[The following figure&#160;](#gceza) shows the DOM APIs in action.

<a name="gceza" id="gceza"></a>Figure&#160; DOM APIs

<img src="../../figures/jaxp/intro/jaxpintro-domApi.gif" alt="DOM APIs" width="393" height="255" />

You use the <tt>javax.xml.parsers.DocumentBuilderFactory</tt> class to get a <tt>DocumentBuilder</tt> instance, and you use that instance to produce a <tt>Document</tt> object that conforms to the DOM specification. The builder you get, in fact, is determined by the system property <tt>javax.xml.parsers.DocumentBuilderFactory</tt>, which selects the factory implementation that is used to produce the builder. (The platform's default value can be overridden from the command line.)

You can also use the <tt>DocumentBuilder</tt> <tt>newDocument()</tt> method to create an empty <tt>Document</tt> that implements the <tt>org.w3c.dom.Document</tt> interface. Alternatively, you can use one of the builder's parse methods to create a <tt>Document</tt> from existing XML data. The result is a DOM tree like that shown in above [Figure&#160;](#gceza).

**Note -** Although they are called objects, the entries in the DOM tree are actually fairly low-level data structures. For example, consider this structure: <tt>&lt;color&gt;blue&lt;/color&gt;</tt>. There is an element node for the color tag, and under that there is a text node that contains the data, blue! This issue will be explored at length in the DOM lesson of this tutorial, but developers who are expecting objects are usually surprised to find that invoking <tt>getNodeValue()</tt> on the element node returns nothing. For a truly object-oriented tree, see the JDOM API at 
[http://www.jdom.org](http://www.jdom.org).

## <a name="gcezn" id="gcezn"></a>DOM Packages

The Document Object Model implementation is defined in the packages listed in the following [Table&#160;](#gcezo).

<a name="gcezo" id="gcezo"></a>

Table&#160; DOM Packages
<th id="h1" align="left" valign="top" scope="col">Package</th><th id="h2" align="left" valign="top" scope="col">Description</th>

Description
<td headers="h1" align="left" valign="top" scope="row"><tt>org.w3c.dom</tt></td><td headers="h2" align="left" valign="top" scope="row">Defines the DOM programming interfaces for XML (and, optionally, HTML) documents, as specified by the W3C.</td>

Defines the DOM programming interfaces for XML (and, optionally, HTML) documents, as specified by the W3C.
<td headers="h1" align="left" valign="top" scope="row"><tt>javax.xml.parsers</tt></td><td headers="h2" align="left" valign="top" scope="row">Defines the <tt>DocumentBuilderFactory</tt> class and the <tt>DocumentBuilder</tt> class, which returns an object that implements the W3C Document interface. The factory that is used to create the builder is determined by the <tt>javax.xml.parsers</tt> system property, which can be set from the command line or overridden when invoking the new Instance method. This package also defines the <tt>ParserConfigurationException</tt> class for reporting errors.</td>

Defines the <tt>DocumentBuilderFactory</tt> class and the <tt>DocumentBuilder</tt> class, which returns an object that implements the W3C Document interface. The factory that is used to create the builder is determined by the <tt>javax.xml.parsers</tt> system property, which can be set from the command line or overridden when invoking the new Instance method. This package also defines the <tt>ParserConfigurationException</tt> class for reporting errors.
