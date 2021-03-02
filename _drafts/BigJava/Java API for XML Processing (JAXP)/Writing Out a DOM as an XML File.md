
# Generating XML from an Arbitrary Data Structure

This section uses XSLT to convert an arbitrary data structure to XML.

Here is an outline of the process:

<li>
Modify an existing program that reads the data, to make it generate SAX events. (Whether that program is a real parser or simply a data filter of some kind is irrelevant for the moment).
</li>
<li>
Use the SAX "parser" to construct a <tt>SAXSource</tt> for the transformation.
</li>
<li>
Use the same <tt>StreamResult</tt> object as created in the last exercise to display the results. (But note that you could just as easily create a <tt>DOMResult</tt> object to create a DOM in memory).
</li>
<li>
Wire the source to the result using the transformer object to make the conversion.
</li>

For starters, you need a data set you want to convert and a program capable of reading the data. The next two sections create a simple data file and a program that reads it.

<a name="gghhh" id="gghhh"></a>

## Creating a Simple File

This example uses data set for an address book, <tt>PersonalAddressBook.ldif</tt>. If you have not done so already, [download the XSLT examples](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. The file shown here was produced by creating a new address book in Netscape Messenger, giving it some dummy data (one address card), and then exporting it in LDAP Data Interchange Format (LDIF) format. It contained in the directory <tt>xslt/data</tt> after you unzip the XSLT examples.

This example uses data set for an address book, <tt>PersonalAddressBook.ldif</tt>. If you have not done so already,
[`download the XSLT examples`](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. The file shown here was produced by creating a new address book in Netscape Messenger, giving it some dummy data (one address card), and then exporting it in LDAP Data Interchange Format (LDIF) format. It contained in the directory <tt>xslt/data</tt> after you unzip the XSLT examples.

The following [Figure&#160;](#gghhj) shows the address book entry that was created.

<a name="gghhj" id="gghhj"></a>

Figure&#160; Address Book Entry

<img src="../../figures/jaxp/intro/addressBook.gif" alt="Snapshot of a Mozilla Thunderbird contact details card." width="302" height="344" />

Exporting the address book produces a file like the one shown next. The parts of the file that we care about are shown in bold.

```

dn: cn=Fred Flintstone,mail=fred@barneys.house
modifytimestamp: 20010409210816Z
cn: Fred Flintstone
**xmozillanickname: Fred**
**mail: Fred@barneys.house**
**xmozillausehtmlmail: TRUE**
**givenname: Fred**
**sn: Flintstone**
**telephonenumber: 999-Quarry**
**homephone: 999-BedrockLane**
**facsimiletelephonenumber: 888-Squawk**
**pagerphone: 777-pager**
**cellphone: 555-cell**
**xmozillaanyphone: 999-Quarry**
**objectclass: top**
**objectclass: person**

```

Note that each line of the file contains a variable name, a colon, and a space followed by a value for the variable. The <tt>sn</tt> variable contains the person's surname (last name) and the variable <tt>cn</tt> contains the <tt>DisplayName</tt> field from the address book entry.

<a name="gghhy" id="gghhy"></a>

## Creating a Simple Parser

The next step is to create a program that parses the data.

**Note -** The code discussed in this section is in <tt>AddressBookReader01.java</tt>, which is found in the <tt>xslt</tt> directory after you unzip [XSLT examples](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

**Note -** The code discussed in this section is in <tt>AddressBookReader01.java</tt>, which is found in the <tt>xslt</tt> directory after you unzip
[`XSLT examples`](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

The text for the program is shown next. It is an extremely simple program that does not even loop for multiple entries because, after all, it is only a demo.

```

import java.io.*; 

public class AddressBookReader01 { 

    public static void main(String argv[]) {
        // Check the arguments
        if (argv.length != 1) {
            System.err.println("Usage: java AddressBookReader01 filename");
            System.exit (1);
        }

        String filename = argv[0];
        File f = new File(filename);
        AddressBookReader01 reader = new AddressBookReader01();
        reader.parse(f);
    }

    // Parse the input file
    public void parse(File f) {
        try {
            // Get an efficient reader for the file
            FileReader r = new FileReader(f);
            BufferedReader br = new BufferedReader(r);

            // Read the file and display its contents.
            String line = br.readLine();
            while (null != (line = br.readLine())) {
                if (line.startsWith("xmozillanickname: "))
                    break;
            }

            output("nickname", "xmozillanickname", line);
            line = br.readLine();
            output("email",  "mail", line);

            line = br.readLine();
            output("html", "xmozillausehtmlmail", line);

            line = br.readLine();
            output("firstname","givenname", line);

            line = br.readLine();
            output("lastname", "sn", line);

            line = br.readLine();
            output("work", "telephonenumber", line);

            line = br.readLine();
            output("home", "homephone", line);

            line = br.readLine();
            output("fax", "facsimiletelephonenumber", line);

            line = br.readLine();
            output("pager", "pagerphone", line);

            line = br.readLine();
            output("cell", "cellphone", line);
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

This program contains three methods:

The <tt>main</tt> method gets the name of the file from the command line, creates an instance of the parser, and sets it to work parsing the file. This method will disappear when we convert the program into a SAX parser. (That is one reason for putting the parsing code into a separate method).

This method operates on the <tt>File</tt> object sent to it by the main routine. As you can see, it is very straightforward. The only concession to efficiency is the use of a <tt>BufferedReader</tt>, which can become important when you start operating on large files.

The output method contains the logic for the structure of a line. It takes three arguments. The first argument gives the method a name to display, so it can output <tt>html</tt> as a variable name, instead of <tt>xmozillausehtmlmail</tt>. The second argument gives the variable name stored in the file (<tt>xmozillausehtmlmail</tt>). The third argument gives the line containing the data. The routine then strips off the variable name from the start of the line and outputs the desired name, plus the data.

<a name="gghni" id="gghni"></a>

### Running the <tt>AddressBookReader01</tt> Sample

<li>**Navigate to the <tt>samples</tt> directory.**
<pre><code>
% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>.
</code></pre>
</li>
<!--
1. **[Download the XSLT examples by clicking this link](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.**
-->
<li><b>
[`Download the XSLT examples by clicking this link`](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.</b></li>
<li>**Navigate to the <tt>xslt</tt> directory.**
<pre><code>
cd xslt
</code></pre>
</li>
<li>**Compile the <tt>AddressBookReader01</tt> sample.**
Type the following command:
<pre><code>
% javac AddressBookReader01.java
</code></pre>
</li>
<li>**Run the <tt>AddressBookReader01</tt> sample on a data file.**
In the case below, <tt>AddressBookReader01</tt> is run on the file <tt>PersonalAddressBook.ldif</tt> shown above, found in the <tt>xslt/data</tt> directory after you have unzipped the samples bundle.
<pre><code>
% java AddressBookReader01 data/PersonalAddressBook.ldif
</code></pre>
You will see the following output:
<pre><code>
nickname: Fred
email: Fred@barneys.house
html: TRUE
firstname: Fred
lastname: Flintstone
work: 999-Quarry
home: 999-BedrockLane
fax: 888-Squawk
pager: 777-pager
cell: 555-cell
</code></pre>
This is a bit more readable than the file shown in [Creating a Simple File](#gghhh).
</li>

<a name="gghhe" id="gghhe"></a>

## Creating a Parser that Generates SAX Events

This section shows how to make the parser generate SAX events, so that you can use it as the basis for a <tt>SAXSource</tt> object in an XSLT transform.

**Note -** The code discussed in this section is in <tt>AddressBookReader02.java</tt>, which is found in the <tt>xslt</tt> directory after you unzip [XSLT examples](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. <tt>AddressBookReader02.java</tt> is adapted from <tt>AddressBookReader01.java</tt>, so only the differences in code between the two examples will be discussed here.

**Note -** The code discussed in this section is in <tt>AddressBookReader02.java</tt>, which is found in the <tt>xslt</tt> directory after you unzip
[`XSLT examples`](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. <tt>AddressBookReader02.java</tt> is adapted from <tt>AddressBookReader01.java</tt>, so only the differences in code between the two examples will be discussed here.

<tt>AddressBookReader02</tt> requires the following highlighted classes that were not used in <tt>AddressBookReader01</tt>.

```

import java.io.*; 

**import org.xml.sax.*;**
**import org.xml.sax.helpers.AttributesImpl;**

```

The application also extends <tt>XmlReader</tt>. This change converts the application into a parser that generates the appropriate SAX events.

```

public class AddressBookReader02 **implements XMLReader** { /* ... */ }

```

Unlike the <tt>AddressBookReader01</tt> sample, this application does not have a <tt>main</tt> method.

The following global variables will be used later in this section:

```

public class AddressBookReader02 implements XMLReader {
    **ContentHandler handler;**

    **String nsu = "";**  
    **Attributes atts = new AttributesImpl();**
    **String rootElement = "addressbook";**

    **String indent = "\n ";**

    // ...
}

```

The SAX <tt>ContentHandler</tt> is the object that will get the SAX events generated by the parser. To make the application into an <tt>XmlReader</tt>, the application defines a <tt>setContentHandler</tt> method. The handler variable will hold a reference to the object that is sent when <tt>setContentHandler</tt> is invoked.

When the parser generates SAX element events, it will need to supply namespace and attribute information. Because this is a simple application, it defines null values for both of those.

The application also defines a root element for the data structure (<tt>addressbook</tt>) and sets up an indent string to improve the readability of the output.

Furthermore, the parse method is modified so that it takes an <tt>InputSource</tt> (rather than a <tt>File</tt>) as an argument and accounts for the exceptions it can generate:

```

public void parse**(InputSource input)** **throws IOException, SAXException**

```

Now, rather than creating a new <tt>FileReader</tt> instance as was done in <tt>AddressBookReader01</tt>, the reader is encapsulated by the <tt>InputSource</tt> object:

```

try {
    **java.io.Reader r = input.getCharacterStream();**
    BufferedReader Br = new BufferedReader(r);
    // ...
}

```

**Note -** The next section shows how to create the input source object and what is put in it will, in fact, be a buffered reader. But the <tt>AddressBookReader</tt> could be used by someone else, somewhere down the line. This step makes sure that the processing will be efficient, regardless of the reader you are given.

The next step is to modify the parse method to generate SAX events for the start of the document and the root element. The following highlighted code does that:

```

public void parse(InputSource input) {
    try {
        // ...
        String line = br.readLine();
        while (null != (line = br.readLine())) {
            if (line.startsWith("xmozillanickname: ")) 
                break;
        }

        **if (handler == null) {**
            **throw new SAXException("No content handler");**
        **}**

        **handler.startDocument();** 
        **handler.startElement(nsu, rootElement, rootElement, atts);**

        output("nickname", "xmozillanickname", line);
        // ...
        output("cell", "cellphone", line);

        **handler.ignorableWhitespace("\n".toCharArray(),** 
            **0,  // start index**
            **1   // length**
        **);** 
        handler.endElement(nsu, rootElement, rootElement);
        handler.endDocument(); 
    }
    catch (Exception e) {
        // ...
    }
}

```

Here, the application checks to make sure that the parser is properly configured with a <tt>ContentHandler</tt>. (For this application, we do not care about anything else). It then generates the events for the start of the document and the root element, and finishes by sending the end event for the root element and the end event for the document.

Two items are noteworthy at this point:

<li>
The <tt>setDocumentLocator</tt> event has not been sent, because that is optional. Were it important, that event would be sent immediately before the <tt>startDocument</tt> event.
</li>
<li>
An <tt>ignorableWhitespace</tt> event is generated before the end of the root element. This, too, is optional, but it drastically improves the readability of the output, as will be seen shortly. (In this case, the whitespace consists of a single newline, which is sent in the same way that characters are sent to the characters method: as a character array, a starting index, and a length).
</li>

Now that SAX events are being generated for the document and the root element, the next step is to modify the output method to generate the appropriate element events for each data item. Removing the call to <tt>System.out.println(name + ": " + text)</tt> and adding the following highlighted code achieves that:

```

void output(String name, String prefix, String line) 
    throws SAXException {

    int startIndex = 
    prefix.length() + 2;   // 2=length of ": "
    String text = line.substring(startIndex);

    **int textLength = line.length() - startIndex;**
    **handler.ignorableWhitespace (indent.toCharArray(),** 
        **0,    // start index**
        **indent.length()**
    **);**
    **handler.startElement(nsu, name, name /*"qName"*/, atts);**
    **handler.characters(line.toCharArray(),** 
        **startIndex,**
        **textLength;**
    **);**
    **handler.endElement(nsu, name, name);**
}

```

Because the <tt>ContentHandler</tt> methods can send <tt>SAXExceptions</tt> back to the parser, the parser must be prepared to deal with them. In this case, none are expected, so the application is simply allowed to fail if any occur.

The length of the data is then calculated, again generating some ignorable whitespace for readability. In this case, there is only one level of data, so we can use a fixed-indent string. (If the data were more structured, we would have to calculate how much space to indent, depending on the nesting of the data).

**Note -** The indent string makes no difference to the data but will make the output a lot easier to read. Without that string, all the elements would be concatenated end to end:

```

&lt;addressbook&gt;
&lt;nickname&gt;Fred&lt;/nickname&gt;
&lt;email&gt;...

```

Next, the following method configures the parser with the <tt>ContentHandler</tt> that is to receive the events it generates:

```

void output(String name, String prefix, String line)
    throws SAXException {
    //  ...
}

// Allow an application to register a content event handler.
**public void setContentHandler(ContentHandler handler) {**
    **this.handler = handler;**
**}**  

// Return the current content handler.
**public ContentHandler getContentHandler() {**
    **return this.handler;**
**}**

```

Several other methods must be implemented in order to satisfy the <tt>XmlReader</tt> interface. For the purpose of this exercise, null methods are generated for all of them. A production application, however, would require that the error handler methods be implemented to produce a more robust application. For this example, though, the following code generates null methods for them:

```

// Allow an application to register an error event handler.
public void setErrorHandler(ErrorHandler handler) { } 

// Return the current error handler.
public ErrorHandler getErrorHandler() { 
    return null; 
}

```

Then the following code generates null methods for the remainder of the <tt>XmlReader</tt> interface. (Most of them are of value to a real SAX parser but have little bearing on a data-conversion application like this one).

```

// Parse an XML document from a system identifier (URI).
public void parse(String systemId) throws IOException, SAXException 
{ } 

// Return the current DTD handler.
public DTDHandler getDTDHandler() { return null; } 

// Return the current entity resolver.
public EntityResolver getEntityResolver() { return null; } 

// Allow an application to register an entity resolver.
public void setEntityResolver(EntityResolver resolver) { } 

// Allow an application to register a DTD event handler.
public void setDTDHandler(DTDHandler handler) { } 

// Look up the value of a property.
public Object getProperty(String name) { return null; } 

// Set the value of a property.
public void setProperty(String name, Object value) { }  

// Set the state of a feature.
public void setFeature(String name, boolean value) { } 

// Look up the value of a feature.
public boolean getFeature(String name) { return false; }

```

You now have a parser you can use to generate SAX events. In the next section, you will use it to construct a SAX source object that will let you transform the data into XML.

<a name="gghik" id="gghik"></a>

## Using the Parser as a <tt>SAXSource</tt>

Given a SAX parser to use as an event source, you can construct a transformer to produce a result. In this section, <tt>TransformerApp</tt> will be updated to produce a stream output result, although it could just as easily produce a DOM result.

**Note -** Note: The code discussed in this section is in <tt>TransformationApp03.java</tt>, which is found in the <tt>xslt</tt> directory after you unzip [XSLT examples](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

**Note -** Note: The code discussed in this section is in <tt>TransformationApp03.java</tt>, which is found in the <tt>xslt</tt> directory after you unzip
[`XSLT examples`](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

To start with, <tt>TransformationApp03</tt> differs from <tt>TransformationApp02</tt> in the classes it needs to import to construct a <tt>SAXSource</tt> object. These classes are shown highlighted below. The DOM classes are no longer needed at this point, so have been discarded, although leaving them in does not do any harm.

```

import org.xml.sax.SAXException; 
import org.xml.sax.SAXParseException; 
**import org.xml.sax.ContentHandler;**
**import org.xml.sax.InputSource;**

**import javax.xml.transform.sax.SAXSource;** 
import javax.xml.transform.stream.StreamResult;

```

Next, instead of creating a DOM <tt>DocumentBuilderFactory</tt> instance, the application creates a SAX parser, which is an instance of the <tt>AddressBookReader</tt>:

```

public class TransformationApp03 {
    static Document document;  
    public static void main(String argv[]) {
        // ...
        // Create the sax "parser".
        **AddressBookReader saxReader = new AddressBookReader();**

        try {
            File f = new File(argv[0]);
            // ...
        }
        // ...
    }
}

```

Then, the following highlighted code constructs a <tt>SAXSource</tt> object

```

// Use a Transformer for output
// ...
Transformer transformer = tFactory.newTransformer();

// Use the parser as a SAX source for input
**FileReader fr = new FileReader(f);**
**BufferedReader br = new BufferedReader(fr);**
**InputSource inputSource = new InputSource(br);**
**SAXSource source = new SAXSource(saxReader, inputSource);**
StreamResult result = new StreamResult(System.out);
transformer.transform(source, result);

```

Here, <tt>TransformationApp03</tt> constructs a buffered reader (as mentioned earlier) and encapsulates it in an input source object. It then creates a <tt>SAXSource</tt> object, passing it the reader and the <tt>InputSource</tt> object, and passes that to the transformer.

When the application runs, the transformer configures itself as the <tt>ContentHandler</tt> for the SAX parser (the <tt>AddressBookReader</tt>) and tells the parser to operate on the <tt>inputSource</tt> object. Events generated by the parser then go to the transformer, which does the appropriate thing and passes the data on to the result object.

Finally, <tt>TransformationApp03</tt> does not generate exceptions, so the exception handling code seen in <tt>TransformationApp02</tt> is no longer present.

<a name="gghmh" id="gghmh"></a>

### Running the <tt>TransformationApp03</tt> Sample

<li>**Navigate to the <tt>samples</tt> directory.**
<pre><code>
% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>.
</code></pre>
</li>
<!--
1. **[Download the XSLT examples by clicking this link](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.**
-->
<li><b>
[`Download the XSLT examples by clicking this link`](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.</b></li>
<li>**Navigate to the <tt>xslt</tt> directory.**
<pre><code>
cd xslt
</code></pre>
</li>
<li>**Compile the <tt>TransformationApp03</tt> sample.**
Type the following command:
<pre><code>
% javac TransformationApp03.java
</code></pre>
</li>
<li>**Run the <tt>TransformationApp03</tt> sample on a data file you wish to convert to XML.**
In the case below, <tt>TransformationApp03</tt> is run on the <tt>PersonalAddressBook.ldif</tt> file, found in the <tt>xslt/data</tt> directory after you have unzipped the samples bundle.
<pre><code>
% java TransformationApp03 
  data/PersonalAddressBook.ldif
</code></pre>
You will see the following output:
<pre><code>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;addressbook&gt;
    &lt;nickname&gt;Fred&lt;/nickname&gt;
    &lt;email&gt;Fred@barneys.house&lt;/email&gt;
    &lt;html&gt;TRUE&lt;/html&gt;
    &lt;firstname&gt;Fred&lt;/firstname&gt;
    &lt;lastname&gt;Flintstone&lt;/lastname&gt;
    &lt;work&gt;999-Quarry&lt;/work&gt;
    &lt;home&gt;999-BedrockLane&lt;/home&gt;
    &lt;fax&gt;888-Squawk&lt;/fax&gt;
    &lt;pager&gt;777-pager&lt;/pager&gt;
    &lt;cell&gt;555-cell&lt;/cell&gt;
&lt;/addressbook&gt;
</code></pre>
As you can see, the LDIF format file <tt>PersonalAddressBook</tt> has been converted to XML!
</li>
