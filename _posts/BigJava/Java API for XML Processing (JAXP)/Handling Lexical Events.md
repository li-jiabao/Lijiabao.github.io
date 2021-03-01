
# Handling Lexical Events

At this point, you have digested many XML concepts, including DTDs and external entities. You have also learned your way around the SAX parser. The remainder of this lesson covers advanced topics that you will need to understand only if you are writing SAX-based applications. If your primary goal is to write DOM-based applications, you can skip ahead to 
[Document Object Model](../dom/index.html).

You saw earlier that if you are writing text out as XML, you need to know whether you are in a CDATA section. If you are, then angle brackets (&lt;) and ampersands (&amp;) should be output unchanged. But if you are not in a CDATA section, they should be replaced by the predefined entities <tt>&amp;lt;</tt> and <tt>&amp;amp;</tt>. But how do you know whether you are processing a CDATA section?

Then again, if you are filtering XML in some way, you want to pass comments along. Normally the parser ignores comments. How can you get comments so that you can echo them?

This section answers those questions. It shows you how to use <tt>org.xml.sax.ext.LexicalHandler</tt> to identify comments, CDATA sections, and references to parsed entities.

Comments, CDATA tags, and references to parsed entities constitute lexical information-that is, information that concerns the text of the XML itself, rather than the XML's information content. Most applications, of course, are concerned only with the content of an XML document. Such applications will not use the <tt>LexicalEventListener</tt> API. But applications that output XML text will find it invaluable.

**Note -** Lexical event handling is an optional parser feature. Parser implementations are not required to support it. (The reference implementation does so.) This discussion assumes that your parser does so.

## <a name="gcymx" id="gcymx"></a>How the <tt>LexicalHandler</tt> Works

To be informed when the SAX parser sees lexical information, you configure the <tt>XmlReader</tt> that underlies the parser with a <tt>LexicalHandler</tt>. The <tt>LexicalHandler</tt> interface defines the following event-handling methods.

Passes comments to the application.

Tells when a CDATA section is starting and ending, which tells your application what kind of characters to expect the next time <tt>characters()</tt> is called.

Gives the name of a parsed entity.

Tells when a DTD is being processed, and identifies it.

To activate the Lexical Handler, your application must extend <tt>DefaultHandler</tt> and implement the <tt>LexicalHandler</tt> interface. Then, you must configure your <tt>XMLReader</tt> instance that the parser delegates to, and configure it to send lexical events to your lexical handler, as shown below.

```

// ...

SAXParser saxParser = factory.newSAXParser();
XMLReader xmlReader = saxParser.getXMLReader();
xmlReader.setProperty("http://xml.org/sax/properties/lexical-handler",
                      handler); 
// ...

```

Here, you configure the <tt>XMLReader</tt> using the <tt>setProperty()</tt> method defined in the <tt>XMLReader</tt> class. The property name, defined as part of the SAX standard, is the URN, <tt>http://xml.org/sax/properties/lexical-handler</tt>.

Finally, add something like the following code to define the appropriate methods that will implement the interface.

```

// ...

public void warning(SAXParseException err) {
    // ...
}

public void comment(char[] ch, int start, int length) throws SAXException {
    // ...   
}

public void startCDATA() throws SAXException {
    // ...
}

public void endCDATA() throws SAXException {
    // ...
}

public void startEntity(String name) throws SAXException {
    // ...
}

public void endEntity(String name) throws SAXException {
    // ...
}

public void startDTD(String name, String publicId, String systemId)
    throws SAXException {
    // ...
}

public void endDTD() throws SAXException {
    // ...
}

private void echoText() {
    // ...
}

// ...

```

This code will transform your parsing application into a lexical handler. All that remains to be done is to give each of these new methods an action to perform.
