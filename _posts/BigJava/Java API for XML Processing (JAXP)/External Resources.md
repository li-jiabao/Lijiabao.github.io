
# New Properties


JAXP 1.5 defines three new properties that can be used to regulate whether or not XML processors  resolve external resources as listed above. The properties are:

- <tt>javax.xml.XMLConstants.ACCESS_EXTERNAL_DTD</tt>
- <tt>javax.xml.XMLConstants.ACCESS_EXTERNAL_SCHEMA</tt>
- <tt>javax.xml.XMLConstants.ACCESS_EXTERNAL_STYLESHEET</tt>


These API properties have corresponding system properties and jaxp.properties.

## ACCESS_EXTERNAL_DTD


**Name**: <tt>http://javax.xml.XMLConstants/property/accessExternalDTD</tt><br />
**Definition**: Restrict access to external DTDs, external Entity References to the protocols specified.<br />
**Value**: see [Values of the Properties](#values)<br />
**Default value**: <tt>all</tt>,  connection permitted to all protocols.<br />
**System property**: <tt>javax.xml.accessExternalDTD</tt>


## ACCESS_EXTERNAL_SCHEMA


**Name**: <tt>http://javax.xml.XMLConstants/property/accessExternalSchema</tt><br />
**Definition**: restrict access to the protocols specified for external reference set by the <tt>schemaLocation</tt> attribute, Import and Include element.<br />
**Value**: see [Values of the Properties](#values)<br />
**Default value**: <tt>all</tt>,  connection permitted to all protocols.<br />
**System property**: <tt>javax.xml.accessExternalSchema</tt>


## ACCESS_EXTERNAL_STYLESHEET


**Name**: <tt>http://javax.xml.XMLConstants/property/accessExternalStylesheet</tt><br />
**Definition**: restrict access to the protocols specified for external reference set by the stylesheet processing instruction, document function, Import and Include element.<br />
**Value**: see [Values of the Properties](#values)<br />
**Default value**: <tt>all</tt>,  connection permitted to all protocols.<br />
**System property**: <tt>javax.xml.accessExternalStylesheet</tt>


## ${java.home}/lib/jaxp.properties


These properties can be specified in <tt>jaxp.properties</tt> to define the behavior for all applications using the Java Runtime. The format is <tt>property-name=[value][,value]*</tt>. For example:

```

javax.xml.accessExternalDTD=file,http

```


The property names are the same as those of the system properties: <tt>javax.xml.accessExternalDTD</tt>, <tt>javax.xml.accessExternalSchema</tt>, and <tt>javax.xml.accessExternalStylesheet</tt>.

## <a name="values" id="values">Values of the Properties</a>


All of the properties have values in the same format.


**Value**: a list of protocols separated by comma. A protocol is the scheme portion of an URI, or in the case of the JAR protocol, "jar" plus the scheme portion separated by colon. A scheme is defined as:


<tt>scheme = alpha *( alpha | digit | "+" | "-" | "." )</tt><br />
where alpha = a-z and A-Z.


And the JAR protocol:<br />
<tt>jar[:scheme]</tt>


Protocols are case-insensitive. Any whitespaces as defined by <tt>Character.isSpaceChar</tt> in the value will be ignored. Examples of protocols are <tt>file</tt>, <tt>http</tt>, <tt>jar:file</tt>.


**Default value**: the default value is implementation specific. In JAXP 1.5 RI, Java SE 7u40, and Java SE 8, the default value is <tt>all</tt>, granting permissions to all protocols.


**Granting all access**: the keyword <tt>all</tt> grants permission to all protocols. For example, setting <tt>javax.xml.accessExternalDTD=all</tt> in <tt>jaxp.properties</tt> would allow a system to work as before with no restrictions on accessing external DTDs and Entity References.


**Denying any access**: an empty string, that is, "", means no permission is granted to any protocol. For example, setting <tt>javax.xml.accessExternalDTD=""</tt> in <tt>jaxp.properties</tt> would instruct the JAXP processors to deny any external connections.
