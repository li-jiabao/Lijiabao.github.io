
# Basic Examples

This section describes the Basic JAXB examples (Modify Marshal,
Unmarshal Validate) that demonstrate how to:

- Unmarshal an XML document into a Java content tree and access the data contained within it
- Modify a Java content tree
- Use the <tt>ObjectFactory</tt> class to create a Java content tree and then marshal it to XML data
- Perform validation during unmarshalling
- Validate a Java content tree at runtime

##  <a name="bnbaz" id="bnbaz"></a>Modify Marshal Example

The Modify Marshal example demonstrates how to modify a Java content tree.

<li>The *jaxb-ri-install*<tt>/samples/modify-marshal/src/Main.java</tt> class declares importing of three standard Java classes, five JAXB binding framework classes and the <tt>primer.po</tt> package:
<pre><code>
import java.io.FileInputStream;
import java.io.IOException;
import java.math.BigDecimal;

import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.JAXBException;
import javax.xml.bind.Marshaller;
import javax.xml.bind.Unmarshaller;

import primer.po.*;
</code></pre>
</li>
<li>A <tt>JAXBContext</tt> instance is created for handling classes generated in the <tt>primer.po</tt>.
<pre><code>	
JAXBContext jc = JAXBContext.newInstance( "primer.po" );
</code></pre>
</li>
<li>An <tt>Unmarshaller</tt> instance is created, and the <tt>po.xml</tt> file is unmarshalled.
<pre><code>
Unmarshaller u = jc.createUnmarshaller();
PurchaseOrder po = (PurchaseOrder)
    u.unmarshal(new FileInputStream("po.xml"));
</code></pre>
</li>
<li><tt>set</tt> methods are used to modify information in the <tt>address</tt> branch of the content tree.
<pre><code>
USAddress address = po.getBillTo();
address.setName("John Bob");
address.setStreet("242 Main Street");
address.setCity("Beverly Hills");
address.setState("CA");
address.setZip(new BigDecimal
address.setName("John Bob");
address.setStreet("242 Main Street");
address.setCity("Beverly Hills");
address.setState("CA");
address.setZip(new BigDecimal("90210"));
</code></pre>
</li>
<li>A <tt>Marshaller</tt> instance is created, and the updated XML content is marshalled to <tt>system.out</tt>. The <tt>setProperty</tt> API is used to specify output encoding; in this case it is formatted (human readable) XML.
<pre><code>
Marshaller m = jc.createMarshaller();
m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, Boolean.TRUE);
m.marshal(po, System.out);
</code></pre>
</li>

### Building and Running the Modify Marshal Example Using Ant

To compile and run the Modify Marshal example using Ant, in a terminal window, go to the *jaxb-ri-install*<tt>/samples/modify-marshal/</tt> directory and type the following:

```

ant

```

## <a name="bnbbc" id="bnbbc"></a>Unmarshal Validate Example

The Unmarshal Validate example demonstrates how to enable validation during unmarshalling. Note that JAXB provides functions for validation during unmarshalling but not during marshalling. Validation is explained in more detail in 
[More About Validation](arch.html#bnazn).

<li>The *jaxb-ri-install*<tt>/samples/unmarshal-validate/src/Main.java</tt> class declares imports for one standard Java class, eleven JAXB binding framework classes, and the <tt>primer.po</tt> package:
<pre><code>
import java.io.File;

import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBException;
import javax.xml.bind.Marshaller;
import javax.xml.bind.UnmarshalException;
import javax.xml.bind.Unmarshaller;
import javax.xml.bind.ValidationEvent;
import javax.xml.bind.ValidationEventHandler;
import javax.xml.bind.ValidationEventLocator;

import static javax.xml.XMLConstants.W3C_XML_SCHEMA_NS_URI;
import javax.xml.validation.SchemaFactory;
import javax.xml.validation.Schema;

import primer.po.*;
</code></pre>
</li>
<li>A <tt>JAXBContext</tt> instance is created for handling classes generated in the <tt>primer.po</tt> package.
<pre><code>
JAXBContext jc = JAXBContext.newInstance("primer.po");
</code></pre>
</li>
<li>An <tt>Unmarshaller</tt> instance is created.
<pre><code>
Unmarshaller u = jc.createUnmarshaller();
</code></pre>
</li>
<li>The default JAXB <tt>Unmarshaller</tt> <tt>ValidationEventHandler</tt> is enabled to send validation warnings and errors to <tt>system.out</tt>. The default configuration causes the unmarshal operation to fail upon encountering the first validation error.
<pre><code>
u.setValidating( true );
</code></pre>
</li>
<li>An attempt is made to unmarshal the <tt>po.xml</tt> file into a Java content tree. For the purposes of this example, the <tt>po.xml</tt> file contains a deliberate error.
<pre><code>
PurchaseOrder po = (PurchaseOrder)u.unmarshal(
    new FileInputStream("po.xml"));
</code></pre>
</li>
<li>The default validation event handler processes a validation error, generates output to <tt>system.out</tt>, and then an exception is thrown. 

<pre><code>
} catch( UnmarshalException ue ) {
    System.out.println("Caught UnmarshalException");
} catch( JAXBException je ) { 
    je.printStackTrace();
} catch( IOException ioe ) {
    ioe.printStackTrace();
}
</code></pre>
</li>

### Building and Running the Unmarshal Validate Example Using Ant

To compile and run the Unmarshal Validate example using Ant, in a
terminal window, go to the *jaxb-ri-install*<tt>/samples/unmarshal-validate/</tt> directory and type the following:

```

ant 

```
