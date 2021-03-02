
# Error Handling


It is recommended that applications catch the <tt>org.xml.sax.SAXNotRecognizedException</tt> exception when setting one of the new properties so that the applications will work properly on older releases that did not support them. For example, the downloadable
[sample code](sample.html) contains the following method, <tt>isNewPropertySupported</tt>, that detects if the sample is run with a version of the JDK that supports the <tt>JDK_GENERAL_ENTITY_SIZE_LIMIT</tt> property:

```

public boolean isNewPropertySupported() {
    try {
        SAXParser parser = getSAXParser(false, false, false);
        parser.setProperty(JDK_GENERAL_ENTITY_SIZE_LIMIT, "10000");
    } catch (ParserConfigurationException ex) {
        fail(ex.getMessage());
    } catch (SAXException ex) {
        String err = ex.getMessage();
        if (err.indexOf("Property '" + JDK_GENERAL_ENTITY_SIZE_LIMIT +
                                       "' is not recognized.") &gt; -1) {
            //expected before this patch
            debugPrint("New limit properties not supported. Samples not run.");
            return false;
        }
    }
    return true;
}

```


When the input files contain constructs that cause an over-the-limit exception, applications may check the error code to determine the nature of the failure. The following error codes are defined for the  limits:

- <tt>EntityExpansionLimit</tt>: JAXP00010001
- <tt>ElementAttributeLimit</tt>: JAXP00010002
- <tt>MaxEntitySizeLimit</tt>: JAXP00010003
- <tt>TotalEntitySizeLimit</tt>: JAXP00010004
- <tt>MaxXMLNameLimit</tt>: JAXP00010005
- <tt>maxElementDepth</tt>: JAXP00010006
- <tt>EntityReplacementLimit</tt>: JAXP00010007


The error code has the following format:

```

"JAXP" + components (two digits) + error category (two digits) + sequence number

```


The code JAXP00010001, therefore, represents the JAXP base parser security limit <tt>EntityExpansionLimit</tt>.
