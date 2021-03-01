
# Customizing Generated Classes and Java Program Elements

The following sections describe how to customize generated JAXB classes and Java program elements.

## <a name="bnazz" id="bnazz"></a>Schema-to-Java

Custom JAXB binding declarations enable you to customize your generated JAXB classes beyond the XML-specific constraints in an XML schema to include Java-specific refinements, such as class and package name mappings. 

JAXB provides two ways to customize an XML schema:

- As inline annotations in a source XML schema
- As declarations in an external binding customization file that is passed to the JAXB binding compiler

Code examples that show how to customize JAXB bindings are provided later in this document.

## <a name="bnbaa" id="bnbaa"></a>Java-to-Schema

The JAXB annotations defined in the <tt>javax.xml.bind.annotation</tt>
package can be used to customize Java program elements to XML schema mapping. The following table summarizes the JAXB annotations that can be used with a Java package.

Table:&#160;JAXB Annotations Associated with a Java Package

```

@XmlSchema ( 
    xmlns = {}, 
    namespace = "", 
    elementFormDefault = XmlNsForm.UNSET, 
    attributeFormDefault = XmlNsForm.UNSET
)

```

```

@XmlAccessorType (
    value = AccessType.PUBLIC_MEMBER 
)
```

```

@XmlAccessorOrder (
    value = AccessorOrder.UNDEFINED
)

```

```

@XmlSchemaType (
    namespace = 
    "http://www.w3.org/2001/XMLSchema", 
    type = DEFAULT.class
)

```

The following table summarizes JAXB annotations that can be used with a Java class.

Table:&#160;JAXB Annotations Associated with a Java Class

```

@XmlType (
    name = "##default", 
    propOrder = {""}, 
    namespace = "##default", 
    factoryClass = DEFAULT.class, 
    factoryMethod = ""
)

```

```

@XmlRootElement (
    name = "##default", 
    namespace = "##default" 
)

```

The following table summarizes JAXB annotations that can be used with a Java <tt>enum</tt> type.

Table:&#160;JAXB Annotations Associated with a Java <tt>enum</tt>
Type

```

@XmlEnum ( value = String.class )

```

```
 
@XmlType (
    name = "##default", 
    propOrder = {""}, 
    namespace = "##default", 
    factoryClass = DEFAULT.class, 
    factoryMethod = ""
)

```

```

@XmlRootElement (
    name = "##default", 
    namespace = "##default" 
)

```

The following table summarizes JAXB annotations that can be used with Java properties and fields.

Table:&#160;JAXB Annotations Associated with Java Properties
and Fields

```

@XmlElement (
    name = "##default", 
    nillable = false, 
    namespace = "##default", 
    type = DEFAULT.class, 
    defaultValue = "\u0000"
)

```

```

@XmlElementRef (
    name = "##default", 
    namespace = "##default", 
    type = DEFAULT.class
)

```

```

@XmlElementWrapper (
    name = "##default", 
    namespace = "##default", 
    nillable = false
)

```

```

@XmlAnyElement (
    lax = false, 
    value = W3CDomHandler.class
)

```

```

@XmlAttribute (
    name = ##default, 
    required = false, 
    namespace = "##default" 
)

```

The following table summarizes the JAXB annotation that can be used with object factories.

Table:&#160;JAXB Annotations Associated with Object Factories

```

@XmlElementDecl (
    scope = GLOBAL.class, 
    namespace = "##default", 
    substitutionHeadNamespace = "##default", 
    substitutionHeadName = ""
)

```

The following table summarizes JAXB annotations that can be used with adapters.

Table:&#160;JAXB Annotations Associated with Adapters

```

@XmlJavaTypeAdapter ( type = DEFAULT.class )

```
