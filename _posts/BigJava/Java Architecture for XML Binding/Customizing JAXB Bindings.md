
# Java-to-Schema Examples

The Java-to-Schema examples show how to use annotations to map Java classes to XML schema.

## <a name="bnbcw" id="bnbcw"></a>j2s-create-marshal Example

The j2s-create-marshal example illustrates Java-to-schema data binding. It demonstrates marshalling and unmarshalling of JAXB annotated classes and also shows how to enable JAXP 1.3 validation at unmarshal time using a schema file that was generated from the JAXB mapped classes.

The schema file, <tt>bc.xsd</tt>, was generated with the following commands:

```

<tt>schemagen src/cardfile/*.java</tt>
<tt>cp schema1.xsd bc.xsd</tt>

```

Note that <tt>schema1.xsd</tt> was copied to <tt>bc.xsd</tt>;
<tt>schemagen</tt> does not allow you to specify a schema name of your choice.

### <a name="bnbcy" id="bnbcy"></a>Building and Running the j2s-create-marshal Example Using Ant

To compile and run the j2s-create-marshal example using Ant, in a
terminal window, go to the *jaxb-ri-install*<tt>/samples/j2s-create-marshal/</tt> directory and type the following:

```

ant 

```

## <a name="bnbcz" id="bnbcz"></a>j2s-xmlAccessorOrder Example

The j2s-xmlAccessorOrder example shows how to use the
<tt>@XmlAccessorOrder</tt> and <tt>@XmlType.propOrder</tt> annotations to dictate the order in which XML content is marshalled and unmarshalled by a Java type.

With Java-to-schema mapping, a JavaBean&#8217;s properties and
fields are mapped to an XML Schema type. The class elements are mapped to either an XML Schema complex type or an XML Schema simple type. The default element order for a generated schema type is currently unspecified because Java reflection does not impose a return order. The lack of reliable element ordering negatively impacts application portability. You can use two annotations, <tt>@XmlAccessorOrder</tt> and <tt>@XmlType.propOrder</tt>, to define schema element ordering for applications that must be portable across JAXB Providers.

### <a name="bnbda" id="bnbda"></a>Using the <tt>@XmlAccessorOrder</tt> Annotation to Define Schema Element Ordering

The <tt>@XmlAccessorOrder</tt> annotation imposes one of two element ordering algorithms, <tt>AccessorOrder.UNDEFINED</tt> or <tt>AccessorOrder.ALPHABETICAL</tt>. <tt>AccessorOrder.UNDEFINED</tt> is the default setting. The order is dependent on the system&#8217;s reflection
implementation. <tt>AccessorOrder.ALPHABETICAL</tt> algorithm orders the elements in lexicographic order as determined by <tt>java.lang.String.CompareTo(String anotherString)</tt>.

You can define the <tt>@XmlAccessorOrder</tt> annotation for annotation type <tt>ElementType.PACKAGE</tt> on a class object. When the <tt>@XmlAccessorOrder</tt> annotation is defined on a package, the scope of the formatting rule is active for every class in the package. When defined on a class, the rule is active on the contents of that class.

There can be multiple <tt>@XmlAccessorOrder</tt> annotations within a package. The innermost (class) annotation takes precedence over the outer annotation. For example, if <tt>@XmlAccessorOrder(AccessorOrder.ALPHABETICAL)</tt> is defined on a package and
<tt>@XmlAccessorOrder(AccessorOrder.UNDEFINED)</tt> is defined on a class in that package, the contents of the generated schema type for the class would be in an unspecified order and the contents of the generated schema type for every other class in the package would be in alphabetical order.

### <a name="bnbdb" id="bnbdb"></a>Using the <tt>@XmlType</tt> Annotation to Define Schema Element Ordering

The <tt>@XmlType</tt> annotation can be defined for a class. The annotation element <tt>propOrder()</tt> in the <tt>@XmlType</tt> annotation enables you to specify the content order in the generated schema type. When you use the <tt>@XmlType.propOrder</tt> annotation on a class to specify content order, all public properties and public fields in the class must be specified in the parameter list. Any public property or field that you want to keep out of the parameter list must be annotated with <tt>@XmlAttribute</tt> or <tt>@XmlTransient</tt> annotation.

The default content order for <tt>@XmlType.propOrder</tt>
is <tt>{}</tt> or <tt>{""}</tt>, not active. In such cases, the active <tt>@XmlAccessorOrder</tt> annotation takes precedence. When class content order is specified by the <tt>@XmlType.propOrder</tt> annotation, it takes precedence over any active <tt>@XmlAccessorOrder</tt>
annotation on the class or package. If the <tt>@XmlAccessorOrder</tt>
and <tt>@XmlType.propOrder(A, B, ...)</tt> annotations are specified on a class, the <tt>propOrder</tt> always takes precedence regardless of the order of the annotation statements. For example, in the following code segment, the <tt>@XmlAccessorOrder</tt> annotation precedes the <tt>@XmlType.propOrder</tt> annotation.

```

@XmlAccessorOrder(AccessorOrder.ALPHABETICAL)
@XmlType(propOrder={"name", "city"})

public class USAddress {
    // ...
    public String getCity() {return city;}
    public void setCity(String city) {this.city = city;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    // ...
}

```

In the following code segment, the <tt>@XmlType.propOrder</tt>
annotation precedes the <tt>@XmlAccessorOrder</tt> annotation.

```

@XmlType(propOrder={"name", "city"})
@XmlAccessorOrder(AccessorOrder.ALPHABETICAL)
public class USAddress {
    // ...
    public String getCity() {return city;}
    public void setCity(String city) {this.city = city;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    // ...
}

```

In both scenarios, <tt>propOrder</tt> takes precedence and the following identical schema content is generated:

```

&lt;xs:complexType name="usAddress"&gt;
    &lt;xs:sequence&gt;
        &lt;xs:element 
            name="name" 
            type="xs:string" 
            minOccurs="0"/&gt;
        &lt;xs:element 
            name="city" 
            type="xs:string" 
            minOccurs="0"/&gt;
    &lt;/xs:sequence&gt;
&lt;/xs:complexType&gt;

```

### <a name="bnbdc" id="bnbdc"></a>Schema Content Ordering in the Example

The purchase order code example demonstrates the effects of schema content ordering using the <tt>@XmlAccessorOrder</tt> annotation at the package and class level, and the <tt>@XmlType.propOrder</tt> annotation on a class.

Class <tt>package-info.java</tt> defines <tt>@XmlAccessorOrder</tt> to be <tt>ALPHABETICAL</tt> for the package. The public fields <tt>shipTo</tt>
and <tt>billTo</tt> in class <tt>PurchaseOrderType</tt> are affected in the generated schema content order by this rule. Class <tt>USAddress</tt> defines the <tt>@XmlType.propOrder</tt> annotation on the class which demonstrates user-defined property order superseding <tt>ALPHABETICAL</tt> order in the generated schema.

The generated schema file can be found in the
*jaxb-ri-install*<tt>/samples/j2s-xmlAccessorOrder/build/schemas/</tt>
directory.

### <a name="bnbde" id="bnbde"></a>Building and Running the j2s-xmlAccessorOrder Example Using Ant

To compile and run the j2s-xmlAccessorOrder example using Ant, in a
terminal window, go to the
*jaxb-ri-install*<tt>/samples/j2s-xmlAccessorOrder/</tt> directory and type the following:

```

ant 

```

## <a name="bnbdf" id="bnbdf"></a>j2s-xmlAdapter Example

 The j2s-xmlAdapter example demonstrates how to use the <tt>XmlAdapter</tt> interface and the <tt>@XmlJavaTypeAdapter</tt> annotation to provide a custom mapping of XML content into and out of a <tt>HashMap</tt> (field) that uses an <tt>int</tt> as the key and a <tt>String</tt> as the value.

Interface <tt>XmlAdapter</tt> and annotation <tt>@XmlJavaTypeAdapter</tt> are used for special processing of data types during unmarshalling and marshalling. There are a variety of XML data types for which the representation does not map easily into Java (for example, <tt>xs:DateTime</tt> and <tt>xs:Duration</tt>), and Java types that do not map properly into XML representations. For example, implementations of <tt>java.util.Collection</tt> (such as <tt>List</tt>) and <tt>java.util.Map</tt> (such as <tt>HashMap</tt>) or for non-JavaBean
classes.

The <tt>XmlAdapter</tt> interface and the <tt>@XmlJavaTypeAdapter</tt> annotation are provided for cases such as these. This combination provides a portable mechanism for reading and writing XML content into and out of Java applications.

The <tt>XmlAdapter</tt> interface defines the methods for data reading and writing.

```

/*
 *  ValueType - Java class that provides an 
 *  XML representation of the data. 
 *  It is the object that is used for marshalling and 
 *  unmarshalling.
 *
 *  BoundType - Java class that is used to 
 *  process XML content.
 */
 
public abstract class XmlAdapter&lt;ValueType,BoundType&gt; {

    // Do-nothing constructor for the derived classes.
    protected XmlAdapter() {}
    
    // Convert a value type to a bound type.
    public abstract BoundType unmarshal(ValueType v);
    
    // Convert a bound type to a value type.
    public abstract ValueType marshal(BoundType v);
}

```

You can use the <tt>@XmlJavaTypeAdapter</tt> annotation to associate a particular <tt>XmlAdapter</tt> implementation with a <tt>Target</tt> type, <tt>PACKAGE</tt>, <tt>FIELD</tt>, <tt>METHOD</tt>, <tt>TYPE</tt>, or <tt>PARAMETER</tt>.

The j2s-xmlAdapter example shows how to use an <tt>XmlAdapter</tt> for mapping XML content into and out of a (custom) <tt>HashMap</tt>. The <tt>HashMap</tt> object, <tt>basket</tt>, in class <tt>KitchenWorldBasket</tt>, uses a key of type <tt>int</tt> and a value of type <tt>String</tt>. These data types should be reflected in the XML content that is read and written, so the XML content should look as in the following example:

```

&lt;basket&gt;
    &lt;entry key="9027"&gt;glasstop stove in black&lt;/entry&gt;
    &lt;entry key="288"&gt;wooden spoon&lt;/entry&gt;
&lt;/basket&gt;

```

The default schema generated for Java type <tt>HashMap</tt>
does not reflect the desired format.

```

&lt;xs:element name="basket"&gt;
    &lt;xs:complexType&gt;
        &lt;xs:sequence&gt;
            &lt;xs:element 
                name="entry" 
                minOccurs="0" 
                maxOccurs="unbounded"&gt;
                &lt;xs:complexType&gt;
                    &lt;xs:sequence&gt;
                        &lt;xs:element 
                            name="key" 
                            minOccurs="0"
                            type="xs:anyType"/&gt;
                        &lt;xs:element 
                            name="value" 
                            minOccurs="0" 
                            type="xs:anyType"/&gt;
                    &lt;/xs:sequence&gt;
                &lt;/xs:complexType&gt;
            &lt;/xs:element&gt;
        &lt;/xs:sequence&gt;
    &lt;/xs:complexType&gt;
&lt;/xs:element&gt;

```

In the default <tt>HashMap</tt> schema, key and value are both elements and are of data type <tt>anyType</tt>. The XML content looks like the following:

```

&lt;basket&gt;
    &lt;entry&gt;
        &lt;key&gt;9027&lt;/key&gt;
        &lt;value&gt;glasstop stove in black&lt;/value&gt;
    &lt;/entry&gt;
    &lt;entry&gt;
        &lt;key&gt;288&lt;/key&gt;
        &lt;value&gt;wooden spoon&lt;/value&gt;
    &lt;/entry&gt;
&lt;/basket&gt;

```

To resolve this issue, the example uses two Java classes,
<tt>PurchaseList</tt> and <tt>PartEntry</tt>,
that reflect the needed schema format for unmarshalling and marshalling
the content. The XML schema generated for these classes is as
follows:

```

&lt;xs:complexType name="PurchaseListType"&gt;
    &lt;xs:sequence&gt;
        &lt;xs:element 
            name="entry" 
            type="partEntry"
            nillable="true" 
            maxOccurs="unbounded"
            minOccurs="0"/&gt;
    &lt;/xs:sequence&gt;
&lt;/xs:complexType&gt;

&lt;xs:complexType name="partEntry"&gt;
    &lt;xs:simpleContent&gt;
        &lt;xs:extension base="xs:string"&gt;
            &lt;xs:attribute
                name="key" 
                type="xs:int"
                use="required"/&gt;
        &lt;/xs:extension&gt;
    &lt;/xs:simpleContent&gt;
&lt;/xs:complexType&gt;

```

Class <tt>AdapterPurchaseListToHashMap</tt> implements the <tt>XmlAdapter</tt> interface. In class <tt>KitchenWorldBasket</tt>, the
<tt>@XmlJavaTypeAdapter</tt> annotation is used to pair  <tt>AdapterPurchaseListToHashMap</tt> with the field <tt>HashMap</tt> <tt>basket</tt>. This pairing causes the marshal or unmarshal method of
<tt>AdapterPurchaseListToHashMap</tt> to be called for any corresponding marshal or unmarshal action on <tt>KitchenWorldBasket</tt>.

### <a name="bnbdh" id="bnbdh"></a>Building and Running the j2s-xmlAdapter Example Using Ant

To compile and run the j2s-xmlAdapter example using Ant, in a
terminal window, go to the *jaxb-ri-install*<tt>/samples/j2s-xmlAdapter/</tt> directory and type the following:

```

ant

```

## <a name="bnbdi" id="bnbdi"></a>j2s-xmlAttribute Example

The j2s-xmlAttribute example shows how to use the <tt>@XmlAttribute</tt> annotation to define a property or field to be treated as an XML
attribute.

The <tt>@XmlAttribute</tt> annotation maps a field
or JavaBean property to an XML attribute. The following rules are
imposed:

	- A static final field is mapped to an XML fixed attribute.
	<li>When the field or property is a collection type, the items of
	the collection type must map to a schema simple type.</li>
	<li>When the field or property is other than a collection type,
	the type must map to a schema simple type.</li>

When following the JavaBean programming paradigm, a property is
defined by a <tt>get</tt> and <tt>set</tt>
prefix on a field name.

```

int zip;
public int getZip(){return zip;}
public void setZip(int z){zip=z;}

```

Within a bean class, you have the choice of setting the <tt>@XmlAttribute</tt> annotation on one of three components: the field, the setter method, or the getter method. If you set the <tt>@XmlAttribute</tt> annotation on the field, the setter method must be renamed or
there will be a naming conflict at compile time. If you set the
<tt>@XmlAttribute</tt> annotation on one of the methods, it must be set on either the setter or getter method, but not on both.

The XmlAttribute example shows how to use the <tt>@XmlAttribute</tt> annotation on a static final field, on a field rather than on one of
the corresponding bean methods, on a bean property (method), and on a
field that is other than a collection type. In class <tt>USAddress</tt>,
fields, country, and zip are tagged as attributes. The <tt>setZip</tt>
method was disabled to avoid the compile error. Property state was
tagged as an attribute on the setter method. You could have used the
getter method instead. In class <tt>PurchaseOrderType</tt>,
field <tt>cCardVendor</tt> is a non-collection type. It meets the requirement of being a simple type; it is an <tt>enum</tt> type.

### <a name="bnbdk" id="bnbdk"></a>Building and Running the j2s-xmlAttribute Example Using Ant

To compile and run the j2s-xmlAttribute example using Ant, in a terminal window, go to the *jaxb-ri-install*<tt>/samples/j2s-xmlAttribute/</tt> directory and type the following:

```

ant

```

## <a name="bnbdl" id="bnbdl"></a>j2s-xmlRootElement Example

The j2s-xmlRootElement example demonstrates the use of the
<tt>@XmlRootElement</tt> annotation to define an XML
element name for the XML schema type of the corresponding class.

The <tt>@XmlRootElement</tt> annotation maps a
class or an <tt>enum</tt> type to an XML element. At
least one element definition is needed for each top-level Java type
used for unmarshalling and marshalling. If there is no element
definition, there is no starting location for XML content processing.

The <tt>@XmlRootElement</tt> annotation uses the
class name as the default element name. You can change the default
name by using the annotation attribute <tt>name</tt>. If you do, the specified name is used as the element name and the type name. 
It is common schema practice for the element and type names to be different. You can use the <tt>@XmlType</tt> annotation to set the element type name.

The namespace attribute of the <tt>@XmlRootElement</tt>
annotation is used to define a namespace for the element.

### <a name="bnbdn" id="bnbdn"></a>Building and Running the j2s-xmlRootElement Example Using Ant

To compile and run the j2s-xmlRootElement example using Ant, in a terminal window, go to the *jaxb-ri-install*<tt>/samples/j2s-xmlRootElement/</tt> directory and type the following:

```

ant

```

## <a name="bnbdo" id="bnbdo"></a>j2s-xmlSchemaType Example

The j2s-xmlSchemaType example demonstrates the use of the
annotation <tt>@XmlSchemaType</tt> to customize the mapping of a property or field to an XML built-in type.

The <tt>@XmlSchemaType</tt> annotation can be used to map a Java type to one of the XML built-in types. This annotation is most useful in mapping a Java type to one of the nine date/time primitive data types.

When the <tt>@XmlSchemaType</tt> annotation is defined at the package level, the identification requires both the XML built-in type name and the corresponding Java type class. An <tt>@XmlSchemaType</tt> definition on a field or property takes precedence over a package definition.

The XmlSchemaType Class example shows how to use the 
<tt>@XmlSchemaType</tt> annotation at the package level, on a field, and on a property. File <tt>TrackingOrder</tt> has two fields, <tt>orderDate</tt> and <tt>deliveryDate</tt>, which are defined to be of type <tt>XMLGregorianCalendar</tt>. The generated schema will define these elements to be of XML built-in type <tt>gMonthDay</tt>. This relationship was defined on the package in the file <tt>package-info.java</tt>. Field <tt>shipDate</tt> in file <tt>TrackingOrder</tt> is also defined to be of type <tt>XMLGregorianCalendar</tt>, but the <tt>@XmlSchemaType</tt> annotation statements override the package definition and specify the field to be of type <tt>date</tt>. Property method <tt>getTrackingDuration</tt>
defines the schema element to be defined as primitive type <tt>duration</tt>
and not Java type <tt>String</tt>.

### <a name="bnbdq" id="bnbdq"></a>Building and Running the j2s-xmlSchemaType Example Using Ant

To compile and run the j2s-xmlSchemaType example using Ant, in a
terminal window, go to the
*jaxb-ri-install*<tt>/samples/j2s-xmlSchemaType/</tt> directory and type the following:

```

ant 

```

## <a name="bnbdr" id="bnbdr"></a>j2s-xmlType Example

The j2s-xmlType example demonstrates the use of the <tt>@XmlType</tt>
annotation. The <tt>@XmlType</tt> annotation maps a class or an <tt>enum</tt> type to an XML Schema type.

A class must have either a public zero-argument constructor or a
static zero-argument factory method in order to be mapped by this
annotation. One of these methods is used during unmarshalling to
create an instance of the class. The factory method can be located within
in a factory class or the existing class.

There is an order of precedence as to which method is used for
unmarshalling:

	<li>If a factory class is identified in the annotation, a
	corresponding factory method in that class must also be identified, 		and that method will be used.</li>
	<li>If a factory method is identified in the annotation but no
	factory class is identified, the factory method must be located in 		the current class. The factory method is used even if there is a 
	public zero argument constructor method present.</li>
	<li>If no factory method is identified in the annotation, the
	class must contain a public zero argument constructor method.</li>

In this example, a factory class provides zero arg factory methods
for several classes. The <tt>@XmlType</tt> annotation on class <tt>OrderContext</tt> references the factory class. The unmarshaller uses the identified factory method in this class.

```

public class OrderFormsFactory {

    public OrderContext newOrderInstance() {
        return new OrderContext()
    }

    public PurchaseOrderType {
        newPurchaseOrderType() {
            return new newPurchaseOrderType();
        }
    }
    
    @XmlType(name="oContext",
        factoryClass="OrderFormsFactory",
        factoryMethod="newOrderInstance")
    public class OrderContext {
        public OrderContext() {
            // ...
        }
    }
}

```

In this example, a factory method is defined in a class, which also
contains a standard class construct. Because the <tt>factoryMethod</tt>
value is defined and no <tt>factoryClass</tt> is defined, the factory method <tt>newOrderInstance</tt> is used during unmarshalling.

```

@XmlType(name="oContext", 
    factoryMethod="newOrderInstance")
public class OrderContext {

    public OrderContext() {
        // ...
    }

    public OrderContext newOrderInstance() {
        return new OrderContext();
    }
}

```

### <a name="bnbdt" id="bnbdt"></a>Building and Running the j2s-xmlType Example Using Ant

To compile and run the j2s-xmlType example using Ant, in a terminal
window, go to the *jaxb-ri-install*<tt>/samples/j2s-xmlType/</tt>
directory and type the following:

```

ant

```
