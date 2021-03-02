
# Binding XML Schemas 

This section describes the default XML-to-Java bindings used by JAXB. All of these bindings can be overridden globally or case-by-case by using a custom binding declaration. See the
[JAXB Specification](http://jaxb.java.net) for complete information about the default JAXB bindings.

## Simple Type Definitions

A schema component using a simple type definition typically binds
to a Java property. Because there are different kinds of schema
components, the following Java property attributes (common to the schema components) include:

	- Base type
	- Collection type, if any
	- Predicate

The rest of the Java property attributes are specified in the
schema component using the <tt>simple</tt> type definition.

## <a name="bnazs" id="bnazs"></a>Default Data Type Bindings

The following sections explain the default schema-to-Java,
<tt>JAXBElement</tt>, and Java-to-schema data type
bindings.

### <a name="bnazt" id="bnazt"></a>Schema-to-Java Mapping

The Java language provides a richer set of data types than the XML schema. The following table provides a mapping of XML data types to Java data types in JAXB.

Table:&#160;JAXB Mapping of XML Schema Built-in Data Types

### <tt>JAXBElement</tt> Object

When XML element information cannot be inferred by the derived Java representation of the XML content, a <tt>JAXBElement</tt> object is provided. This object has methods to get and set the object name and object value.

### <a name="bnazw" id="bnazw"></a>Java-to-Schema Mapping

<a name="bnazx" id="bnazx"></a>The following table shows the default mapping of Java classes to XML data types.

Table:&#160;JAXB Mapping of XML Data Types to Java Classes
