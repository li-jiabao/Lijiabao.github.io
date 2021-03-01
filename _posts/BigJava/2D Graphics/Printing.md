
# Lesson: Printing

Since the Java 2D API enables you to draw on any surface, a natural extension of that is the ability to print Java 2D graphics. A printer can be considered a graphics device just like a display.

The Java 2D printing API is not limited to printing graphics. It enables you to print the content of an application's user interface as well. Content can be printed by sending raw data to the printer under the formatting control of the Java 2D printing API, or by using the Java 2D Graphics API.

In this lesson you will explore the printer and job control functions of the Java 2D printing API which are complements to the rendering elements. You will learn how to look up printers configured on the system or network and discover information about these printers such as the paper sizes it supports, and selecting these attributes for printing, and user dialogs.

The main classes and interfaces involved in printing are represented in the 
[`java.awt.print`](https://docs.oracle.com/javase/8/docs/api/java/awt/print/package-frame.html) and
[`javax.print`](https://docs.oracle.com/javase/8/docs/api/javax/print/package-frame.html) packages (the last package allows you to get access to the printing services).

The basic printing operations are represented in the following sections:

<li>
[A Basic Printing Program](../printing/printable.html) &#8211; this section describes the `Printable` interface and presents a basic printing program.</li>
<li>
[Using Print Setup Dialogs](../printing/dialog.html)&#8211; this sections explains how to display the Print Setup Dialog.</li>
<li>
[Printing a Multiple Page Document](../printing/set.html) &#8211; this section explains how to use pagination for printing a multiple page document.</li>
<li>
[Working with Print Services and Attributes](../printing/services.html) ndash; this section teaches you about print services, how to specify the print data format, and how to create print job using the `javax.print` package.</li>
<li>
[Printing the Contents of a User Interface](../printing/gui.html) &#8211; this section explains how to print the contents of a window or a frame.</li>
<li>
[Printing Support in Swing Components](../printing/swing.html) ndash; this section provides a brief description of the related printing functionality in `Swing` and refers to specific `Swing` classes and interfaces.</li>
