
# JAXB Examples

The following sections describe how to use the sample applications that are included in the JAXB RI bundle. The JAXB RI bundle is available from 
[http://jaxb.java.net](http://jaxb.java.net). Download and install the JAXB RI bundle. The examples are located in the *jaxb-ri-install*<tt>/samples/</tt> directory. These examples demonstrate and build upon key JAXB features and concepts. Follow these procedures in the order presented.

After reading this section, you should feel comfortable enough
with JAXB that you can:

	- Generate JAXB Java classes from an XML schema
	<li>Use schema-derived JAXB classes to unmarshal and marshal XML
	content in a Java application</li>
	<li>Create a Java content tree using schema-derived JAXB classes
	</li>
	- Validate XML content during unmarshalling and at runtime
	- Customize JAXB schema-to-Java bindings

This document describes three sets of examples:

	<li>The basic examples (Modify Marshal, Unmarshal Validate)
	demonstrate basic JAXB concepts such as unmarshalling, marshalling, 		and validating XML content using default settings and bindings.</li>
	<li>The customize examples (Customize Inline, Datatype Converter,
	External Customize) demonstrate various ways of customizing the
	default binding of XML schemas to Java objects.</li>
	<li>The java-to-schema examples show how to use annotations to
	map Java classes to an XML schema.
	</li>

The basic and customize example directories contain several base files:

- <tt>po.xsd</tt> is the XML schema that is used as input to the JAXB binding compiler, and from which schema-derived JAXB Java classes will be generated. For the Customize Inline and Datatype Converter examples, this file contains inline binding customizations.
- <tt>po.xml</tt> is the Purchase Order XML file containing sample XML content, and it is the file unmarshalled into a Java content tree in each example. This file is almost the same in each example; there are minor content differences to highlight different JAXB concepts.
- <tt>Main.java</tt> is the main Java class for each example.
- <tt>build.xml</tt> is an Ant project file provided for your convenience. Use the Ant tool to generate, compile, and run the schema-derived JAXB classes automatically. The <tt>build.xml</tt> file varies across the examples.
- <tt>MyDatatypeConverter.java</tt> in the <tt>inline-customize</tt> example is a Java class used to provide custom data type conversions.
- <tt>binding.xjb</tt> in the External Customize example is an external binding declarations file that is passed to the JAXB binding compiler to customize the default JAXB bindings.

The following tables briefly describe the basic, customize, and java-to-schema JAXB examples.

Table:&#160;Basic JAXB Examples

Table:&#160; Customize JAXB Examples

Table:&#160; Java-to-Schema JAXB Examples

## <a name="bnbal" id="bnbal"></a>JAXB Compiler Options

The JAXB XJC schema binding compiler transforms, or binds, a source XML schema to a set of JAXB content classes in the Java programming language.
The compiler class, <tt>xjc</tt>, is provided as: <tt>xjc.sh</tt> on Solaris/Linux and <tt>xjc.bat</tt> on Windows in the JAXB RI bundle. The <tt>xjc</tt> class is included in the JDK class library (in tools.jar).

Both <tt>xjc.sh</tt> and <tt>xjc.bat</tt> take the same command-line options. You can display quick usage instructions by invoking the scripts without any options, or with the <tt>-help</tt> switch. The syntax is as follows:

```

xjc [-options ...] &lt;schema file/URL/dir/jar&gt;... [-b &gt;bindinfo&lt;] ...

```

If <tt>dir</tt> is specified, all schema files in the directory will be compiled. If <tt>jar</tt> is specified, /META-INF/sun-jaxb.episode binding file will be compiled.

The <tt>xjc</tt> command-line options are as follows:

## <a name="bnban" id="bnban"></a>JAXB Schema Generator Option

The JAXB Schema Generator, <tt>schemagen</tt>, creates a schema file for each namespace referenced in your Java classes. The schema generator can be started by using the appropriate <tt>schemagen</tt> shell script in the <tt>bin</tt> directory for your platform. The schema generator processes Java source files only. If your Java sources reference other classes, those sources must be accessible from your system CLASSPATH environment variable; otherwise errors will occur when the schema is generated. There is no way to control the name of the generated schema files.

You can display quick usage instructions by invoking the scripts
without any options or by using the <tt>-help</tt> option. The syntax is as follows:

```

schemagen [-d *path*] 
    [*java-source-files*]

```

The <tt>-d</tt> *path* option specifies the location of the processor-generated and <tt>javac</tt>-generated class files.

## <a name="bnbao" id="bnbao"></a>About the Schema-to-Java Bindings

When you run the JAXB binding compiler against the <tt>po.xsd</tt>
XML schema used in the basic examples (Unmarshal Read, Modify Marshal, Unmarshal Validate), the JAXB binding compiler generates a Java package named <tt>primer.po</tt> containing the classes, described in the following table.

Table:&#160; Schema-Derived JAXB Classes in the Basic Examples

These classes and their specific bindings to the source XML schema
for the basic examples are described in the following table.

Table:&#160;Schema-to-Java Bindings for the Basic Examples

```

&lt;xsd:schema xmlns:xsd=
 "http://www.w3.org/2001/XMLSchema"&gt;

```

```

&lt;xsd:complexType 
  name="PurchaseOrderType"&gt;
  &lt;xsd:sequence&gt;
    &lt;xsd:element 
      name="shipTo" 
      type="USAddress"/&gt;
    &lt;xsd:element 
      name="billTo" 
      type="USAddress"/&gt;
    &lt;xsd:element 
      ref="comment" 
      minOccurs="0"/&gt;
    &lt;xsd:element 
      name="items"
      type="Items"/&gt;
  &lt;/xsd:sequence&gt;
  &lt;xsd:attribute 
    name="orderDate"
    type="xsd:date"/&gt;
&lt;/xsd:complexType&gt;

```

```

&lt;xsd:complexType 
  name="USAddress"&gt;
  &lt;xsd:sequence&gt;
    &lt;xsd:element 
      name="name" 
      type="xsd:string"/&gt;
    &lt;xsd:element 
      name="street" 
      type="xsd:string"/&gt;
    &lt;xsd:element 
      name="city" 
      type="xsd:string"/&gt;
    &lt;xsd:element 
      name="state" 
      type="xsd:string"/&gt;
    &lt;xsd:element 
      name="zip" 
      type="xsd:decimal"/&gt;
  &lt;/xsd:sequence&gt;
  &lt;xsd:attribute 
    name="country" 
    type="xsd:NMTOKEN" 
    fixed="US"/&gt;
&lt;/xsd:complexType&gt;

```

```

&lt;xsd:complexType 
  name="Items"&gt;
  &lt;xsd:sequence&gt;
    &lt;xsd:element 
      name="item" 
      minOccurs="1" 
      maxOccurs="unbounded"&gt;

```

```

&lt;xsd:complexType&gt;
  &lt;xsd:sequence&gt;
    &lt;xsd:element 
      name="productName" 
      type="xsd:string"/&gt;
    &lt;xsd:element 
      name="quantity"&gt;
      &lt;xsd:simpleType&gt;
        &lt;xsd:restriction 
          base="xsd:positiveInteger"&gt;
          &lt;xsd:maxExclusive 
            value="100"/&gt;
        &lt;/xsd:restriction&gt;
      &lt;/xsd:simpleType&gt;
    &lt;/xsd:element&gt;
    &lt;xsd:element 
      name="USPrice" 
      type="xsd:decimal"/&gt;
    &lt;xsd:element 
      ref="comment" 
      minOccurs="0"/&gt;
    &lt;xsd:element 
      name="shipDate" 
      type="xsd:date" 
      minOccurs="0"/&gt;
  &lt;/xsd:sequence&gt;
  &lt;xsd:attribute 
    name="partNum" 
    type="SKU" 
    use="required"/&gt;
&lt;/xsd:complexType&gt;

```

```

&lt;/xsd:element&gt;
&lt;/xsd:sequence&gt;
&lt;/xsd:complexType&gt;

```

```

&lt;!-- Stock Keeping Unit, a code for 
    identifying products --&gt;

```

```

&lt;xsd:simpleType 
  name="SKU"&gt;
  &lt;xsd:restriction 
    base="xsd:string"&gt;
    &lt;xsd:pattern 
      value="\d{3}-[A-Z]{2}"/&gt;
  &lt;/xsd:restriction&gt;
&lt;/xsd:simpleType&gt;

```

```

&lt;/xsd:schema&gt;

```

## Schema-Derived JAXB Classes

The next sections briefly explain the functions of the following individual classes generated by the JAXB binding compiler for the examples:


- [<tt>Items</tt> Class](#bnbat)
- [<tt>ObjectFactory</tt> Class](#bnbau)
- [<tt>PurchaseOrderType</tt> Class](#bnbaw)
- [<tt>USAddress</tt> Class](#bnbax)

### <a name="bnbat" id="bnbat"></a><tt>Items</tt> Class

In <tt>Items.java</tt>:

	<li>The <tt>Items</tt> class is part of the
	<tt>primer.po</tt> package.</li>
	<li>The class provides public interfaces for <tt>Items</tt>
	and <tt>ItemType</tt>.</li>
	<li>Content in instantiations of this class binds to the XML
	ComplexTypes <tt>Items</tt> and its child element 
	<tt>ItemType</tt>.</li>
	- <tt>Item</tt> provides the <tt>getItem()</tt> method.
	<li><tt>ItemType</tt> provides methods for:
	<ul>
	- <tt>getPartNum();</tt>
	- <tt>setPartNum(String value);</tt>
	- <tt>getComment();</tt>
	- <tt>setComment(java.lang.String value);</tt>
	- <tt>getUSPrice();</tt>
	- <tt>setUSPrice(java.math.BigDecimal value);</tt>
	- <tt>getProductName();</tt>
	- <tt>setProductName(String value);</tt>
	- <tt>getShipDate();</tt>
	- <tt>setShipDate(java.util.Calendar value);</tt>
	- <tt>getQuantity();</tt>
	- <tt>setQuantity(java.math.BigInteger value);</tt>
	
### <a name="bnbau" id="bnbau"></a><tt>ObjectFactory</tt> Class

In <tt>ObjectFactory.java</tt>:

- The <tt>ObjectFactory</tt> class is part of the <tt>primer.po</tt> package.
- <tt>ObjectFactory</tt> provides factory methods for instantiating Java interfaces representing XML content in the Java content tree.
<li>Method names are generated by concatenating:
<ul>
- The string constant <tt>create</tt>.
- All outer Java class names, if the Java content interface is nested within another interface.
- The name of the Java content interface.

As an example, in this case, for the Java interface <tt>primer.po.Items.ItemType</tt>, the <tt>ObjectFactory</tt> creates the method <tt>createItemsItemType()</tt>.

### <a name="bnbaw" id="bnbaw"></a><tt>PurchaseOrderType</tt> Class

In <tt>PurchaseOrderType.java</tt>:

- The <tt>PurchaseOrderType</tt> class is part of the <tt>primer.po</tt> package.
- Content in instantiations of this class binds to the XML schema child element named <tt>PurchaseOrderType</tt>.
<li><tt>PurchaseOrderType</tt> is a public interface that provides the following methods:
<ul>
- <tt>getItems();</tt>
- <tt>setItems(primer.po.Items value);</tt>
- <tt>getOrderDate();</tt>
- <tt>setOrderDate(java.util.Calendar value);</tt>
- <tt>getComment();</tt>
- <tt>setComment(java.lang.String value);</tt>
- <tt>getBillTo();</tt>
- <tt>setBillTo(primer.po.USAddress value);</tt>
- <tt>getShipTo();</tt>
- <tt>setShipTo(primer.po.USAddress value);</tt>

### <a name="bnbax" id="bnbax"></a><tt>USAddress</tt> Class

In <tt>USAddress.java</tt>:

	<li>The <tt>USAddress</tt> class is part of the
	<tt>primer.po</tt> package.</li>
	<li>Content in instantiations of this class binds to the XML
	schema element named <tt>USAddress</tt>.</li>
	<li><tt>USAddress</tt> is a public interface that
	provides the following methods:
	<ul>
		- <tt>getState();</tt>
		- <tt>setState(String value);</tt>
		- <tt>getZip();</tt>
		- <tt>setZip(java.math.BigDecimal value);</tt>
		- <tt>getCountry();</tt>
		- <tt>setCountry(String value);</tt>
		- <tt>getCity();</tt>
		- <tt>setCity(String value);</tt>
		- <tt>getStreet();</tt>
		- <tt>setStreet(String value);</tt>
		- <tt>getName();</tt>
		- <tt>setName(String value);</tt>
	