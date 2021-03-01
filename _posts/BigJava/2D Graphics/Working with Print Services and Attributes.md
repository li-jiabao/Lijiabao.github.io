
# Working with Print Services and Attributes

From the previous lessons you have learned that the Java 2D printing API supports page imaging, displays print and page setup dialogs, and specifies printing attributes. Printing services is another key component of any printing subsystem.

The **Java Print Service (JPS) API** extends the current Java 2D printing features to offer the following functionality:

- Application discovers printers that cater to its needs by dynamically querying the printer capabilities.
- Application extends the attributes included with the JPS API.
- Third parties can plug in their own print services with the Service Provider Interface, which print different formats, including Postscript, PDF, and SVG.

The Java Print Service API consists of four packages:

The 
[`javax.print`](https://docs.oracle.com/javase/8/docs/api/javax/print/package-summary.html) package provides the principal classes and interfaces for the Java Print Service API. It enables client and server applications to:

- Discover and select print services based on their capabilities.
- Specify the format of print data.
- Submit print jobs to services that support the document type to be printed.

## Document Type Specification

The 
[`DocFlavor`](https://docs.oracle.com/javase/8/docs/api/javax/print/DocFlavor.html) class represents format of the print data, such as JPEG or PostScript. The `DocFlavor` format consists of two parts: a MIME type and a representation class name. A MIME type describes the format, and a document representation class name indicates how the document is delivered to the printer or output stream. An application uses the `DocFlavor` and an attribute set to find printers with the capabilities specified by the attribute set. This code sample demonstrates obtaining an array of `StreamPrintServiceFactory` objects that can return `StreamPrintService` objects able to convert a GIF image into PostScript:

```

DocFlavor flavor  = DocFlavor.INPUT_STREAM.GIF;
String psMimeType = DocFlavor.BYTE_ARRAY.
                    POSTSCRIPT.getMimeType();
StreamPrintServiceFactory[] psfactories =
              StreamPrintServiceFactory.
              lookupStreamPrintServiceFactories(
              flavor, psMimeType);

```

## Attribute Definitions

The 
[`javax.print.attribute`](https://docs.oracle.com/javase/8/docs/api/javax/print/attribute/package-frame.html) and 
[`javax.print.attribute.standard`](https://docs.oracle.com/javase/8/docs/api/javax/print/attribute/standard/package-frame.html) packages define print attributes which describe the capabilities of a print service, specify the requirements of a print job, and track the progress of the print job.

For example, if you would like to use A4 paper format and print three copies of your document you will have to create a set of the following attributes implementing the `PrintRequestAttributeSet` interface:

```

PrintRequestAttributeSet attr_set =
    new HashPrintRequestAttributeSet();
attr_set.add(MediaSize.ISO_A4); 
attr_set.add(new Copies(3)); 

```

Then you must pass the attribute set to the print job's `print` method, along with the `DocFlavor`.

## Print Service Discovery

An application invokes the static methods of the abstract class `PrintServiceLookup` to locate print services that have the capabilities to satisfy the application's print request. For example, in order to print two copies of a double-sided document, the application first needs to find printers that have double-sided printing capability:

```

DocFlavor doc_flavor = DocFlavor.INPUT_STREAM.PDF;
PrintRequestAttributeSet attr_set =
    new HashPrintRequestAttributeSet();
attr_set.add(new Copies(2));
attr_set.add(Sides.DUPLEX);
PrintService[] service = PrintServiceLookup.
              lookupPrintServices(doc_flavor,
              attr_set);

```

## Common Use of the API

In conclusion, the Java Print Service API performs the following steps to process a print request:

1. Chooses a `DocFlavor`.
1. Creates a set of attributes.
1. Locates a print service that can handle the print request as specified by the `DocFlavor` and the attribute set.
1. Creates a `Doc` object encapsulating the `DocFlavor` and the actual print data.
1. Gets a print job, represented by `DocPrintJob`, from the print service.
1. Calls the `print` method of the print job.

For more information about Java Print Service, see 
[Java 2D Print Service API User Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/jps/spec/JPSTOC.fm.html).
