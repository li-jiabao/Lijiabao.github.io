
# Error Handling


Since the properties are new to the current release, it is recommended that applications catch exceptions proper to the interface, for example, SAXException in the following example.  Catching the applications may work properly on older releases, for example the sample code contains the following method that detects if the sample is run with a version of the JDK or a JAXP implementation that supports the new property:

```

public boolean isNewPropertySupported() {
       try {
           SAXParserFactory spf = SAXParserFactory.newInstance();
           SAXParser parser = spf.newSAXParser();
           parser.setProperty("http://javax.xml.XMLConstants/property/accessExternalDTD", "file");
       } catch (ParserConfigurationException ex) {
           fail(ex.getMessage());
       } catch (SAXException ex) {
           String err = ex.getMessage();
           if (err.indexOf("Property 'http://javax.xml.XMLConstants/property/accessExternalDTD' is not recognized.") &gt; -1)
           {
             //expected, jaxp 1.5 not supported
             return false;
           }
       }
       return true;
  }

```


If access to external resources is denied due to the restrictions set by the new properties, an exception will be thrown with an error in the following format:

```

[type of construct]: Failed to read [type of construct] "[name of the external resource]", because "[type of restriction]" access is not allowed due to restriction set by the [property name] property.

```


For example, if fetching an external DTD is denied by restriction to http protocol such as the following: <tt>parser.setProperty("http://javax.xml.XMLConstants/property/accessExternalDTD", "file");</tt> and if the parser parses an XML file that contains external reference to <tt>"http://java.sun.com/dtd/properties.dtd"</tt>, the error message would look like the following:

```

External DTD: Failed to read external DTD ''http://java.sun.com/dtd/properties.dtd'', because ''http'' access is not allowed due to restriction set by the accessExternalDTD property.

```
