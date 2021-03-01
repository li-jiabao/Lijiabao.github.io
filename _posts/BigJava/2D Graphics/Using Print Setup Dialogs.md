
# Using Print Setup Dialogs

Traditionally, the user wants to see the page setup and print dialog boxes. From the print dialog you can select a printer, specify pages to print, and set the number of copies.

An application displays a print dialog when the user presses a button related to the print command, or chooses an item from the print menu. To display this dialog, call the 
[`printDialog`](https://docs.oracle.com/javase/8/docs/api/java/awt/print/PrinterJob.html#printDialog--) method of the 
[`PrinterJob`](https://docs.oracle.com/javase/8/docs/api/java/awt/print/PrinterJob.html) class:

```

PrinterJob pj = PrinterJob.getPrinterJob();
...
    if (pj.printDialog()) {
        try {pj.print();}
        catch (PrinterException exc) {
            System.out.println(exc);
         }
     }   
...    

```

This method returns `true` if the user clicked OK to leave the dialog, and `false` otherwise. The user's choices in the dialog are constrained based on the number and format of the pages that have been set to the `PrinterJob`.

The `printDialog` method in the above code snippet opens a native print dialog. The 
[`PrintDialogExample.java`](examples/PrintDialogExample.java) code example shows how to display a cross-platform print dialog.

You can change the page setup information contained in the 
[`PageFormat`](https://docs.oracle.com/javase/8/docs/api/java/awt/print/PageFormat.html) object by using the page setup dialog.

To display the page setup dialog, call the `pageDialog` method of the `PrinterJob` class.

```

PrinterJob pj = PrinterJob.getPrinterJob();
PageFormat pf = pj.pageDialog(pj.defaultPage());

```

The page setup dialog is initialized using the parameter passed to `pageDialog`. If the user clicks the OK button in the dialog, the `PageFormat` instance will be created in accordance with the user&#239;&#191;&#189;s selections, and then returned. If the user cancels the dialog, `pageDialog` returns the original unchanged PageFormat.

Usually the Java 2D Printing API requires an application to display a print dialog, but in sometimes it's possible to print without showing any dialog at all. This type of printing is called **silent printing**. It may be useful in specific cases, such as, when you need to print a particular database weekly report. In the other cases it is always recommended to inform the user when a print process is starting.
