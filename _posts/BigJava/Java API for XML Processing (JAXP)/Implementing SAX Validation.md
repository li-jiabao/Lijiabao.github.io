
# Implementing SAX Validation

The sample program <tt>SAXLocalNameCount</tt> uses the non-validating parser by default, but it can also activate validation. Activating validation allows the application to tell whether the XML document contains the right tags or whether those tags are in the right sequence. In other words, it can tell you whether the document is *valid*. If validation is not activated, however, it can only tell whether or not the document is well-formed, as was shown in the previous section when you deleted the closing tag from an XML element. For validation to be possible, the XML document needs to be associated to a DTD or an XML schema. Both options are possible with the <tt>SAXLocalNameCount</tt> program.

<a name="gcwuk" id="gcwuk"></a>

## Choosing the Parser Implementation

If no other factory class is specified, the default <tt>SAXParserFactory</tt> class is used. To use a parser from a different manufacturer, you can change the value of the environment variable that points to it. You can do that from the command line:

`java -Djavax.xml.parsers.SAXParserFactory=*yourFactoryHere* [...]`

The factory name you specify must be a fully qualified class name (all package prefixes included). For more information, see the documentation in the <tt>newInstance()</tt> method of the <tt>SAXParserFactory</tt> class.

<a name="gcwub" id="gcwub"></a>

## Using the Validating Parser

Up until this point, this lesson has concentrated on the non-validating parser. This section examines the validating parser to find out what happens when you use it to parse the sample program.

Two things must be understood about the validating parser:

<li>
A schema or DTD is required.
</li>
<li>
Because the schema or DTD is present, the <tt>ContentHandler.</tt><tt>ignorableWhitespace()</tt> method is invoked whenever possible.
</li>

<a name="gcwth" id="gcwth"></a>

## Ignorable White Space

When a DTD is present, the parser will no longer call the <tt>characters()</tt> method on white space that it knows to be irrelevant. From the standpoint of an application that is interested in processing only the XML data, that is a good thing because the application is never bothered with white space that exists purely to make the XML file readable.

On the other hand, if you are writing an application that filters an XML data file and if you want to output an equally readable version of the file, then that white space would no longer be irrelevant: it would be essential. To get those characters, you would add the <tt>ignorableWhitespace</tt> method to your application. To process any (generally) ignorable white space that the parser sees, you would need to add something like the following code to implement the <tt>ignorableWhitespace</tt> event handler.

`public void ignorableWhitespace (char buf[], int start, int length) throws SAXException { emit("IGNORABLE"); }`

This code simply generates a message to let you know that ignorable white space was seen. However, not all parsers are created equal. The SAX specification does not require that this method be invoked. The Java XML implementation does so whenever the DTD makes it possible.

<a name="gcwtg" id="gcwtg"></a>

## Configuring the Factory

The <tt>SAXParserFactory</tt> needs to be set up such that it uses a validating parser instead of the default non-validating parser. The following code from the <tt>SAXLocalNameCount</tt> example's <tt>main()</tt> method shows how to configure the factory so that it implements the validating parser.

```

static public void main(String[] args) throws Exception {

    String filename = null;
    boolean dtdValidate = false;
    boolean xsdValidate = false;
    String schemaSource = null;

    for (int i = 0; i &lt; args.length; i++) {

        if (args[i].equals("-dtd")) {
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
        else if (args[i].equals("-usage")) {
            usage();
        }
        else if (args[i].equals("-help")) {
            usage();
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

    SAXParserFactory spf = SAXParserFactory.newInstance();
    spf.setNamespaceAware(true);
    spf.setValidating(dtdValidate || xsdValidate);
    SAXParser saxParser = spf.newSAXParser();

    // ... 
}

```

Here, the <tt>SAXLocalNameCount</tt> program is configured to take additional arguments when it is started, which tell it to implement no validation, DTD validation, XML Schema Definition (XSD) validation, or XSD validation against a specific schema source file. (Descriptions of these options, <tt>-dtd</tt>, <tt>-xsd</tt>, and <tt>-xsdss</tt> are also added to the <tt>usage()</tt> method, but are not shown here.) Then, the factory is configured so that it will produce the appropriate validating parser when <tt>newSAXParser</tt> is invoked. As seen in 
[Setting up the Parser](parsing.html#gclmt), you can also use <tt>setNamespaceAware(true)</tt> to configure the factory to return a namespace-aware parser. Sun's implementation supports any combination of configuration options. (If a combination is not supported by a particular implementation, it is required to generate a factory configuration error).

<a name="gcwtl" id="gcwtl"></a>

## Validating with XML Schema

Although a full treatment of XML Schema is beyond the scope of this tutorial, this section shows you the steps you take to validate an XML document using an existing schema written in the XML Schema language. To learn more about XML Schema, you can review the online tutorial, *XML Schema Part 0: Primer*, at 
[http://www.w3.org/TR/xmlschema-0/](http://www.w3.org/TR/xmlschema-0/).

**Note -** There are multiple schema-definition languages, including RELAX NG, Schematron, and the W3C "XML Schema" standard. (Even a DTD qualifies as a "schema," although it is the only one that does not use XML syntax to describe schema constraints.) However, "XML Schema" presents us with a terminology challenge. Although the phrase "XML Schema schema" would be precise, we will use the phrase "XML Schema definition" to avoid the appearance of redundancy.

To be notified of validation errors in an XML document, the parser factory must be configured to create a validating parser, as shown in the preceding section. In addition, the following must be true:

<li>
The appropriate properties must be set on the SAX parser.
</li>
<li>
The appropriate error handler must be set.
</li>
<li>
The document must be associated with a schema.
</li>

<a name="gcwtd" id="gcwtd"></a>

## Setting the SAX Parser Properties

It is helpful to start by defining the constants you will use when setting the properties. The <tt>SAXLocalNameCount</tt> example sets the following constants.

```

public class SAXLocalNameCount extends DefaultHandler {

    static final String JAXP_SCHEMA_LANGUAGE =
        "http://java.sun.com/xml/jaxp/properties/schemaLanguage";
    
    static final String W3C_XML_SCHEMA =
        "http://www.w3.org/2001/XMLSchema";

    static final String JAXP_SCHEMA_SOURCE =
        "http://java.sun.com/xml/jaxp/properties/schemaSource";
}

```

**Note -** The parser factory must be configured to generate a parser that is namespace-aware as well as validating. This was shown in [Configuring the Factory](#gcwtg). More information about namespaces is provided in 
[Document Object Model](../dom/index.html) but for now, understand that schema validation is a namespace-oriented process. Because JAXP-compliant parsers are not namespace-aware by default, it is necessary to set the property for schema validation to work.

Then you must configure the parser to tell it which schema language to use. In <tt>SAXLocalNameCount</tt>, validation can be performed either against a DTD or against an XML Schema. The following code uses the constants defined above to specify the W3C's XML Schema language as the one to use if the <tt>-xsd</tt> option is specified when the program is started.

```

// ...
if (xsdValidate) {
    saxParser.setProperty(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA);
    // ...
}

```

In addition to the error handling described in 
[Setting up Error Handling](parsing.html#gcnsr), there is one error that can occur when configuring the parser for schema-based validation. If the parser is not compliant with the JAXP spec, and therefore does not support XML Schema, it can throw a <tt>SAXNotRecognizedException</tt>. To handle that case, the <tt>setProperty()</tt> statement is wrapped in a try/catch block, as shown in the code below.

```

// ...
if (xsdValidate) {
    try {
        saxParser.setProperty(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA);
    }
    catch (SAXNotRecognizedException x){
        System.err.println("Error: JAXP SAXParser property not recognized: "
                           + JAXP_SCHEMA_LANGUAGE);

        System.err.println( "Check to see if parser conforms to the JAXP spec.");
        System.exit(1);
    }
}
// ...

```

<a name="gcwus" id="gcwus"></a>

## Associating a Document with a Schema

To validate the data using an XML Schema definition, it is necessary to ensure that the XML document is associated with one. There are two ways to do that.

- By including a schema declaration in the XML document.
- By specifying the schema to use in the application.

**Note -** When the application specifies the schema to use, it overrides any schema declaration in the document.

To specify the schema definition in the document, you would create XML such as this:

```

&lt;*documentRoot*
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation='*YourSchemaDefinition*.xsd'&gt;

```

The first attribute defines the XML namespace (<tt>xmlns</tt>) prefix, <tt>xsi</tt>, which stands for XML Schema instance. The second line specifies the schema to use for elements in the document that do not have a namespace prefix, namely for the elements that are typically defined in any simple, uncomplicated XML document.

**Note -** More information about namespaces is included in Validating with XML Schema in 
[Document Object Model](../dom/index.html). For now, think of these attributes as the "magic incantation" you use to validate a simple XML file that does not use them. After you have learned more about namespaces, you will see how to use XML Schema to validate complex documents that do use them. Those ideas are discussed in Validating with Multiple Namespaces in 
[Document Object Model](../dom/index.html).

You can also specify the schema file in the application, as is the case in <tt>SAXLocalNameCount</tt>.

```

// ...
if (schemaSource != null) {
    saxParser.setProperty(JAXP_SCHEMA_SOURCE, new File(schemaSource));
}

// ...

```

In the code above, the variable <tt>schemaSource</tt> relates to a schema source file that you can point the <tt>SAXLocalNameCount</tt> application to by starting it with the <tt>-xsdss</tt> option and providing the name of the schema source file to be used.

<a name="gcxhv" id="gcxhv"></a>

## Error Handling in the Validating Parser

It is important to recognize that the only reason an exception is thrown when a file fails validation is as a result of the error-handling code shown in 
[Setting up Error Handling](parsing.html#gcnsr). That code is reproduced here as a reminder:

```

// ...

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

// ...

```

If these exceptions are not thrown, the validation errors are simply ignored. In general, a SAX parsing error is a validation error, although it can also be generated if the file specifies a version of XML that the parser is not prepared to handle. Remember that your application will not generate a validation exception unless you supply an error handler such as the one here.

<a name="gcxhr" id="gcxhr"></a>

## DTD Warnings

As mentioned earlier, warnings are generated only when the SAX parser is processing a DTD. Some warnings are generated only by the validating parser. The non-validating parser's main goal is to operate as rapidly as possible, but it too generates some warnings.

The XML specification suggests that warnings should be generated as a result of the following:

<li>
Providing additional declarations for entities, attributes, or notations. (Such declarations are ignored. Only the first is used. Also, note that duplicate definitions of elements always produce a fatal error when validating, as you saw earlier.)
</li>
<li>
Referencing an undeclared element type. (A validity error occurs only if the undeclared type is actually used in the XML document. A warning results when the undeclared element is referenced in the DTD.)
</li>
<li>
Declaring attributes for undeclared element types.
</li>

The Java XML SAX parser also emits warnings in other cases:

<li>
No <tt>&lt;!DOCTYPE ...&gt;</tt> when validating.
</li>
<li>
References to an undefined parameter entity when not validating. (When validating, an error results. Although nonvalidating parsers are not required to read parameter entities, the Java XML parser does so. Because it is not a requirement, the Java XML parser generates a warning, rather than an error.)
</li>
<li>
Certain cases where the character-encoding declaration does not look right.
</li>

<a name="gcwvc" id="gcwvc"></a>

## Running the SAX Parser Examples with Validation

In this section, the <tt>SAXLocalNameCount</tt> sample program used previously will be used again, except this time it will be run with validation against an XML Schema or a DTD. The best way to demonstrate the different types of validation is to modify the code of the XML file being parsed, as well as the associated schema and DTDs, to break the processing and get the application to generate exceptions.

<a name="gcxic" id="gcxic"></a>

## Experimenting with DTD Validation Errors

As stated above, these examples reuse the <tt>SAXLocalNameCount</tt> program. The locations where you will find the sample and its associated files are given in 
[Running the SAX Parser Example without Validation](parsing.html#gcnrx).

1. **If you have not already done so, navigate to the <tt>samples</tt> directory.** `% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>`
1. **If you have not already done so, compile the example class.**`% javac sax/*`
<li>**Open the file <tt>data/rich_iii.xml</tt> in a text editor.**
<p>This is the same XML file that was processed without validation in 
[To Run the <tt>SAXLocalNameCount</tt> Example without Validation](parsing.html#gcvwx). At the beginning of <tt>data/rich_iii.xml</tt>, you will see that the <tt>DOCTYPE</tt> declaration points a validating parser to a DTD file called <tt>play.dtd</tt>. If DTD validation is activated, the structure of the XML file being parsed will be checked against the structure provided in <tt>play.dtd</tt>.</p>
</li>
<li>**Delete the declaration <tt>&lt;!DOCTYPE PLAY SYSTEM "play.dtd"&gt;</tt> from the beginning of the file.**
Do not forget to save the modification, but leave the file open, as it will be needed again later.
</li>
<li>**Run the <tt>SAXLocalNameCount</tt> program, with DTD validation activated.**
To do this, you must specify the <tt>-dtd</tt> option when you run the program.
`% java sax/SAXLocalNameCount -dtd data/rich_iii.xml`
The result you see will look something like this:
<pre><code>
Exception in thread "main" org.xml.sax.SAXException:
Error: URI=file:*install-dir*/JAXP_sources/jaxp-1_4_2-*release-date*
/samples/data/rich_iii.xml 
Line=12: Document is invalid: no grammar found.
</code></pre>
<hr />
**Note -** This message was generated by the JAXP 1.4.2 libraries. If you are using a different parser, the error message is likely to be somewhat different.
<hr />
This message says that there is no grammar against which the document <tt>rich_iii.txt</tt> can be validated, so therefore it is automatically invalid. In other words, the message is saying that you are trying to validate the document, but no DTD has been declared, because no <tt>DOCTYPE</tt> declaration is present. So now you know that a DTD is a requirement for a valid document. That makes sense.
</li>
<li>**Restore the <tt>&lt;!DOCTYPE PLAY SYSTEM "play.dtd"&gt;</tt> declaration to <tt>data/rich_iii.xml</tt>.**
Again, do not forget to save the file, but leave it open.
</li>
<li>**Return to <tt>data/rich_iii.xml</tt> and modify the tags for the character "KING EDWARD The Fourth" in line 26.**
Change the start and end tags from <tt>&lt;PERSONA&gt;</tt> and <tt>&lt;/PERSONA&gt;</tt> to <tt>&lt;PERSON&gt;</tt> and <tt>&lt;/PERSON&gt;</tt>. Line 26 should now look like this:
`26:&lt;PERSON&gt;KING EDWARD The Fourth&lt;/PERSON&gt;`
Again, do not forget to save the modification, and leave the file open.
</li>
<li>**Run the <tt>SAXLocalNameCount</tt> program, with DTD validation activated.**
This time, you will see a different error when you run the program:
<pre><code>
% java sax/SAXLocalNameCount -dtd data/rich_iii.xml
Exception in thread "main" org.xml.sax.SAXException: 
Error: URI=file:*install-dir*/JAXP_sources/jaxp-1_4_2-*release-date*
/samples/data/rich_iii.xml 
Line=26: Element type "PERSON" must be declared.
</code></pre>
Here you can see that the parser has objected to an element that is not included in the DTD <tt>data/play.dtd</tt>.
</li>
<li>**In <tt>data/rich_iii.xml</tt> correct the tags for "KING EDWARD The Fourth".**
Return the start and end tags to their original versions, <tt>&lt;PERSONA&gt;</tt> and <tt>&lt;/PERSONA&gt;</tt>.
</li>
<li>**In <tt>data/rich_iii.xml</tt>, delete <tt>&lt;TITLE&gt;Dramatis Personae&lt;/TITLE&gt;</tt> from line 24.**
Once more, do not forget to save the modification.
</li>
<li>**Run the <tt>SAXLocalNameCount</tt> program, with DTD validation activated.**
As before, you will see another validation error:
<pre><code>
java sax/SAXLocalNameCount -dtd data/rich_iii.xml
Exception in thread "main" org.xml.sax.SAXException: 
Error: URI=file:*install-dir*/JAXP_sources/jaxp-1_4_2-*release-date*
/samples/data/rich_iii.xml 
Line=85: The content of element type "PERSONAE" must match "(TITLE,(PERSONA|PGROUP)+)".
</code></pre>
By deleting the <tt>&lt;TITLE&gt;</tt> element from line 24, the <tt>&lt;PERSONAE&gt;</tt> element is rendered invalid because it does not contain the sub-elements that the DTD expects of a <tt>&lt;PERSONAE&gt;</tt> element. Note that the error message states that the error is in line 85 of <tt>data/rich_iii.xml</tt>, even though you deleted the <tt>&lt;TITLE&gt;</tt> element from line 24. This is because the closing tag of the <tt>&lt;PERSONAE&gt;</tt> element is located at line 85 and the parser only throws the exception when it reaches the end of the element it parsing.
</li>
<li>**Open the DTD file, <tt>data/play.dtd</tt> in a text editor.**
In the DTD file, you can see the declaration of the <tt>&lt;PERSONAE&gt;</tt> element, as well as all the other elements that can be used in XML documents that conform to the play DTD. The declaration of <tt>&lt;PERSONAE&gt;</tt> looks like this.
<pre><code>
&lt;!ELEMENT PERSONAE (TITLE, (PERSONA | PGROUP)+)&gt;
</code></pre>
As you can see, the <tt>&lt;PERSONAE&gt;</tt> element requires a <tt>&lt;TITLE&gt;</tt> sub-element. The pipe (<tt>|</tt>) key means that either <tt>&lt;PERSONA&gt;</tt> or <tt>&lt;PGROUP&gt;</tt> sub-elements can be included in a <tt>&lt;PERSONAE&gt;</tt> element, and the plus (<tt>+</tt>) key after the <tt>(PERSONA | PGROUP)</tt> grouping means that at least one or more of either of these sub-elements must be included.
</li>
<li>**Add a question mark (<tt>?</tt>) key after <tt>TITLE</tt> in the declaration of <tt>&lt;PERSONAE&gt;</tt>.**
Adding a question mark to a sub-element's declaration in a DTD makes the presence of one instance of that sub-element optional.
<pre><code>
&lt;!ELEMENT PERSONAE (TITLE?, (PERSONA | PGROUP)+)&gt;
</code></pre>
If you were add an asterisk (<tt>*</tt>) after the element, you could include either zero or multiple instances of that sub-element. However, in this case it does not make sense to have more than one title in a section of a document.
Do not forget to save the modification you have made to <tt>data/play.dtd</tt>.
</li>
<li>**Run the <tt>SAXLocalNameCount</tt> program, with DTD validation activated.**`% java sax/SAXLocalNameCount -dtd data/rich_iii.xml`
This time, you should see the proper output of <tt>SAXLocalNameCount</tt>, with no errors.
</li>

<a name="gcxhp" id="gcxhp"></a>

## Experimenting with Schema Validation Errors

The previous exercise demonstrated using <tt>SAXLocalNameCount</tt> to validate an XML file against a DTD. In this exercise you will use <tt>SAXLocalNameCount</tt> to validate a different XML file against both the standard XML Schema definition and a custom schema source file. Again, this type of validation will be demonstrated by breaking the parsing process by modifying the XML file and the schema, so that the parser throws errors.

As stated above, these examples reuse the <tt>SAXLocalNameCount</tt> program. The locations where you will find the sample and its associated files are given in 
[Running the SAX Parser Example without Validation](parsing.html#gcnrx).

1. **If you have not already done so, navigate to the <tt>samples</tt> directory.**`% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>`
1. **If you have not already done so, compile the example class.**`% javac sax/*`
<li>**Open the file <tt>data/personal-schema.xml</tt> in a text editor.**
This is a simple XML file that provides the names and contact details for the employees of a small company. In this XML file, you will see that it has been associated with a schema definition file <tt>personal.xsd</tt>.
`&lt;personnel xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation='personal.xsd'&gt;`</li>
<li>**Open the file <tt>data/personal.xsd</tt> in a text editor.**
This schema defines what kinds of information are required about each employee in order for an XML document associated with the schema to be considered valid. For example, by examining the schema definition, you can see that each <tt>person</tt> element requires a <tt>name</tt>, and that each person's name must comprise a <tt>family</tt> name and a <tt>given</tt> name. Employees can also optionally have email addresses and URLs.
</li>
<li>**In <tt>data/personal.xsd</tt>, change the minimum number of email addresses required for a <tt>person</tt> element from <tt>0</tt> to <tt>1</tt>.**
The declaration of the <tt>email</tt> element is now as follows.
`&lt;xs:element ref="email" minOccurs='1' maxOccurs='unbounded'/&gt;`</li>
<li>**In <tt>data/personal-schema.xml</tt>, delete the <tt>email</tt> element from the <tt>person</tt> element <tt>one.worker</tt>.**
Worker One now looks like this:
<pre><code>
&lt;person id="one.worker"&gt;
  &lt;name&gt;&lt;family&gt;Worker&lt;/family&gt; &lt;given&gt;One&lt;/given&gt;&lt;/name&gt;
  &lt;link manager="Big.Boss"/&gt;
&lt;/person&gt;
</code></pre>
</li>
<li>**Run <tt>SAXLocalNameCount</tt> against <tt>personal-schema.xml</tt>, with no schema validation.**`% java sax/SAXLocalNameCount data/personal-schema.xml`
<tt>SAXLocalNameCount</tt> informs you of the number of times each element occurs in <tt>personal-schema.xml</tt>.
<pre><code>
Local Name "email" occurs 5 times
Local Name "name" occurs 6 times
Local Name "person" occurs 6 times
Local Name "family" occurs 6 times
Local Name "link" occurs 6 times
Local Name "personnel" occurs 1 times
Local Name "given" occurs 6 times
</code></pre>
You see that <tt>email</tt> only occurs five times, whereas there are six <tt>person</tt> elements in <tt>personal-schema.xml</tt>. So, because we set the minimum occurrences of the <tt>email</tt> element to 1 per <tt>person</tt> element, we know that this document is invalid. However, because <tt>SAXLocalNameCount</tt> was not told to validate against a schema, no error is reported.
</li>
<li>**Run <tt>SAXLocalNameCount</tt> again, this time specifying that the <tt>personal&#8212;schema.xml</tt> document should be validated against a the <tt>personal.xsd</tt> schema definition.**
As you saw in [Validating with XML Schema](#gcwtl) above, <tt>SAXLocalNameCount</tt> has an option to enable schema validation. Run <tt>SAXLocalNameCount</tt> with the following command.
`% java sax/SAXLocalNameCount -xsd data/personal-schema.xml`
This time, you will see the following error message.
<pre><code>
Exception in thread "main" org.xml.sax.SAXException: Error: 
URI=file:*install_dir*/samples/data/personal-schema.xml 
Line=19: cvc-complex-type.2.4.a: Invalid content was found starting with 
element 'link'. 
One of '{email}' is expected.
</code></pre>
</li>
1. **Restore the <tt>email</tt> element to the <tt>person</tt> element <tt>one.worker</tt>.**
<li>**Run <tt>SAXLocalNameCount</tt> a third time, again specifying that the <tt>personal&#8212;schema.xml</tt> document should be validated against a the <tt>personal.xsd</tt> schema definition.**`% java sax/SAXLocalNameCount -xsd data/personal-schema.xml`
This time you will see the correct output, with no errors.
</li>
1. **Open <tt>personal-schema.xml</tt> in a text editor again.**
<li>**Delete the declaration of the schema definition <tt>personal.xsd</tt> from the <tt>personnel</tt> element.**
Remove the italicized code from the <tt>personnel</tt> element.
`&lt;personnel *xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation='personal.xsd'/*&gt;`</li>
<li>**Run <tt>SAXLocalNameCount</tt>, again specifying schema validation.**`% java sax/SAXLocalNameCount -xsd data/personal-schema.xml`
Obviously, this will not work, as the schema definition against which to validate the XML file has not been declared. You will see the following error.
<pre><code>
Exception in thread "main" org.xml.sax.SAXException: 
Error: URI=file:*install_dir*/samples/data/personal-schema.xml 
Line=8: cvc-elt.1: Cannot find the declaration of element 'personnel'.
</code></pre>
</li>
<li>**Run <tt>SAXLocalNameCount</tt> again, this time passing it the schema definition file at the command line.**`% java sax/SAXLocalNameCount -xsdss data/personal.xsd data/personal-schema.xml`
This time you use the <tt>SAXLocalNameCount</tt> option that allows you to specify a schema definition that is not hard-coded into the application. You should see the correct output.
</li>
