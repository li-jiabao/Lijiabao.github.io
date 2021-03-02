
# Representing XML Content

This section describes how JAXB represents XML content as Java objects.

## <a name="bnazp" id="bnazp"></a>Java Representation of XML Schema

JAXB supports the grouping of generated classes in Java packages. A package consists of the following:

	- A Java class name that is derived from the XML element name 	or specified by a binding customization.
	<li>An <tt>ObjectFactory</tt> class, which is a factory that 
	is used to return instances of a bound Java class.</li>
