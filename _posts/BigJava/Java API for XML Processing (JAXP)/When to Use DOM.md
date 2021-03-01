
# Reading XML Data into a DOM

In this section, you will construct a Document Object Model by reading in an existing XML file.

**Note -** In 
[Extensible Stylesheet Language Transformations](../xslt/index.html), you will see how to write out a DOM as an XML file. (You will also see how to convert an existing data file into XML with relative ease.)

## <a name="gestu" id="gestu"></a>Creating the Program

The Document Object Model provides APIs that let you create, modify, delete, and rearrange nodes. Before you try to create a DOM, it is helpful to understand how a DOM is structured. This series of examples will make DOM internals visible via a sample program called <tt>DOMEcho</tt>, which you will find in the directory <tt>*INSTALL_DIR*/jaxp-*version*/samples/dom</tt> after you have installed the JAXP API.

### <a name="gestx" id="gestx"></a>Create the Skeleton

First, build a simple program to read an XML document into a DOM and then write it back out again.

Start with the normal basic logic for an application, and check to make sure that an argument has been supplied on the command line:

```

public class DOMEcho {

    static final String outputEncoding = "UTF-8";

    private static void usage() {
        // ...
    }

    public static void main(String[] args) throws Exception {
        String filename = null;
    
        for (int i = 0; i &lt; args.length; i++) {
            if (...) { 
                // ...
            } 
            else {
                filename = args[i];
                if (i != args.length - 1) {
                    usage();
                }
            }
        }

        if (filename == null) {
            usage();
        }
    }
}

```

This code performs all the basic set up operations. All output for <tt>DOMEcho</tt> uses UTF-8 encoding. The <tt>usage()</tt> method that is called if no argument is specified simply tells you what arguments <tt>DOMEcho</tt> expects, so the code is not shown here. A <tt>filename</tt> string is also declared, which will be the name of the XML file to be parsed into a DOM by <tt>DOMEcho</tt>.

### <a name="gesun" id="gesun"></a>Import the Required Classes

In this section, all the classes are individually named so you that can see where each class comes from, in case you want to reference the API documentation. In the sample file, the import statements are made with the shorter form, such as <tt>javax.xml.parsers.*</tt>.

These are the JAXP APIs used by <tt>DOMEcho</tt>:

```

package dom;
import javax.xml.parsers.DocumentBuilder; 
import javax.xml.parsers.DocumentBuilderFactory;

```

These classes are for the exceptions that can be thrown when the XML document is parsed:

```

import org.xml.sax.ErrorHandler;
import org.xml.sax.SAXException; 
import org.xml.sax.SAXParseException;
import org.xml.sax.helpers.*

```

These classes read the sample XML file and manage output:

```

import java.io.File;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;

```

Finally, import the W3C definitions for a DOM, DOM exceptions, entities and nodes:

```

import org.w3c.dom.Document;
import org.w3c.dom.DocumentType;
import org.w3c.dom.Entity;
import org.w3c.dom.NamedNodeMap;
import org.w3c.dom.Node;

```

### <a name="gestm" id="gestm"></a>Handle Errors

Next, add the error-handling logic. The most important point is that a JAXP-conformant document builder is required to report SAX exceptions when it has trouble parsing an XML document. The DOM parser does not have to actually use a SAX parser internally, but because the SAX standard is already there, it makes sense to use it for reporting errors. As a result, the error-handling code for DOM applications is very similar to that for SAX applications:

```

private static class MyErrorHandler implements ErrorHandler {
     
    private PrintWriter out;

    MyErrorHandler(PrintWriter out) {
        this.out = out;
    }

    private String getParseExceptionInfo(SAXParseException spe) {
        String systemId = spe.getSystemId();
        if (systemId == null) {
            systemId = "null";
        }

        String info = "URI=" + systemId + " Line=" + spe.getLineNumber() +
                      ": " + spe.getMessage();
        return info;
    }

    public void warning(SAXParseException spe) throws SAXException {
        out.println("Warning: " + getParseExceptionInfo(spe));
    }
        
    public void error(SAXParseException spe) throws SAXException {
        String message = "Error: " + getParseExceptionInfo(spe);
        throw new SAXException(message);
    }

    public void fatalError(SAXParseException spe) throws SAXException {
        String message = "Fatal Error: " + getParseExceptionInfo(spe);
        throw new SAXException(message);
    }
}


```

As you can see, the <tt>DomEcho</tt> class's error handler generates its output using <tt>PrintWriter</tt> instances.

### <a name="geswy" id="geswy"></a>Instantiate the Factory

Next, add the following code to the <tt>main()</tt> method, to obtain an instance of a factory that can give us a document builder.

```

public static void main(String[] args) throws Exception {
    **DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();**

    // ...
}


```

### <a name="geswm" id="geswm"></a>Get a Parser and Parse the File

Now, add the following code to <tt>main()</tt> to get an instance of a builder, and use it to parse the specified file.

```

DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
<b>DocumentBuilder db = dbf.newDocumentBuilder(); 
Document doc = db.parse(new File(filename));</b>


```

The file being parsed is provided by the <tt>filename</tt> variable that was declared at the beginning of the <tt>main()</tt> method, which is passed to <tt>DOMEcho</tt> as an argument when the program is run.

### <a name="geswk" id="geswk"></a>Configuring the Factory

By default, the factory returns a non-validating parser that knows nothing about name spaces. To get a validating parser, or one that understands name spaces (or both), you can configure the factory to set either or both of those options using the following code.

```

public static void main(String[] args) throws Exception {

    String filename = null;
    boolean dtdValidate = false;
    boolean xsdValidate = false;
    String schemaSource = null;
        
    for (int i = 0; i &lt; args.length; i++) {
        if (args[i].equals("-dtd"))  { 
            dtdValidate = true;
        } 
        else if (args[i].equals("-xsd")) {
            xsdValidate = true;
        } 
        else if (args[i].equals("-xsdss")) {
            if (i == args.length - 1) {
                usage();
            }
            xsdValidate = true;
            schemaSource = args[++i];
        }
        else {
            filename = args[i];
            if (i != args.length - 1) {
                usage();
            }
        }
    }

    if (filename == null) {
        usage();
    }

    DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

    dbf.setNamespaceAware(true);
    dbf.setValidating(dtdValidate || xsdValidate);

    // ...

    DocumentBuilder db = dbf.newDocumentBuilder();
    Document doc = db.parse(new File(filename));
}

```

As you can see, command line arguments are set up so that you can inform <tt>DOMEcho</tt> to perform validation against either a DTD or an XML Schema, and the factory is configured to be name space aware and to perform whichever type of validation the user specifies.

**Note -** JAXP-conformant parsers are not required to support all combinations of those options, even though the reference parser does. If you specify an invalid combination of options, the factory generates a <tt>ParserConfigurationException</tt> when you attempt to obtain a parser instance.

More information about how to use name spaces and validation is provided in 
[Validating with XML Schema](validating.html), in which the code that is missing from the above extract will be described.

### <a name="gesxf" id="gesxf"></a>Handling Validation Errors

The default response to a validation error, as dictated by the SAX standard, is to do nothing. The JAXP standard requires throwing SAX exceptions, so you use exactly the same error-handling mechanisms as you use for a SAX application. In particular, you use the <tt>DocumentBuilder</tt> class's <tt>setErrorHandler</tt> method to supply it with an object that implements the SAX <tt>ErrorHandler</tt> interface.

**Note -** <tt>DocumentBuilder</tt> also has a <tt>setEntityResolver</tt> method you can use.

The following code configures the document builder to use the error handler defined in 
[Handle Errors](readingXML.html#gestm).

```

DocumentBuilder db = dbf.newDocumentBuilder();
OutputStreamWriter errorWriter = new OutputStreamWriter(System.err,
                                         outputEncoding);
db.setErrorHandler(new MyErrorHandler (new PrintWriter(errorWriter, true)));
Document doc = db.parse(new File(filename));

```

The code you have seen so far has set up the document builder, and configured it to perform validation upon request. Error handling is also in place. However, <tt>DOMEcho</tt> does not do anything yet. In the next section, you will see how to display the DOM structure and begin to explore it. For example, you will see what entity references and CDATA sections look like in the DOM. And perhaps most importantly, you will see how text nodes (which contain the actual data) reside under element nodes in a DOM.

## <a name="gesxh" id="gesxh"></a>Displaying the DOM Nodes

To create or manipulate a DOM, it helps to have a clear idea of how the nodes in a DOM are structured. This section of the tutorial exposes the internal structure of a DOM, so that you can see what it contains. The <tt>DOMEcho</tt> example does this by echoing the DOM nodes, and then printing them out onscreen, with the appropriate indentation to make the node hierarchy apparent. The specification of these node types can be found in the 
[DOM Level 2 Core Specification](http://www.w3.org/TR/2000/REC-DOM-Level-2-Core-20001113), under the specification for <tt>Node</tt>. [Table&#160;3-1](#gfzpy) below is adapted from that specification.

<a name="gfzpy" id="gfzpy"></a>Table&#160;3-1 Node Types
<th id="h1" align="left" valign="top" scope="col">Node</th><th id="h2" align="left" valign="top" scope="col">nodeName</th><th id="h3" align="left" valign="top" scope="col">nodeValue</th><th id="h4" align="left" valign="top" scope="col">Attributes</th>

nodeName

Attributes
<td headers="h1" align="left" valign="top" scope="row"><tt>Attr</tt></td><td headers="h2" align="left" valign="top" scope="row">Name of attribute</td><td headers="h3" align="left" valign="top" scope="row">Value of attribute</td><td headers="h4" align="left" valign="top" scope="row">null</td>

Name of attribute

null
<td headers="h1" align="left" valign="top" scope="row"><tt>CDATASection</tt></td><td headers="h2" align="left" valign="top" scope="row"><tt>#cdata-section</tt></td><td headers="h3" align="left" valign="top" scope="row">Content of the CDATA section</td><td headers="h4" align="left" valign="top" scope="row">null</td>

<tt>#cdata-section</tt>

null
<td headers="h1" align="left" valign="top" scope="row"><tt>Comment</tt></td><td headers="h2" align="left" valign="top" scope="row"><tt>#comment</tt></td><td headers="h3" align="left" valign="top" scope="row">Content of the comment</td><td headers="h4" align="left" valign="top" scope="row">null</td>

<tt>#comment</tt>

null
<td headers="h1" align="left" valign="top" scope="row"><tt>Document</tt></td><td headers="h2" align="left" valign="top" scope="row"><tt>#document</tt></td><td headers="h3" align="left" valign="top" scope="row">null</td><td headers="h4" align="left" valign="top" scope="row">null</td>

<tt>#document</tt>

null
<td headers="h1" align="left" valign="top" scope="row"><tt>DocumentFragment</tt></td><td headers="h2" align="left" valign="top" scope="row"><tt>#documentFragment</tt></td><td headers="h3" align="left" valign="top" scope="row">null</td><td headers="h4" align="left" valign="top" scope="row">null</td>

<tt>#documentFragment</tt>

null
<td headers="h1" align="left" valign="top" scope="row"><tt>DocumentType</tt></td><td headers="h2" align="left" valign="top" scope="row">Document Type name</td><td headers="h3" align="left" valign="top" scope="row">null</td><td headers="h4" align="left" valign="top" scope="row">null</td>

Document Type name

null
<td headers="h1" align="left" valign="top" scope="row"><tt>Element</tt></td><td headers="h2" align="left" valign="top" scope="row">Tag name</td><td headers="h3" align="left" valign="top" scope="row">null</td><td headers="h4" align="left" valign="top" scope="row">null</td>

Tag name

null
<td headers="h1" align="left" valign="top" scope="row"><tt>Entity</tt></td><td headers="h2" align="left" valign="top" scope="row">Entity name</td><td headers="h3" align="left" valign="top" scope="row">null</td><td headers="h4" align="left" valign="top" scope="row">null</td>

Entity name

null
<td headers="h1" align="left" valign="top" scope="row"><tt>EntityReference</tt></td><td headers="h2" align="left" valign="top" scope="row">Name of entity referenced</td><td headers="h3" align="left" valign="top" scope="row">null</td><td headers="h4" align="left" valign="top" scope="row">null</td>

Name of entity referenced

null
<td headers="h1" align="left" valign="top" scope="row"><tt>Notation</tt></td><td headers="h2" align="left" valign="top" scope="row">Notation name</td><td headers="h3" align="left" valign="top" scope="row">null</td><td headers="h4" align="left" valign="top" scope="row">null</td>

Notation name

null
<td headers="h1" align="left" valign="top" scope="row"><tt>ProcessingInstruction</tt></td><td headers="h2" align="left" valign="top" scope="row">Target</td><td headers="h3" align="left" valign="top" scope="row">Entire content excluding the target</td><td headers="h4" align="left" valign="top" scope="row">null</td>

Target

null
<td headers="h1" align="left" valign="top" scope="row"><tt>Text</tt></td><td headers="h2" align="left" valign="top" scope="row"><tt>#text</tt></td><td headers="h3" align="left" valign="top" scope="row">Content of the text node</td><td headers="h4" align="left" valign="top" scope="row">null</td>

<tt>#text</tt>

null

The information in this table is extremely useful; you will need it when working with a DOM, because all these types are intermixed in a DOM tree.

### <a name="gfzqh" id="gfzqh"></a>Obtaining Node Type Information

The DOM node element type information is obtained by calling the various methods of the <tt>org.w3c.dom.Node</tt> class. The node attributes by exposed by <tt>DOMEcho</tt> are echoed by the following code.

```

private void printlnCommon(Node n) {
    out.print(" nodeName=\"" + n.getNodeName() + "\"");

    String val = n.getNamespaceURI();
    if (val != null) {
        out.print(" uri=\"" + val + "\"");
    }

    val = n.getPrefix();

    if (val != null) {
        out.print(" pre=\"" + val + "\"");
    }

    val = n.getLocalName();
    if (val != null) {
        out.print(" local=\"" + val + "\"");
    }

    val = n.getNodeValue();
    if (val != null) {
        out.print(" nodeValue=");
        if (val.trim().equals("")) {
            // Whitespace
            out.print("[WS]");
        }
        else {
            out.print("\"" + n.getNodeValue() + "\"");
        }
    }
    out.println();
}

```

Every DOM node has at least a type, a name, and a value, which might or might not be empty. In the example above, the <tt>Node</tt> interface's <tt>getNamespaceURI()</tt>, <tt>getPrefix()</tt>, <tt>getLocalName()</tt>, and <tt>getNodeValue()</tt> methods return and print the echoed node's namespace URI, namespace prefix, local qualified name and value. Note that the <tt>trim()</tt> method is called on the value returned by <tt>getNodeValue()</tt> to establish whether the node's value is empty white space and print a message accordingly.

For the full list of <tt>Node</tt> methods and the different information they return, see the API documentation for 
[`Node`](https://docs.oracle.com/javase/8/docs/api/org/w3c/dom/Node.html).

Next, a method is defined to set the indentation for the nodes when they are printed, so that the node hierarchy will be easily visible.

```

private void outputIndentation() {
    for (int i = 0; i &lt; indent; i++) {
        out.print(basicIndent);
    }
}

```

The <tt>basicIndent</tt> constant to define the basic unit of indentation used when <tt>DOMEcho</tt> displays the node tree hierarchy, is defined by adding the following highlighted lines to the <tt>DOMEcho</tt> constructor class.

```

public class DOMEcho {
    static final String outputEncoding = "UTF-8";

    **private PrintWriter out;**
    **private int indent = 0;**
    **private final String basicIndent = " ";**

    DOMEcho(PrintWriter out) {
        this.out = out;
    }
}

```

As was the case with the error handler defined in 
[Handle Errors](readingXML.html#gestm), the <tt>DOMEcho</tt> program will create its output as <tt>PrintWriter</tt> instances.

### <a name="ggdwv" id="ggdwv"></a>Lexical Controls

Lexical information is the information you need to reconstruct the original syntax of an XML document. Preserving lexical information is important in editing applications, where you want to save a document that is an accurate reflection of the original-complete with comments, entity references, and any CDATA sections it may have included at the outset.

Most applications, however, are concerned only with the content of the XML structures. They can afford to ignore comments, and they do not care whether data was coded in a CDATA section or as plain text, or whether it included an entity reference. For such applications, a minimum of lexical information is desirable, because it simplifies the number and kind of DOM nodes that the application must be prepared to examine.

The following <tt>DocumentBuilderFactory</tt> methods give you control over the lexical information you see in the DOM.

To convert <tt>CDATA</tt> nodes to <tt>Text</tt> nodes and append to an adjacent <tt>Text</tt> node (if any).

To expand entity reference nodes.

To ignore comments.

To ignore whitespace that is not a significant part of element content.

The default values for all these properties is false, which preserves all the lexical information necessary to reconstruct the incoming document in its original form. Setting them to true lets you construct the simplest possible DOM so that the application can focus on the data's semantic content without having to worry about lexical syntax details. [Table&#160;3-2](#ggdxy) summarizes the effects of the settings.

<a name="ggdxy" id="ggdxy"></a>Table&#160;3-2 Lexical Control Settings
<th id="h101" align="left" valign="top" scope="col">API</th><th id="h102" align="left" valign="top" scope="col">Preserve Lexical Info</th><th id="h103" align="left" valign="top" scope="col">Focus on Content</th>

Preserve Lexical Info
<td headers="h101" align="left" valign="top" scope="row"><tt>setCoalescing()</tt></td><td headers="h102" align="left" valign="top" scope="row">False</td><td headers="h103" align="left" valign="top" scope="row">True</td>

False
<td headers="h101" align="left" valign="top" scope="row"><tt>setExpandEntityReferences()</tt></td><td headers="h102" align="left" valign="top" scope="row">False</td><td headers="h103" align="left" valign="top" scope="row">True</td>

False
<td headers="h101" align="left" valign="top" scope="row"><tt>setIgnoringComments()</tt></td><td headers="h102" align="left" valign="top" scope="row">False</td><td headers="h103" align="left" valign="top" scope="row">True</td>

False
<td headers="h101" align="left" valign="top" scope="row"><tt>setIgnoringElementContent</tt><tt>Whitespace()</tt></td><td headers="h102" align="left" valign="top" scope="row">False</td><td headers="h103" align="left" valign="top" scope="row">True</td>

False

The implementation of these methods in the main method of the <tt>DomEcho</tt> example is shown below.

```

// ...

dbf.setIgnoringComments(ignoreComments);
dbf.setIgnoringElementContentWhitespace(ignoreWhitespace);
dbf.setCoalescing(putCDATAIntoText);
dbf.setExpandEntityReferences(!createEntityRefs);

// ...

```

The boolean variables <tt>ignoreComments</tt>, <tt>ignoreWhitespace</tt>, <tt>putCDATAIntoText</tt>, and <tt>createEntityRefs</tt> are declared at the beginning of the main method code, and they are set by command line arguments when <tt>DomEcho</tt> is run.

```

public static void main(String[] args) throws Exception {
    // ...

    boolean ignoreWhitespace = false;
    boolean ignoreComments = false;
    boolean putCDATAIntoText = false;
    boolean createEntityRefs = false;

    for (int i = 0; i &lt; args.length; i++) {
        if (...) {  // Validation arguments here
           // ... 
        } 
        else if (args[i].equals("-ws")) {
            ignoreWhitespace = true;
        } 
        else if (args[i].startsWith("-co")) {
            ignoreComments = true;
        }
        else if (args[i].startsWith("-cd")) {
            putCDATAIntoText = true;
        } 
        else if (args[i].startsWith("-e")) {
            createEntityRefs = true;

            // ...
        } 
        else {
            filename = args[i];

            // Must be last arg
            if (i != args.length - 1) {
                usage();
            }
        }
    }

    // ...
}


```

<a name="ggawi" id="ggawi"></a>

## Printing DOM Tree Nodes

The <tt>DomEcho</tt> application allows you to see the structure of a DOM, and demonstrates what nodes make up the DOM and how they are arranged. Generally, the vast majority of nodes in a DOM tree will be <tt>Element</tt> and <tt>Text</tt> nodes.

**Note -** Text nodes exist **under** element nodes in a DOM, and data is always stored in text nodes. Perhaps the most common error in DOM processing is to navigate to an element node and expect it to contain the data that is stored in that element. Not so! Even the simplest element node has a text node under it that contains the data.

The code to print out the DOM tree nodes with the appropriate indentation is shown below.

```

private void echo(Node n) {
    outputIndentation();
    int type = n.getNodeType();

    switch (type) {
        case Node.ATTRIBUTE_NODE:
            out.print("ATTR:");
            printlnCommon(n);
            break;

        case Node.CDATA_SECTION_NODE:
            out.print("CDATA:");
            printlnCommon(n);
            break;

        case Node.COMMENT_NODE:
            out.print("COMM:");
            printlnCommon(n);
            break;

        case Node.DOCUMENT_FRAGMENT_NODE:
            out.print("DOC_FRAG:");
            printlnCommon(n);
            break;

        case Node.DOCUMENT_NODE:
            out.print("DOC:");
            printlnCommon(n);
            break;

        case Node.DOCUMENT_TYPE_NODE:
            out.print("DOC_TYPE:");
            printlnCommon(n);
            NamedNodeMap nodeMap = ((DocumentType)n).getEntities();
            indent += 2;
            for (int i = 0; i &lt; nodeMap.getLength(); i++) {
                Entity entity = (Entity)nodeMap.item(i);
                echo(entity);
            }
            indent -= 2;
            break;

        case Node.ELEMENT_NODE:
            out.print("ELEM:");
            printlnCommon(n);

            NamedNodeMap atts = n.getAttributes();
            indent += 2;
            for (int i = 0; i &lt; atts.getLength(); i++) {
                Node att = atts.item(i);
                echo(att);
            }
            indent -= 2;
            break;

        case Node.ENTITY_NODE:
            out.print("ENT:");
            printlnCommon(n);
            break;

        case Node.ENTITY_REFERENCE_NODE:
            out.print("ENT_REF:");
            printlnCommon(n);
            break;

        case Node.NOTATION_NODE:
            out.print("NOTATION:");
            printlnCommon(n);
            break;

        case Node.PROCESSING_INSTRUCTION_NODE:
            out.print("PROC_INST:");
            printlnCommon(n);
            break;

        case Node.TEXT_NODE:
            out.print("TEXT:");
            printlnCommon(n);
            break;

        default:
            out.print("UNSUPPORTED NODE: " + type);
            printlnCommon(n);
            break;
    }

    indent++;
    for (Node child = n.getFirstChild(); child != null;
         child = child.getNextSibling()) {
        echo(child);
    }
    indent--;
}

```

This code first of all uses switch statements to print out the different node types and any possible child nodes, with the appropriate indentation.

Node attributes are not included as children in the DOM hierarchy. They are instead obtained via the <tt>Node</tt> interface's <tt>getAttributes</tt> method.

The <tt>DocType</tt> interface is an extension of <tt>w3c.org.dom.Node</tt>. It defines the <tt>getEntities</tt> method, which you use to obtain <tt>Entity</tt> nodes - the nodes that define entities. Like <tt>Attribute</tt> nodes, <tt>Entity</tt> nodes do not appear as children of DOM nodes.

<a name="ggdyc" id="ggdyc"></a>

## Node Operations

This section takes a quick look at some of the operations you might want to apply to a DOM.

<li>
Creating nodes
</li>
<li>
Traversing nodes
</li>
<li>
Searching for nodes
</li>
<li>
Obtaining node content
</li>
<li>
Creating attributes
</li>
<li>
Removing and changing nodes
</li>
<li>
Inserting nodes
</li>

<a name="ggdyj" id="ggdyj"></a>

### Creating Nodes

You can create different types nodes using the methods of the <tt>Document</tt> interface. For example, <tt>createElement</tt>, <tt>createComment</tt>, <tt>createCDATAsection</tt>, <tt>createTextNode</tt>, and so on. The full list of methods for creating different nodes is provided in the API documentation for 
[`org.w3c.dom.Document`](https://docs.oracle.com/javase/8/docs/api/org/w3c/dom/Document.html).

<a name="ggdvz" id="ggdvz"></a>

### Traversing Nodes

The <tt>org.w3c.dom.Node</tt> interface defines a number of methods you can use to traverse nodes, including <tt>getFirstChild</tt>, <tt>getLastChild</tt>, <tt>getNextSibling</tt>, <tt>getPreviousSibling</tt>, and <tt>getParentNode</tt>. Those operations are sufficient to get from anywhere in the tree to any other location in the tree.

<a name="ggdwa" id="ggdwa"></a>

### Searching for Nodes

When you are searching for a node with a particular name, there is a bit more to take into account. Although it is tempting to get the first child and inspect it to see whether it is the right one, the search must account for the fact that the first child in the sub-list could be a comment or a processing instruction. If the XML data has not been validated, it could even be a text node containing ignorable whitespace.

In essence, you need to look through the list of child nodes, ignoring the ones that are of no concern and examining the ones you care about. Here is an example of the kind of routine you need to write when searching for nodes in a DOM hierarchy. It is presented here in its entirety (complete with comments) so that you can use it as a template in your applications.

```

/**
 * Find the named subnode in a node's sublist.
 * &lt;ul&gt;
 * &lt;li&gt;Ignores comments and processing instructions.
 * &lt;li&gt;Ignores TEXT nodes (likely to exist and contain
 *         ignorable whitespace, if not validating.
 * &lt;li&gt;Ignores CDATA nodes and EntityRef nodes.
 * &lt;li&gt;Examines element nodes to find one with
 *        the specified name.
 * &lt;/ul&gt;
 * @param name  the tag name for the element to find
 * @param node  the element node to start searching from
 * @return the Node found
 */
public Node findSubNode(String name, Node node) {
    if (node.getNodeType() != Node.ELEMENT_NODE) {
        System.err.println("Error: Search node not of element type");
        System.exit(22);
    }

    if (! node.hasChildNodes()) return null;

    NodeList list = node.getChildNodes();
    for (int i=0; i &lt; list.getLength(); i++) {
        Node subnode = list.item(i);
        if (subnode.getNodeType() == Node.ELEMENT_NODE) {
           if (subnode.getNodeName().equals(name)) 
               return subnode;
        }
    }
    return null;
}

```

For a deeper explanation of this code, see 
[Increasing the Complexity](when.html#gchls) in 
[When to Use DOM](when.html). Note, too, that you can use APIs described in 
[Lexical Controls](readingXML.html#ggdwv) to modify the kind of DOM the parser constructs. The nice thing about this code, though, is that it will work for almost any DOM.

<a name="ggdxv" id="ggdxv"></a>

### Obtaining Node Content

When you want to get the text that a node contains, you again need to look through the list of child nodes, ignoring entries that are of no concern and accumulating the text you find in <tt>TEXT</tt> nodes, <tt>CDATA</tt> nodes, and <tt>EntityRef</tt> nodes. Here is an example of the kind of routine you can use for that process.

```

/**
  * Return the text that a node contains. This routine:
  * &lt;ul&gt;
  * &lt;li&gt;Ignores comments and processing instructions.
  * &lt;li&gt;Concatenates TEXT nodes, CDATA nodes, and the results of
  *     recursively processing EntityRef nodes.
  * &lt;li&gt;Ignores any element nodes in the sublist.
  *     (Other possible options are to recurse into element 
  *      sublists or throw an exception.)
  * &lt;/ul&gt;
  * @param    node  a  DOM node
  * @return   a String representing its contents
  */
public String getText(Node node) {
    StringBuffer result = new StringBuffer();
    if (! node.hasChildNodes()) return "";

    NodeList list = node.getChildNodes();
    for (int i=0; i &lt; list.getLength(); i++) {
        Node subnode = list.item(i);
        if (subnode.getNodeType() == Node.TEXT_NODE) {
            result.append(subnode.getNodeValue());
        }
        else if (subnode.getNodeType() == Node.CDATA_SECTION_NODE) {
            result.append(subnode.getNodeValue());
        }
        else if (subnode.getNodeType() == Node.ENTITY_REFERENCE_NODE) {
            // Recurse into the subtree for text
            // (and ignore comments)
            result.append(getText(subnode));
        }
    }

    return result.toString();
}

```

For a deeper explanation of this code, see 
[Increasing the Complexity](when.html#gchls) in 
[When to Use DOM](when.html). Again, you can simplify this code by using the APIs described in 
[Lexical Controls](readingXML.html#ggdwv) to modify the kind of DOM the parser constructs. But the nice thing about this code is that it will work for almost any DOM.

<a name="ggdxn" id="ggdxn"></a>

### Creating Attributes

The <tt>org.w3c.dom.Element</tt> interface, which extends Node, defines a <tt>setAttribute</tt> operation, which adds an attribute to that node. (A better name from the Java platform standpoint would have been <tt>addAttribute</tt>. The attribute is not a property of the class, and a new object is created.) You can also use the <tt>Document</tt>'s <tt>createAttribute</tt> operation to create an instance of <tt>Attribute</tt> and then use the <tt>setAttributeNode</tt> method to add it.

<a name="ggdyw" id="ggdyw"></a>

### Removing and Changing Nodes

To remove a node, you use its parent Node's <tt>removeChild</tt> method. To change it, you can use either the parent node's <tt>replaceChild</tt> operation or the node's <tt>setNodeValue</tt> operation.

<a name="ggdxt" id="ggdxt"></a>

### Inserting Nodes

The important thing to remember when creating new nodes is that when you create an element node, the only data you specify is a name. In effect, that node gives you a hook to hang things on. You hang an item on the hook by adding to its list of child nodes. For example, you might add a text node, a <tt>CDATA</tt> node, or an attribute node. As you build, keep in mind the structure you have seen in this tutorial. Remember: Each node in the hierarchy is extremely simple, containing only one data element.

<a name="ggegf" id="ggegf"></a>

## Running the <tt>DOMEcho</tt> Sample

To run the <tt>DOMEcho</tt> sample, follow the steps below.

1. **Navigate to the <tt>samples</tt> directory.**`% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>.`
1. **Compile the example class.**`% javac dom/*`
<li>**Run the <tt>DOMEcho</tt> program on an XML file.**
Choose one of the XML files in the <tt>data</tt> directory and run the <tt>DOMEcho</tt> program on it. Here, we have chosen to run the program on the file <tt>personal-schema.xml</tt>.
`% java dom/DOMEcho data/personal-schema.xml`
The XML file <tt>personal-schema.xml</tt> contains the personnel files for a small company. When you run the <tt>DOMEcho</tt> program on it, you should see the following output.
<pre><code>
DOC: nodeName="#document"
 ELEM: nodeName="personnel" 
       local="personnel"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="person" 
       local="person"
 ATTR: nodeName="id" 
       local="id" 
       nodeValue="Big.Boss"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="name" 
       local="name"
 ELEM: nodeName="family" 
       local="family"
 TEXT: nodeName="#text" 
       nodeValue="Boss"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="given" 
       local="given"
 TEXT: nodeName="#text" 
       nodeValue="Big"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="email" 
       local="email"
 TEXT: nodeName="#text" 
       nodeValue="chief@foo.example.com"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="link" 
       local="link"
 ATTR: nodeName="subordinates" 
       local="subordinates" 
       nodeValue="one.worker two.worker 
                  three.worker four.worker
                  five.worker"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="person" 
       local="person"
 ATTR: nodeName="id" 
       local="id" 
       nodeValue="one.worker"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="name" 
       local="name"
 ELEM: nodeName="family" 
       local="family"
 TEXT: nodeName="#text" 
       nodeValue="Worker"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="given" 
       local="given"
 TEXT: nodeName="#text" 
       nodeValue="One"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="email" 
       local="email"
 TEXT: nodeName="#text" 
       nodeValue="one@foo.example.com"
 TEXT: nodeName="#text" 
       nodeValue=[WS]
 ELEM: nodeName="link" 
       local="link"
 ATTR: nodeName="manager" 
       local="manager" 
       nodeValue="Big.Boss"
 TEXT: nodeName="#text"
       nodeValue=[WS]

[...]

</code></pre>
As you can see, <tt>DOMEcho</tt> prints out all the nodes for the different elements in the document, with the correct indentation to show the node hierarchy.
</li>
