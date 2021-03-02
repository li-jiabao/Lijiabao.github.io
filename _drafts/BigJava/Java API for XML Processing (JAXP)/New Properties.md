
# Scope and Order


<tt>javax.xml.XMLConstants#FEATURE_SECURE_PROCESSING</tt> (FSP) is a required feature for XML processors including DOM, SAX, Schema Validation, XSLT and XPath.  When set to <tt>true</tt>, it is recommended that implementations enable access restrictions as defined by the new properties specified above.  For compatibility, JAXP 1.5 does not enable the new restrictions, although FSP is true by default for DOM, SAX and Schema Validation.


For JDK 8, the new <tt>accessExternal*</tt> properties are proposed to be set to the empty string when FSP is explicitly set.  This is only the case when FSP is set through the API, e.g. <tt>factory.setFeature(FSP, true)</tt>. Although FSP is true by default for DOM, SAX and Schema Validation it is not treated as if "explicitly" set, JDK 8 therefore does not set restrictions by default.


Properties specified in the <tt>jaxp.properties</tt> file affect all invocations of the JDK or JRE, and will override their default values, or those that may have been set by FEATURE_SECURE_PROCESSING.


System properties, when set, will affect one invocation only, and will override the default settings or those set in jaxp.properties, or those that may have been set by FEATURE_SECURE_PROCESSING.


JAXP properties specified through JAXP factories or <tt>SAXParser</tt> take preference over system properties, the <tt>jaxp.properties</tt> file, as well as <tt>javax.xml.XMLConstants#FEATURE_SECURE_PROCESSING</tt>.


The new JAXP properties have no effect on the relevant constructs they attempt to restrict in the following situations:

<li>
When there is a resolver and the source returned by the resolver is not null. This applies to entity resolvers that may be set on SAX and DOM parsers, XML resolvers on StAX parsers, LSResourceResolver on SchemaFactory, a Validator or ValidatorHandler, or URIResolver on a transformer.</li>
- When a schema is created explicitly by calling <tt>SchemaFactory</tt>'s <tt>newSchema</tt> method.
<li>When external resources are not required. For example, the following features/properties are supported by the reference implementation and may be used to instruct the processor to not load the external DTD or resolve external entities.<br />
<pre><code>
http://apache.org/xml/features/disallow-doctype-decl true
http://apache.org/xml/features/nonvalidating/load-external-dtd false
http://xml.org/sax/features/external-general-entities false
http://xml.org/sax/features/external-parameter-entities false
</code></pre>
</li>
