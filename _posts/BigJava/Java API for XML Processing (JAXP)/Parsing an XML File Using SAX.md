
# Parsing an XML File Using SAX

In real-life applications, you will want to use the SAX parser to process XML data and do something useful with it. This section examines an example JAXP program, <tt>SAXLocalNameCount</tt>, that counts the number of elements using only the <tt>localName</tt> component of the element, in an XML document. Namespace names are ignored for simplicity. This example also shows how to use a SAX <tt>ErrorHandler</tt>.

**Note -** After you have downloaded and installed the sources of the JAXP API from the 
[JAXP download area](http://jaxp.java.net/downloads.html), the sample program for this example is found in the directory *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples/sax</tt>. The XML files it interacts with are found in *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples/data</tt>.

<a name="gcljn" id="gcljn"></a>

## Creating the Skeleton

The <tt>SAXLocalNameCount</tt> program is created in a file named <tt>SAXLocalNameCount.java</tt>.

```

public class SAXLocalNameCount {
    static public void main(String[] args) {
        // ...
    }
}

```

Because you will run it standalone, you need a <tt>main()</tt> method. And you need command-line arguments so that you can tell the application which file to process.

<a name="gcljh" id="gcljh"></a>

## Importing Classes

The import statements for the classes the application will use are the following.

```

package sax;
import javax.xml.parsers.*;
import org.xml.sax.*;
import org.xml.sax.helpers.*;

import java.util.*;
import java.io.*;

public class SAXLocalNameCount {
    // ...
}

```

The <tt>javax.xml.parsers</tt> package contains the <tt>SAXParserFactory</tt> class that creates the parser instance used. It throws a <tt>ParserConfigurationException</tt> if it cannot produce a parser that matches the specified configuration of options. (Later, you will see more about the configuration options). The <tt>javax.xml.parsers</tt> package also contains the <tt>SAXParser</tt> class, which is what the factory returns for parsing. The <tt>org.xml.sax</tt> package defines all the interfaces used for the SAX parser. The <tt>org.xml.sax.helpers</tt> package contains <tt>DefaultHandler</tt>, which defines the class that will handle the SAX events that the parser generates. The classes in <tt>java.util</tt> and <tt>java.io</tt>, are needed to provide hash tables and output.

<a name="gcnsk" id="gcnsk"></a>

## Setting Up I/O

The first order of business is to process the command-line arguments, which at this stage only serve to get the name of the file to process. The following code in the <tt>main</tt> method tells the application what file you want <tt>SAXLocalNameCountMethod</tt> to process.

```

static public void main(String[] args) throws Exception {
    String filename = null;

    for (int i = 0; i &lt; args.length; i++) {
        filename = args[i];
        if (i != args.length - 1) {
            usage();
        }
    }

    if (filename == null) {
        usage();
    } 
}

```

This code sets the main method to throw an <tt>Exception</tt> when it encounters problems, and defines the command-line options which are required to tell the application the name of the XML file to be processed. Other command line arguments in this part of the code will be examined later in this lesson, when we start looking at validation.

The <tt>filename</tt> String that you give when you run the application will be converted to a <tt>java.io.File</tt> URL by an internal method, <tt>convertToFileURL()</tt>. This is done by the following code in <tt>SAXLocalNameCountMethod</tt>.

```

public class SAXLocalNameCount {
    private static String convertToFileURL(String filename) {
        String path = new File(filename).getAbsolutePath();
        if (File.separatorChar != '/') {
            path = path.replace(File.separatorChar, '/');
        }

        if (!path.startsWith("/")) {
            path = "/" + path;
        }
        return "file:" + path;
    }

    // ...
}

```

If the incorrect command-line arguments are specified when the program is run, then the <tt>SAXLocalNameCount</tt> application's <tt>usage()</tt> method is invoked, to print out the correct options onscreen.

```

private static void usage() {
    System.err.println("Usage: SAXLocalNameCount &lt;file.xml&gt;");
    System.err.println("       -usage or -help = this message");
    System.exit(1);
}

```

Further <tt>usage()</tt> options will be examined later in this lesson, when validation is addressed.

<a name="gclmw" id="gclmw"></a>

## Implementing the <tt>ContentHandler</tt> Interface

The most important interface in <tt>SAXLocalNameCount</tt> is <tt>ContentHandler</tt>. This interface requires a number of methods that the SAX parser invokes in response to various parsing events. The major event-handling methods are: <tt>startDocument</tt>, <tt>endDocument</tt>, <tt>startElement</tt>, and <tt>endElement</tt>.

The easiest way to implement this interface is to extend the <tt>DefaultHandler</tt> class, defined in the <tt>org.xml.sax.helpers</tt> package. That class provides do-nothing methods for all the <tt>ContentHandler</tt> events. The example program extends that class.

```

public class SAXLocalNameCount **extends DefaultHandler** {
    // ...
} 

```

**Note -** <tt>DefaultHandler</tt> also defines do-nothing methods for the other major events, defined in the <tt>DTDHandler</tt>, <tt>EntityResolver</tt>, and <tt>ErrorHandler</tt> interfaces. You will learn more about those methods later in this lesson.

Each of these methods is required by the interface to throw a <tt>SAXException</tt>. An exception thrown here is sent back to the parser, which sends it on to the code that invoked the parser.

<a name="gclnc" id="gclnc"></a>

## Handling Content Events

This section shows the code that processes the <tt>ContentHandler</tt> events.

When a start tag or end tag is encountered, the name of the tag is passed as a String to the <tt>startElement</tt> or the <tt>endElement</tt> method, as appropriate. When a start tag is encountered, any attributes it defines are also passed in an <tt>Attributes</tt> list. Characters found within the element are passed as an array of characters, along with the number of characters (length) and an offset into the array that points to the first character.

<a name="gclmb" id="gclmb"></a>

### Document Events

The following code handles the start-document and end-document events:

```

public class SAXLocalNameCount extends DefaultHandler {
    
    private Hashtable tags;

    public void startDocument() throws SAXException {
        tags = new Hashtable();
    }

    public void endDocument() throws SAXException {
        Enumeration e = tags.keys();
        while (e.hasMoreElements()) {
            String tag = (String)e.nextElement();
            int count = ((Integer)tags.get(tag)).intValue();
            System.out.println("Local Name \"" + tag + "\" occurs " 
                               + count + " times");
        }    
    }
 
    private static String convertToFileURL(String filename) {
        // ...
    }

    // ...
}

```

This code defines what the application does when the parser encounters the start and end points of the document being parsed. The <tt>ContentHandler</tt> interface's <tt>startDocument()</tt> method creates a <tt>java.util.Hashtable</tt> instance, which in [Element Events](#gclmm) will be populated with the XML elements the parser finds in the document. When the parser reaches the end of the document, the <tt>endDocument()</tt> method is invoked, to get the names and counts of the elements contained in the hash table, and print out a message onscreen to tell the user how many incidences of each element were found.

Both of these <tt>ContentHandler</tt> methods throw <tt>SAXException</tt>s. You will learn more about SAX exceptions in [Setting up Error Handling](#gcnsr).

<a name="gclmm" id="gclmm"></a>

### Element Events

As mentioned in [Document Events](#gclmb), the hash table created by the <tt>startDocument</tt> method needs to be populated with the various elements that the parser finds in the document. The following code processes the start-element and end-element events:

```

public void startDocument() throws SAXException {
    tags = new Hashtable();
}

public void startElement(String namespaceURI,
                         String localName,
                         String qName, 
                         Attributes atts)
    throws SAXException {

    String key = localName;
    Object value = tags.get(key);

    if (value == null) {
        tags.put(key, new Integer(1));
    } 
    else {
        int count = ((Integer)value).intValue();
        count++;
        tags.put(key, new Integer(count));
    }
}
 
public void endDocument() throws SAXException {
    // ...
}

```

This code processes the element tags, including any attributes defined in the start tag, to obtain the namespace universal resource identifier (URI), the local name and the qualified name of that element. The <tt>startElement()</tt> method then populates the hash map created by <tt>startDocument()</tt> with the local names and the counts thereof, for each type of element. Note that when the <tt>startElement()</tt> method is invoked, if namespace processing is not enabled, then the local name for elements and attributes could turn out to be an empty string. The code handles that case by using the qualified name whenever the simple name is an empty string.

<a name="gclmx" id="gclmx"></a>

### Character Events

The JAXP SAX API also allows you to handle the characters that the parser delivers to your application, using the <tt>ContentHandler.characters()</tt> method.

**Note -** Character events are not demonstrated in the <tt>SAXLocalNameCount</tt> example, but a brief description is included in this section, for completeness.

Parsers are not required to return any particular number of characters at one time. A parser can return anything from a single character at a time up to several thousand and still be a standard-conforming implementation. So if your application needs to process the characters it sees, it is wise to have the <tt>characters()</tt> method accumulate the characters in a <tt>java.lang.StringBuffer</tt> and operate on them only when you are sure that all of them have been found.

You finish parsing text when an element ends, so you normally perform your character processing at that point. But you might also want to process text when an element starts. This is necessary for document-style data, which can contain XML elements that are intermixed with text. For example, consider this document fragment:

`**&lt;para&gt;**This paragraph contains **&lt;bold&gt;**important**&lt;/bold&gt;** ideas.**&lt;/para&gt;**`

The initial text, <tt>This paragraph contains</tt>, is terminated by the start of the <tt>&lt;bold&gt;</tt> element. The text <tt>important</tt> is terminated by the end tag, <tt>&lt;/bold&gt;</tt>, and the final text, <tt>ideas.</tt>, is terminated by the end tag, <tt>&lt;/para&gt;</tt>.

To be strictly accurate, the character handler should scan for ampersand characters (&amp;) and left-angle bracket characters (&lt;) and replace them with the strings <tt>&amp;amp;</tt> or <tt>&amp;lt;</tt>, as appropriate. This is explained in the next section.

<a name="gcwqt" id="gcwqt"></a>

### Handling Special Characters

In XML, an entity is an XML structure (or plain text) that has a name. Referencing the entity by name causes it to be inserted into the document in place of the entity reference. To create an entity reference, you surround the entity name with an ampersand and a semicolon:

`&amp;entityName;`

When you are handling large blocks of XML or HTML that include many special characters, you can use a CDATA section. A CDATA section works like <tt>&lt;code&gt;...&lt;/code&gt;</tt> in HTML, only more so: all white space in a CDATA section is significant, and characters in it are not interpreted as XML. A CDATA section starts with <tt>&lt;![[CDATA[</tt> and ends with <tt>]]&gt;</tt>.

An example of a CDATA section, taken from the sample XML file *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples/data/REC-xml-19980210.xml</tt>, is shown below.

`&lt;p&gt;&lt;termdef id="dt-cdsection" term="CDATA Section"&lt;&lt;term&gt;CDATA sections&lt;/term&gt; may occur anywhere character data may occur; they are used to escape blocks of text containing characters which would otherwise be recognized as markup. CDATA sections begin with the string "&lt;code&gt;&amp;lt;![CDATA[&lt;/code&gt;" and end with the string "&lt;code&gt;]]&amp;gt;&lt;/code&gt;"`

Once parsed, this text would be displayed as follows:

CDATA sections may occur anywhere character data may occur; they are used to escape blocks of text containing characters which would otherwise be recognized as markup. CDATA sections begin with the string "<tt>&lt;![CDATA[</tt>" and end with the string "<tt>]]&gt;</tt>".

The existence of CDATA makes the proper echoing of XML a bit tricky. If the text to be output is not in a CDATA section, then any angle brackets, ampersands, and other special characters in the text should be replaced with the appropriate entity reference. (Replacing left angle brackets and ampersands is most important, other characters will be interpreted properly without misleading the parser.) But if the output text is in a CDATA section, then the substitutions should not occur, resulting in text like that in the earlier example. In a simple program such as our <tt>SAXLocalNameCount</tt> application, this is not particularly serious. But many XML-filtering applications will want to keep track of whether the text appears in a CDATA section, so that they can treat special characters properly.

<a name="gclmt" id="gclmt"></a>

## Setting up the Parser

The following code sets up the parser and gets it started:

```

static public void main(String[] args) throws Exception {

    // Code to parse command-line arguments 
    //(shown above)
    // ...

    SAXParserFactory spf = SAXParserFactory.newInstance();
    spf.setNamespaceAware(true);
    SAXParser saxParser = spf.newSAXParser();
}

```

These lines of code create a <tt>SAXParserFactory</tt> instance, as determined by the setting of the <tt>javax.xml.parsers.SAXParserFactory</tt> system property. The factory to be created is set up to support XML namespaces by setting <tt>setNamespaceAware</tt> to true, and then a <tt>SAXParser</tt> instance is obtained from the factory by invoking its <tt>newSAXParser()</tt> method.

**Note -** The <tt>javax.xml.parsers.SAXParser</tt> class is a wrapper that defines a number of convenience methods. It wraps the (somewhat less friendly) <tt>org.xml.sax.Parser</tt> object. If needed, you can obtain that parser using the <tt>getParser()</tt> method of the <tt>SAXParser</tt> class.

You now need to implement the <tt>XMLReader</tt> that all parsers must implement. The <tt>XMLReader</tt> is used by the application to tell the SAX parser what processing it is to perform on the document in question. The <tt>XMLReader</tt> is implemented by the following code in the <tt>main</tt> method.

```

// ...
SAXParser saxParser = spf.newSAXParser();
XMLReader xmlReader = saxParser.getXMLReader();
xmlReader.setContentHandler(new SAXLocalNameCount());
xmlReader.parse(convertToFileURL(filename));

```

Here, you obtain an <tt>XMLReader</tt> instance for your parser by invoking your <tt>SAXParser</tt> instance's <tt>getXMLReader()</tt> method. The <tt>XMLReader</tt> then registers the <tt>SAXLocalNameCount</tt> class as its content handler, so that the actions performed by the parser will be those of the <tt>startDocument()</tt>, <tt>startElement()</tt>, and <tt>endDocument()</tt> methods shown in [Handling Content Events](#gclnc). Finally, the <tt>XMLReader</tt> tells the parser which document to parse by passing it the location of the XML file in question, in the form of the <tt>File</tt> URL generated by the <tt>convertToFileURL()</tt> method defined in [Setting Up I/O](#gcnsk).

<a name="gcnsr" id="gcnsr"></a>

## Setting up Error Handling

You could start using your parser now, but it is safer to implement some error handling. The parser can generate three kinds of errors: a fatal error, an error, and a warning. When a fatal error occurs, the parser cannot continue. So if the application does not generate an exception, then the default error-event handler generates one. But for nonfatal errors and warnings, exceptions are never generated by the default error handler, and no messages are displayed.

As shown in [Document Events](#gclmb), the application's event handling methods throw <tt>SAXException</tt>. For example, the signature of the <tt>startDocument()</tt> method in the <tt>ContentHandler</tt> interface is defined as returning a <tt>SAXException</tt>.

`public void startDocument() throws SAXException { /* ... */ }`

A <tt>SAXException</tt> can be constructed using a message, another exception, or both.

Because the default parser only generates exceptions for fatal errors, and because the information about the errors provided by the default parser is somewhat limited, the <tt>SAXLocalNameCount</tt> program defines its own error handling, through the <tt>MyErrorHandler</tt> class.

```

xmlReader.setErrorHandler(new MyErrorHandler(System.err));

// ...

private static class MyErrorHandler implements ErrorHandler {
    private PrintStream out;

    MyErrorHandler(PrintStream out) {
        this.out = out;
    }

    private String getParseExceptionInfo(SAXParseException spe) {
        String systemId = spe.getSystemId();

        if (systemId == null) {
            systemId = "null";
        }

        String info = "URI=" + systemId + " Line=" 
            + spe.getLineNumber() + ": " + spe.getMessage();

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

In the same way as in [Setting up the Parser](#gclmt), which showed the <tt>XMLReader</tt> being pointed to the correct content handler, here the <tt>XMLReader</tt> is pointed to the new error handler by calling its <tt>setErrorHandler()</tt> method.

The <tt>MyErrorHandler</tt> class implements the standard <tt>org.xml.sax.ErrorHandler</tt> interface, and defines a method to obtain the exception information that is provided by any <tt>SAXParseException</tt> instances generated by the parser. This method, <tt>getParseExceptionInfo()</tt>, simply obtains the line number at which the error occurs in the XML document and the identifier of the system on which it is running by calling the standard <tt>SAXParseException</tt> methods <tt>getLineNumber()</tt> and <tt>getSystemId()</tt>. This exception information is then fed into implementations of the basic SAX error handling methods <tt>error()</tt>, <tt>warning()</tt>, and <tt>fatalError()</tt>, which are updated to send the appropriate messages about the nature and location of the errors in the document.

<a name="gcvww" id="gcvww"></a>

### Handling NonFatal Errors

A nonfatal error occurs when an XML document fails a validity constraint. If the parser finds that the document is not valid, then an error event is generated. Such errors are generated by a validating parser, given a document type definition (DTD) or schema, when a document has an invalid tag, when a tag is found where it is not allowed, or (in the case of a schema) when the element contains invalid data.

The most important principle to understand about nonfatal errors is that they are ignored by default. But if a validation error occurs in a document, you probably do not want to continue processing it. You probably want to treat such errors as fatal.

To take over error handling, you override the <tt>DefaultHandler</tt> methods that handle fatal errors, nonfatal errors, and warnings as part of the <tt>ErrorHandler</tt> interface. As shown in the code extract in the previous section, the SAX parser delivers a <tt>SAXParseException</tt> to each of these methods, so generating an exception when an error occurs is as simple as throwing it back.

**Note -** It can be instructive to examine the error-handling methods defined in <tt>org.xml.sax.helpers.DefaultHandler</tt>. You will see that the <tt>error()</tt> and <tt>warning()</tt> methods do nothing, whereas <tt>fatalError()</tt> throws an exception. Of course, you could always override the <tt>fatalError()</tt> method to throw a different exception. But if your code does not throw an exception when a fatal error occurs, then the SAX parser will. The XML specification requires it.

<a name="gcvwp" id="gcvwp"></a>

### Handling Warnings

Warnings, too, are ignored by default. Warnings are informative and can only be generated in the presence of a DTD or schema. For example, if an element is defined twice in a DTD, a warning is generated. It is not illegal, and it does not cause problems, but it is something you might like to know about because it might not have been intentional. Validating an XML document against a DTD will be shown in the section 
.

<a name="gcnrx" id="gcnrx"></a>

## Running the SAX Parser Example without Validation

As stated at the beginning of this lesson, after you have downloaded and installed the sources of the JAXP API from the [JAXP sources download area](http://jaxp.java.net/downloads.html), the sample program and the associated files needed to run it are found in the following locations.

<li>
The different Java archive (JAR) files for the example are located in the directory *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/lib</tt>.
</li>
<li>
The <tt>SAXLocalNameCount.java</tt> file is found in *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples/sax</tt>.
</li>
<li>
The XML files that <tt>SAXLocalNameCount</tt> interacts with are found in *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples/data</tt>.
</li>

The following steps explain how to run the SAX parser example without validation.

<a name="gcvwx" id="gcvwx"></a>

### To Run the <tt>SAXLocalNameCount</tt> Example without Validation

1. **Navigate to the <tt>samples</tt> directory.**`% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>.`
1. **Compile the example class.**`% javac sax/*`
<li>**Run the <tt>SAXLocalNameCount</tt> program on an XML file.**
Choose one of the XML files in the <tt>data</tt> directory and run the <tt>SAXLocalNameCount</tt> program on it. Here, we have chosen to run the program on the file <tt>rich_iii.xml</tt>.
`% java sax/SAXLocalNameCount data/rich_iii.xml`
The XML file <tt>rich_iii.xml</tt> contains an XML version of William Shakespeare's play *Richard III*. When you run the <tt>SAXLocalNameCount</tt> on it, you should see the following output.
<pre><code>
Local Name "STAGEDIR" occurs 230 times
Local Name "PERSONA" occurs 39 times
Local Name "SPEECH" occurs 1089 times
Local Name "SCENE" occurs 25 times
Local Name "ACT" occurs 5 times
Local Name "PGROUP" occurs 4 times
Local Name "PLAY" occurs 1 times
Local Name "PLAYSUBT" occurs 1 times
Local Name "FM" occurs 1 times
Local Name "SPEAKER" occurs 1091 times
Local Name "TITLE" occurs 32 times
Local Name "GRPDESCR" occurs 4 times
Local Name "P" occurs 4 times
Local Name "SCNDESCR" occurs 1 times
Local Name "PERSONAE" occurs 1 times
Local Name "LINE" occurs 3696 times
</code></pre>
The <tt>SAXLocalNameCount</tt> program parses the XML file, and provides a count of the number of instances of each type of XML tag that it contains.
</li>
<li>**Open the file <tt>data/rich_iii.xml</tt> in a text editor.**
To check that the error handling is working, delete the closing tag from an entry in the XML file, for example the closing tag <tt>&lt;/PERSONA&gt;</tt>, from line 30, shown below.
`30 &lt;PERSONA&gt;EDWARD, Prince of Wales, afterwards King Edward V.&lt;/PERSONA&gt;`</li>
<li>**Run <tt>SAXLocalNameCount</tt> again.**
This time, you should see the following fatal error message.
`% java sax/SAXLocalNameCount data/rich_iii.xml Exception in thread "main" org.xml.sax.SAXException: Fatal Error: URI=file:*install-dir* /JAXP_sources/jaxp-1_4_2-*release-date*/samples/data/rich_iii.xml Line=30: The element type "PERSONA" must be terminated by the matching end-tag "&lt;/PERSONA&gt;".`
As you can see, when the error was encountered, the parser generated a <tt>SAXParseException</tt>, a subclass of <tt>SAXException</tt> that identifies the file and the location where the error occurred.
</li>
