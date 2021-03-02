
# External Resources


XML, Schema, and XSLT standards support the following constructs that require external resources. The default behavior of the JDK XML processors is to make a connection and fetch the external resources as specified.

- External DTD: references an external Document Type Definition (DTD), example: <tt>&lt;!DOCTYPE root_element SYSTEM "url"&gt;</tt>
 
<li>External Entity Reference: refer to external data, syntax: <tt>&lt;!ENTITY name SYSTEM "url"&gt;</tt>
<br /> 
General entity reference such as the following:
<pre><code> 
&lt;?xml version="1.0" standalone="no" ?&gt;
&lt;!DOCTYPE doc [&lt;!ENTITY otherFile SYSTEM "otherFile.xml"&gt;]&gt;
&lt;doc&gt;
    &lt;foo&gt;
    &lt;bar&gt;&amp;otherFile;&lt;/bar&gt;
    &lt;/foo&gt;
&lt;/doc&gt;
</code></pre>
</li>
 
<li>External Parameter Entities, syntax <tt>&lt;!ENTITY % name SYSTEM uri&gt;</tt>. For example:
<pre><code>
&lt;?xml version="1.0" standalone="no"?&gt;
    &lt;!DOCTYPE doc [
      &lt;!ENTITY % foo SYSTEM "http://www.example.com/student.dtd"&lt;
      %foo;
    ]&gt;
</code></pre>
</li>
 
- XInclude: include an external infoset in an XML document
 
- Reference to XML Schema components using <tt>schemaLocation</tt> attribute, and <tt>import</tt> and <tt>include</tt> elements. Example: <tt>schemaLocation="http://www.example.com/schema/bar.xsd"</tt>
 
- Combining style sheets using <tt>import</tt> or <tt>include</tt> elements: syntax: <tt>&lt;xsl:include href="include.xsl"/&gt;</tt>
 
- xml-stylesheet processing instruction: used to include a stylesheet in an xml document, syntax: <tt>&lt;?xml-stylesheet href="foo.xsl" type="text/xsl"?&gt;</tt>
 
- XSLT <tt>document()</tt> function: used to access nodes in an external XML document. For example, <tt>&lt;xsl:variable name="dummy" select="document('DocumentFunc2.xml')"/&gt;</tt>.
