
# Why StAX?

The StAX project was spearheaded by BEA with support from Sun Microsystems, and the
[JSR 173 specification](http://jcp.org/en/jsr/detail?id=173) passed the Java Community Process final approval ballot in March, 2004. The primary goal of the StAX API is to give &#8220;parsing control to the programmer by exposing a simple iterator based API. This allows the programmer to ask for the next event (pull the event) and allows state to be stored in procedural fashion.&#8221; StAX was created to address limitations in the two most prevalent parsing APIs, SAX and DOM.<a name="bnbdx" id="bnbdx"></a>

## Streaming versus DOM

<a name="indexterm-5" id="indexterm-5"></a><a name="indexterm-6" id="indexterm-6"></a>

Generally speaking, there are two programming models for working with XML infosets: **streaming** and the **document object model** (DOM).

The DOM model involves creating in-memory objects representing an entire document tree and the complete infoset state for an XML document. Once in memory, DOM trees can be navigated freely and parsed arbitrarily, and as such provide maximum flexibility for developers. However, the cost of this flexibility is a potentially large memory footprint and significant processor requirements, because the entire representation of the document must be held in memory as objects for the duration of the document processing. This may not be an issue when working with small documents, but memory and processor requirements can escalate quickly with document size.

Streaming refers to a programming model in which XML infosets are transmitted and parsed serially at application runtime, often in real time, and often from dynamic sources whose contents are not precisely known beforehand. Moreover, stream-based parsers can start generating output immediately, and infoset elements can be discarded and garbage collected immediately after they are used. While providing a smaller memory footprint, reduced processor requirements, and higher performance in certain situations, the primary trade-off with stream processing is that you can only see the infoset state at one location at a time in the document. You are essentially limited to the &#8220;cardboard tube&#8221; view of a document, the implication being that you need to know what processing you want to do before reading the XML document.

Streaming models for XML processing are particularly useful when your application has strict memory limitations, as with a cellphone running the Java Platform, Micro Edition (Java ME platform), or when your application needs to process several requests simultaneously, as with an application server. In fact, it can be argued that the majority of XML business logic can benefit from stream processing, and does not require the in-memory maintenance of entire DOM trees.

<a name="bnbdy" id="bnbdy"></a>

## Pull Parsing versus Push Parsing

<a name="indexterm-7" id="indexterm-7"></a><a name="indexterm-8" id="indexterm-8"></a>

Streaming **pull parsing** refers to a programming model in which a client application calls methods on an XML parsing library when it needs to interact with an XML infoset&#8212;that is, the client only gets (pulls) XML data when it explicitly asks for it.

Streaming **push parsing** refers to a programming model in which an XML parser sends (pushes) XML data to the client as the parser encounters elements in an XML infoset&#8212;that is, the parser sends the data whether or not the client is ready to use it at that time.

Pull parsing provides several advantages over push parsing when working with XML streams:

<li>
With pull parsing, the client controls the application thread, and can call methods on the parser when needed. By contrast, with push processing, the parser controls the application thread, and the client can only accept invocations from the parser.
</li>
<li>
Pull parsing libraries can be much smaller and the client code to interact with those libraries much simpler than with push libraries, even for more complex documents.
</li>
<li>
Pull clients can read multiple documents at one time with a single thread.
</li>
<li>
A StAX pull parser can filter XML documents such that elements unnecessary to the client can be ignored, and it can support XML views of non-XML data.
</li>

<a name="bnbdz" id="bnbdz"></a>

## StAX Use Cases

<a name="indexterm-9" id="indexterm-9"></a>

The StAX specification defines a number of use cases for the API:

<li>
<a name="indexterm-10" id="indexterm-10"></a>Data binding
<ul>
<li>
Unmarshalling an XML document
</li>
<li>
Marshalling an XML document
</li>
<li>
Parallel document processing
</li>
<li>
Wireless communication
</li>

<a name="indexterm-11" id="indexterm-11"></a>Simple Object Access Protocol (SOAP) message processing

<li>
Parsing simple predictable structures
</li>
<li>
Parsing graph representations with forward references
</li>
<li>
Parsing Web Services Description Language (WSDL)
</li>

Virtual data sources

<li>
Viewing as XML data stored in databases
</li>
<li>
Viewing data in Java objects created by XML data binding
</li>
<li>
Navigating a DOM tree as a stream of events
</li>

Parsing specific XML vocabularies

Pipelined XML processing

A complete discussion of all these use cases is beyond the scope of this lesson. Please refer to the StAX specification for further information.

<a name="bnbea" id="bnbea"></a>

## Comparing StAX to Other JAXP APIs

As an API in the JAXP family, StAX can be compared, among other APIs, to SAX, TrAX, and JDOM. Of the latter two, StAX is not as powerful or flexible as TrAX or JDOM, but neither does it require as much memory or processor load to be useful, and StAX can, in many cases, outperform the DOM-based APIs. The same arguments outlined above, weighing the cost/benefits of the DOM model versus the streaming model, apply here.

With this in mind, the closest comparisons can be made between StAX and SAX, and it is here that StAX offers features that are beneficial in many cases; some of these include:

<li>
StAX-enabled clients are generally easier to code than SAX clients. While it can be argued that SAX parsers are marginally easier to write, StAX parser code can be smaller and the code necessary for the client to interact with the parser simpler.
</li>
<li>
StAX is a bidirectional API, meaning that it can both read and write XML documents. SAX is read only, so another API is needed if you want to write XML documents.
</li>
<li>
SAX is a push API, whereas StAX is pull. The trade-offs between push and pull APIs outlined above apply here.
</li>

The following table summarizes the comparative features of StAX, SAX, DOM, and TrAX. (The table is adapted from 
[Does StAX Belong in Your XML Toolbox?](http://www.developer.com/xml/article.php/3397691) by Jeff Ryan).

<a name="indexterm-12" id="indexterm-12"></a>
<th id="h1" align="left" valign="top">Feature</th><th id="h2" align="left" valign="top">StAX</th><th id="h3" align="left" valign="top">SAX</th><th id="h4" align="left" valign="top">DOM</th><th id="h5" align="left" valign="top">TrAX</th>

StAX

DOM
<td headers="h1" align="left" valign="top">API Type</td><td headers="h2" align="left" valign="top">Pull, streaming</td><td headers="h3" align="left" valign="top">Push, streaming</td><td headers="h4" align="left" valign="top">In memory tree</td><td headers="h5" align="left" valign="top">XSLT Rule</td>

Pull, streaming

In memory tree
<td headers="h1" align="left" valign="top">Ease of Use</td><td headers="h2" align="left" valign="top">High</td><td headers="h3" align="left" valign="top">Medium</td><td headers="h4" align="left" valign="top">High</td><td headers="h5" align="left" valign="top">Medium</td>

High

High
<td headers="h1" align="left" valign="top">XPath Capability</td><td headers="h2" align="left" valign="top">No</td><td headers="h3" align="left" valign="top">No</td><td headers="h4" align="left" valign="top">Yes</td><td headers="h5" align="left" valign="top">Yes</td>

No

Yes
<td headers="h1" align="left" valign="top">CPU and Memory Efficiency</td><td headers="h2" align="left" valign="top">Good</td><td headers="h3" align="left" valign="top">Good</td><td headers="h4" align="left" valign="top">Varies</td><td headers="h5" align="left" valign="top">Varies</td>

Good

Varies
<td headers="h1" align="left" valign="top">Forward Only</td><td headers="h2" align="left" valign="top">Yes</td><td headers="h3" align="left" valign="top">Yes</td><td headers="h4" align="left" valign="top">No</td><td headers="h5" align="left" valign="top">No</td>

Yes

No
<td headers="h1" align="left" valign="top">Read XML</td><td headers="h2" align="left" valign="top">Yes</td><td headers="h3" align="left" valign="top">Yes</td><td headers="h4" align="left" valign="top">Yes</td><td headers="h5" align="left" valign="top">Yes</td>

Yes

Yes
<td headers="h1" align="left" valign="top">Write XML</td><td headers="h2" align="left" valign="top">Yes</td><td headers="h3" align="left" valign="top">No</td><td headers="h4" align="left" valign="top">Yes</td><td headers="h5" align="left" valign="top">Yes</td>

Yes

Yes
<td headers="h1" align="left" valign="top">Create, Read, Update, Delete</td><td headers="h2" align="left" valign="top">No</td><td headers="h3" align="left" valign="top">No</td><td headers="h4" align="left" valign="top">Yes</td><td headers="h5" align="left" valign="top">No</td>

No

Yes
