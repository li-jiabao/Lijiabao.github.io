
# Scope and Order


The <tt>javax.xml.XMLConstants#FEATURE_SECURE_PROCESSING</tt> (FSP) feature is required for XML processors including DOM, SAX, Schema Validation, XSLT, and XPath. The proposed default limits are enforced when FSP is set to <tt>true</tt>. Setting FSP to <tt>false</tt> does not change the limits.


When the Java Security Manager is present, FSP is set to true and can not be turned off. The proposed default limits are therefore enforced.


Properties specified in the <tt>jaxp.properties</tt> file affect all invocations of the JDK and JRE, and will override their default values, or those that may have been set by FSP.

 
System properties, when set, affect the invocation of the JDK and JRE, and override the default settings or that set in <tt>jaxp.properties</tt>, or those that may have been set by FSP.

 
JAXP properties specified through JAXP factories or <tt>SAXParser</tt> take preference over system properties, the <tt>jaxp.properties</tt> file, as well as <tt>FEATURE_SECURE_PROCESSING</tt>.
