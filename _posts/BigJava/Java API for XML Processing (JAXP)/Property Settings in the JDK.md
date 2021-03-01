
# Using the Properties

This section focuses on the new properties introduced in JAXP 1.5.

## When to Use the Properties


Restrictions to fetching external resources are only needed if applications process untrusted XML contents. Internal systems and applications that do not handle untrusted contents do not need to be concerned with the new restrictions or make any changes. Since 7u40 and JDK8 have no requirement on such restrictions by default, applications shall experience no behavioral change when upgrading to 7u40 and JDK8.


For applications that do handle untrusted XML input, Schema, or Stylesheet, if there is already existing security measures, such as enabling the Java Security Manager to grant only trusted external connections, or resolving entities using resolvers, there is no need for the new features added in JAXP 1.5.


However, JAXP 1.5 does provide straight-forward protections for system and applications that run without security manager. For such applications, restrictions may be considered by using the new feature as described in detail below.

## Setting Properties Through the API


When changing code is feasible, setting the new properties through JAXP factories or parser is the best way to enable restrictions. The properties can be set through the following interfaces:

```

DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
dbf.setAttribute(name, value);
 
SAXParserFactory spf = SAXParserFactory.newInstance();
SAXParser parser = spf.newSAXParser();
parser.setProperty(name, value);
 
XMLInputFactory xif = XMLInputFactory.newInstance();
xif.setProperty(name, value);
 
SchemaFactory schemaFactory = SchemaFactory.newInstance(schemaLanguage);
schemaFactory.setProperty(name, value);
 
TransformerFactory factory = TransformerFactory.newInstance();
factory.setAttribute(name, value);

```


The following is an example of limiting a DOM parser to local connection only for external DTDs:

```

DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
try {
    dbf.setAttribute({{XMLConstants.ACCESS_EXTERNAL_DTD}}, "file, jar:file");
} catch (IllegalArgumentException e) {
    //jaxp 1.5 feature not supported
}

```


When code change is possible, and for new development, it is recommended that the new properties be set as demonstrated above. By setting the properties this way, applications can be sure to maintain the desired behavior whether they are deployed to older or newer version of the JDK, or whether the properties are set through System Properties or <tt>jaxp.properties</tt>.

## Using System Properities


System properties may be useful if changing code is not feasible.


If it is desirable to set restrictions for an entire invocation of the JDK/JRE, set the system properties  on the command line; If it is needed for only a portion of the application, the system properties may be set before the section and cleared afterwards. For example, the following code shows how system properties may be used:

```

//allow resolution of external schemas

System.setProperty("javax.xml.accessExternalSchema", "file, http");

//this setting will affect all processing after it's set
some processing here

//after it's done, clear the property
System.clearProperty("javax.xml.accessExternalSchema");

```

## Using jaxp.properties


<tt>jaxp.properties</tt> is a plain configuration file. It is located at <tt>${java.home}/lib/jaxp.properties</tt> where <tt>java.home</tt> is the JRE install directory, e.g., <tt>[path to installation directory]/jdk7/jre</tt>.


An external access restriction can be set by adding this line into jaxp.properties file:

```

javax.xml.accessExternalStylesheet=file, http

```


When this is set, all invocations of the JDK/JRE will observe the restriction with regards to loading external Stylesheet.


This feature may be useful for systems that do not want to allow any external connection by XML processors, in which case, all three properties may be set to, for example, file only.
