
# Using the Limits

## Environment Evaluation


    Evaluation includes, at the system level, the amount of memory available for applications, whether XML, XSD, or XSL sources from untrusted sources are accepted and processed, and at the application level, whether certain constructs such as DTDs are used.

## Memory Setting and Limits

    
XML processing can be very memory intensive.  The amount of memory that should be allowed to be consumed depends on the requirements of the applications in a specific environment. Processing of malformed XML data must be prevented from consuming excessive memory.



The default limits are generally set to allow legitimate XML inputs for most applications with memory usage allowed for a small hardware system, such as a PC. It is recommended that the limits are set to the smallest possible values, so that any malformed input can be caught before it consumes large amount of memory.



The limits are correlated, but not entirely redundant. You should set appropriate values for all of the limits: usually the limits should be set to a much smaller value than the default. 



As an example, the <tt>ENTITY_EXPANSION_LIMIT</tt> and <tt>GENERAL_ENTITY_SIZE_LIMIT</tt> can be set to prevent excessive entity references. But when the exact combination of the expansion and entity sizes are unknown, the <tt>TOTAL_ENTITY_SIZE_LIMIT</tt> can serve as a overall control. Similarly, while <tt>TOTAL_ENTITY_SIZE_LIMIT</tt> controls the total size of a replacement text, if the text is a very large chunk of XML, the <tt>ENTITY_REPLACEMENT_LIMIT</tt> sets a restriction on the total number of nodes that can appear in the text and prevents overloading the system.


## Estimating the Limits Using the getEntityCountInfo Property


To help you analyze what values you should set for the limits, a special property called <tt>http://www.oracle.com/xml/jaxp/properties/getEntityCountInfo</tt> is available. The following code snippet shows an example of using the property:

```

parser.setProperty("http://www.oracle.com/xml/jaxp/properties/getEntityCountInfo", "yes");

```


See
[Samples](sample.html) for more information on downloading the example code.


When the program is run with the DTD in W3C MathML 3.0, it prints out the following table:
<th id="h1">Property</th><th id="h2">Limit</th><th id="h3">Total Size</th><th id="h4">Size</th><th id="h5">Entity Name</th>
<td headers="h1"><tt>ENTITY_EXPANSION_LIMIT</tt></td><td headers="h2">64000</td><td headers="h3">1417</td><td headers="h4">0</td><td headers="h5"><tt>null</tt></td>
<td headers="h1"><tt>MAX_OCCUR_NODE_LIMIT</tt></td><td headers="h2">5000</td><td headers="h3">0</td><td headers="h4">0</td> <td headers="h5"><tt>null</tt></td>
<td headers="h1"><tt>ELEMENT_ATTRIBUTE_LIMIT</tt></td><td headers="h2">10000</td><td headers="h3">0</td><td headers="h4">0</td><td headers="h5"><tt>null</tt></td>
<td headers="h1"><tt>TOTAL_ENTITY_SIZE_LIMIT</tt></td><td headers="h2">50000000</td><td headers="h3">55425</td><td headers="h4">0</td><td headers="h5"><tt>null</tt></td>
<td headers="h1"><tt>GENERAL_ENTITY_SIZE_LIMIT</tt></td><td headers="h2">0</td><td headers="h3">0</td><td headers="h4">0</td><td headers="h5"><tt>null</tt></td>
<td headers="h1"><tt>PARAMETER_ENTITY_SIZE_LIMIT</tt></td><td headers="h2">1000000</td><td headers="h3">0</td><td headers="h4">7303</td><td headers="h5">%MultiScriptExpression</td>
<td headers="h1"><tt>MAX_ELEMENT_DEPTH_LIMIT</tt></td><td headers="h2">0</td><td headers="h3">2</td><td headers="h4">0</td><td headers="h5"><tt>null</tt></td>
<td headers="h1"><tt>MAX_NAME_LIMIT</tt></td><td headers="h2">1000</td><td headers="h3">13</td><td headers="h4">13</td><td headers="h5"><tt>null</tt></td>
<td headers="h1"><tt>ENTITY_REPLACEMENT_LIMIT</tt></td><td headers="h2">3000000</td><td headers="h3">0</td><td headers="h4">0</td><td headers="h5"><tt>null</tt></td>


In this example, the total number of entity references, or the entity expansion, is 1417; the default limit is 64000. The total size of all entities is 55425; the default limit is 50000000. The biggest parameter entity is <tt>%MultiScriptExpression</tt> with a length of 7303 after all references are resolved; the default limit is 1000000.


If this is the largest file that the application is expected to process, it is recommended that the limits be set to smaller numbers. For example, 2000 for <tt>ENTITY_EXPANSION_LIMIT</tt>, 100000 for <tt>TOTAL_ENTITY_SIZE_LIMIT</tt>, and 10000 for <tt>PARAMETER_ENTITY_SIZE_LIMIT</tt>.

## Setting Limits


Limits can be set in the same way as other JAXP properties. They can be set through factory methods, or through the parser:

```

DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
dbf.setAttribute(name, value);
 
SAXParserFactory spf = SAXParserFactory.newInstance();
SAXParser parser = spf.newSAXParser();
parser.setProperty(name, value);
 
XMLInputFactory xif = XMLInputFactory.newInstance();
xif.setProperty(name, value);
 
SchemaFactory schemaFactory = SchemaFactory.newInstance(schemaLanguage);
schemaFactory.setProperty(name, value);
 
TransformerFactory factory = TransformerFactory.newInstance();
factory.setAttribute(name, value);

```


The following example shows how to set limits using <tt>DocumentBuilderFactory</tt>:

```

dbf.setAttribute(JDK_ENTITY_EXPANSION_LIMIT, "2000");
dbf.setAttribute(TOTAL_ENTITY_SIZE_LIMIT, "100000");
dbf.setAttribute(PARAMETER_ENTITY_SIZE_LIMIT, "10000"); 
dbf.setAttribute(JDK_MAX_ELEMENT_DEPTH, "100"); 

```

## Using System Properties


System properties may be useful if changing code is not feasible.


To set limits for an entire invocation of the JDK or JRE, set the system properties on the command line. To set the limits for only a portion of the application, the system properties may be set before the section and cleared afterwards. The following code shows how system properties may be used:

```

public static final String SP_GENERAL_ENTITY_SIZE_LIMIT = "jdk.xml.maxGeneralEntitySizeLimit";

//set limits using system property
System.setProperty(SP_GENERAL_ENTITY_SIZE_LIMIT, "2000");

//this setting will affect all processing after it's set
...

//after it is done, clear the property
System.clearProperty(SP_GENERAL_ENTITY_SIZE_LIMIT);

```


Note that the values of the properties are expected to be integers. A <tt>NumberFormatException</tt> will be thrown if
the value entered does not contain a parsable integer; see the method 
[`parseInt(String)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#parseInt-java.lang.String-).



See
[Samples](sample.html) for more information on downloading the example code.

## Using the jaxp.properties File


The <tt>jaxp.properties</tt> file is a configuration file. It is usually located at <tt>${**java.home**}/lib/jaxp.properties</tt> where <tt>**java.home**</tt> is the JRE install directory, e.g., **[path to installation directory]**/jdk8/jre.


A limit can be set by adding the following line to the <tt>jaxp.properties</tt> file:

```

jdk.xml.maxGeneralEntitySizeLimit=2000

```


Note that the property name is the same as that of the system property and has the prefix <tt>jdk.xml</tt>.
The values of the properties are
expected to be integers. A <tt>NumberFormatException</tt> will be thrown if the value entered does not contain a parsable integer; see the method 
[`parseInt(String)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#parseInt-java.lang.String-).



When the property is set in the file, all invocations of the JDK and JRE will observe the limit.
