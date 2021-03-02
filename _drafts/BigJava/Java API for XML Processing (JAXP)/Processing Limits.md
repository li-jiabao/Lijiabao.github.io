
# Processing Limit Definitions

The following list describes the JAXP XML Processing Limits supported in the JDK. 
These limits can be specified through the factory APIs, system properties, and the 
<tt>jaxp.properties</tt> file.

## entityExpansionLimit
<th id="h1">Attribute</th><th id="h2">Description</th>
<td headers="h1">Name</td><td headers="h2"><tt>http://www.oracle.com/xml/jaxp/properties/entityExpansionLimit</tt></td>
<td headers="h1">Definition</td><td headers="h2">Limit the number of entity expansions.</td>
<td headers="h1">Value</td><td headers="h2">A positive integer. A value less than or equal to 0 indicates no limit. If the value is notan integer, a <tt>NumericFormatException</tt> is thrown.</td>
<td headers="h1">Default value</td><td headers="h2">64000</td>
<td headers="h1">System property</td><td headers="h2"><tt>jdk.xml.entityExpansionLimit</tt></td>
<td headers="h1">Since</td><td headers="h2">7u45, 8</td>

## elementAttributeLimit
<th id="h101">Attribute</th><th id="h102">Description</th>
<td headers="h101">Name</td><td headers="h102"><tt>http://www.oracle.com/xml/jaxp/properties/elementAttributeLimit</tt></td>
<td headers="h101">Definition</td><td headers="h102">Limit the number of attributes an element can have.</td>
<td headers="h101">Value</td><td headers="h102">A positive integer. A value less than or equal to 0 indicates no limit. If the value is notan integer, a <tt>NumericFormatException</tt> is thrown.</td>
<td headers="h101">Default value</td><td headers="h102">10000</td>
<td headers="h101">System property</td><td headers="h102"><tt>jdk.xml.elementAttributeLimit</tt></td>
<td headers="h101">Since</td><td headers="h102">7u45, 8</td>

## maxOccurLimit
<th id="h201">Attribute</th><th id="h202">Description</th>
<td headers="h201">Name</td><td headers="h202"><tt>http://www.oracle.com/xml/jaxp/properties/maxOccurLimit</tt></td>
<td headers="h201">Definition</td><td headers="h202">Limit the number of content model nodes that may be created when building a grammar for a W3C  XML Schema that contains <tt>maxOccurs</tt> attributes with values other than "unbounded".</td>
<td headers="h201">Value</td><td headers="h202">A positive integer. A value less than or equal to 0 indicates no limit. If the value is notan integer, a <tt>NumericFormatException</tt> is thrown.</td>
<td headers="h201">Default value</td><td headers="h202">5000</td>
<td headers="h201">System property</td><td headers="h202"><tt>jdk.xml.maxOccurLimit</tt></td>
<td headers="h201">Since</td><td headers="h202">7u45, 8</td>

## totalEntitySizeLimit
<th id="h301">Attribute</th><th id="h302">Description</th>
<td headers="h301">Name</td><td headers="h302"><tt>http://www.oracle.com/xml/jaxp/properties/totalEntitySizeLimit</tt></td>
<td headers="h301">Definition</td><td headers="h302">Limit the total size of all entities that include general and parameter entities. The size is calculated as an aggregation of all entities.</td>
<td headers="h301">Value</td><td headers="h302">A positive integer. A value less than or equal to 0 indicates no limit. If the value is notan integer, a <tt>NumericFormatException</tt> is thrown.</td>
<td headers="h301">Default value</td><td headers="h302">5x10^7</td>
<td headers="h301">System property</td><td headers="h302"><tt>jdk.xml.totalEntitySizeLimit</tt></td>
<td headers="h301">Since</td><td headers="h302">7u45, 8</td>

## maxGeneralEntitySizeLimit
<th id="h401">Attribute</th><th id="h402">Description</th>
<td headers="h401">Name</td><td headers="h402"><tt>http://www.oracle.com/xml/jaxp/properties/maxGeneralEntitySizeLimit</tt></td>
<td headers="h401">Definition</td><td headers="h402">Limit the maximum size of any general entities.</td>
<td headers="h401">Value</td><td headers="h402">A positive integer. A value less than or equal to 0 indicates no limit. If the value is notan integer, a <tt>NumericFormatException</tt> is thrown.</td>
<td headers="h401">Default value</td><td headers="h402">0</td>
<td headers="h401">System property</td><td headers="h402"><tt>jdk.xml.maxGeneralEntitySizeLimit</tt></td>
<td headers="h401">Since</td><td headers="h402">7u45, 8</td>

## maxParameterEntitySizeLimit
<th id="h501">Attribute</th><th id="h502">Description</th>
<td headers="h501">Name</td><td headers="h502"><tt>http://www.oracle.com/xml/jaxp/properties/maxParameterEntitySizeLimit</tt></td>
<td headers="h501">Definition</td><td headers="h502">Limit the maximum size of any parameter entities, including the result of nesting multiple parameter entities.</td>
<td headers="h501">Value</td><td headers="h502">A positive integer. A value less than or equal to 0 indicates no limit. If the value is notan integer, a <tt>NumericFormatException</tt> is thrown.</td>
<td headers="h501">Default value</td><td headers="h502">1000000</td>
<td headers="h501">System property</td><td headers="h502"><tt>jdk.xml.maxParameterEntitySizeLimit</tt></td>
<td headers="h501">Since</td><td headers="h502">7u45, 8</td>

## entityReplacementLimit
<th id="h601">Attribute</th><th id="h602">Description</th>
<td headers="h601">Name</td><td headers="h602"><tt>http://www.oracle.com/xml/jaxp/properties/entityReplacementLimit</tt></td>
<td headers="h601">Definition</td><td headers="h602">Limit the total number of nodes in all entity references.</td>
<td headers="h601">Value</td><td headers="h602">A positive integer. A value less than or equal to 0 indicates no limit. If the value is notan integer, a <tt>NumericFormatException</tt> is thrown.</td>
<td headers="h601">Default value</td><td headers="h602">3000000</td>
<td headers="h601">System property</td><td headers="h602"><tt>jdk.xml.entityReplacementLimit</tt></td>
<td headers="h601">Since</td><td headers="h602">7u111, 8u101</td>

## maxElementDepth
<th id="h701">Attribute</th><th id="h702">Description</th>
<td headers="h701">Name</td><td headers="h702"><tt>http://www.oracle.com/xml/jaxp/properties/maxElementDepth</tt></td>
<td headers="h701">Definition</td><td headers="h702">Limit the maximum element depth.</td>
<td headers="h701">Value</td><td headers="h702">A positive integer. A value less than or equal to 0 indicates no limit. If the value is notan integer, a <tt>NumericFormatException</tt> is thrown.</td>
<td headers="h701">Default value</td><td headers="h702">0</td>
<td headers="h701">System property</td><td headers="h702"><tt>jdk.xml.maxElementDepth</tt></td>
<td headers="h701">Since</td><td headers="h702">7u65, 8u11</td>

## maxXMLNameLimit
<th id="h801">Attribute</th><th id="h802">Description</th>
<td headers="h801">Name</td><td headers="h802"><tt>http://www.oracle.com/xml/jaxp/properties/maxXMLNameLimit</tt></td>
<td headers="h801">Definition</td><td headers="h802">Limit the maximum size of XML names, including element name, attribute nameand namespace prefix and URI.</td>
<td headers="h801">Value</td><td headers="h802">A positive integer. A value less than or equal to 0 indicates no limit. If the value is notan integer, a <tt>NumericFormatException</tt> is thrown.</td>
<td headers="h801">Default value</td><td headers="h802">1000</td>
<td headers="h801">System property</td><td headers="h802"><tt>jdk.xml.maxXMLNameLimit</tt></td>
<td headers="h801">Since</td><td headers="h802">7u91, 8u65</td>

## Legacy System Properties

These properties, which were introduced since JDK 5.0 and 6, continue to be supported for backward compatibility.
<th id="h901">System Property</th><th id="h902">Since</th><th id="h903">New System Property</th>
<td headers="h901">entityExpansionLimit</td><td headers="h902">1.5</td><td headers="h903">jdk.xml.entityExpansionLimit</td>
<td headers="h901">elementAttributeLimit</td><td headers="h902">1.5</td><td headers="h903">jdk.xml.elementAttributeLimit</td>
<td headers="h901">maxOccurLimit</td><td headers="h902">1.6</td><td headers="h903">jdk.xml.maxOccur</td>

## {java.home}/lib/jaxp.properties

The system properties can be specified in the <tt>jaxp.properties</tt> file to define the behavior for all invocations of the JDK or JRE. The format is <tt>system-property-name=value</tt>. For example:

```

jdk.xml.maxGeneralEntitySizeLimit=1024

```
