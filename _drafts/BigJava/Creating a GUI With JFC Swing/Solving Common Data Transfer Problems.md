
# Solving Common Data Transfer Problems

This section discusses problems that you might encounter when using data transfer.

**Problem:** My drag gesture recognizer is not working properly with table/list/tree/text.

Do not use your own drag gesture recognizers with these components. Use `setDragEnabled(true)` and a `TransferHandler`.

**Problem:** I am unable to drop data onto my empty `JTable`.

You need to call `setFillsViewportHeight(true)` on the table. See 
[Empty Table Drop](emptytable.html) for more information.
