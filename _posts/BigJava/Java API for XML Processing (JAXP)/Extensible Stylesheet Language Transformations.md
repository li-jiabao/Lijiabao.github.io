
# Introducing XSL, XSLT, and XPath

The Extensible Stylesheet Language (XSL) has three major subcomponents:

The Formatting Objects standard. By far the largest subcomponent, this standard gives mechanisms for describing font sizes, page layouts, and other aspects of object rendering. This subcomponent is not covered by JAXP, nor is it included in this tutorial.

This is the transformation language, which lets you define a transformation from XML into some other format. For example, you might use XSLT to produce HTML or a different XML structure. You could even use it to produce plain text or to put the information in some other document format. (And as you will see in 
[Generating XML from an Arbitrary Data Structure](generatingXML.html), a clever application can press it into service to manipulate non-XML data as well).

At bottom, XSLT is a language that lets you specify what sorts of things to do when a particular element is encountered. But to write a program for different parts of an XML data structure, you need to specify the part of the structure you are talking about at any given time. XPath is that specification language. It is an addressing mechanism that lets you specify a path to an element so that, for example, <tt>&lt;article&gt;&lt;title&gt;</tt> can be distinguished from <tt>&lt;person&gt;&lt;title&gt;</tt>. In that way, you can describe different kinds of translations for the different <tt>&lt;title&gt;</tt> elements.

The remainder of this section describes the packages that make up the JAXP Transformation APIs.

<a name="gchlx" id="gchlx"></a>

## JAXP Transformation Packages

Here is a description of the packages that make up the JAXP Transformation APIs:

This package defines the factory class you use to get a <tt>Transformer</tt> object. You then configure the transformer with input (source) and output (result) objects, and invoke its <tt>transform()</tt> method to make the transformation happen. The source and result objects are created using classes from one of the other three packages.

Defines the <tt>DOMSource</tt> and <tt>DOMResult</tt> classes, which let you use a DOM as an input to or output from a transformation.

Defines the <tt>SAXSource</tt> and <tt>SAXResult</tt> classes, which let you use a SAX event generator as input to a transformation, or deliver SAX events as output to a SAX event processor.

Defines the <tt>StreamSource</tt> and <tt>StreamResult</tt> classes, which let you use an I/O stream as an input to or output from a transformation.

<a name="ggywi" id="ggywi"></a>

## XSLT Sample Programs

Unlike for the other lessons in this tutorial, the sample programs used in this lesson are not included in the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory provided with the JAXP 1.4.2 Reference Implementation. However you can [download a ZIP file of the XSLT samples here](../examples/xslt_samples.zip).

Unlike for the other lessons in this tutorial, the sample programs used in this lesson are not included in the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory provided with the JAXP 1.4.2 Reference Implementation.
However you can
[`download a ZIP file of the XSLT samples here`](../examples/xslt_samples.zip).

