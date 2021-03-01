
# Putting it All Together - DnD and CCP

We have shown how to implement drag and drop support and how to implement cut, copy, paste support. How do you combine both in one component?

You implement both within the `TransferHandler`'s `importData` method, like this:

```

if (transferSupport.isDrop()) {
    // put data in transferSupport.getDropLocation()
} else {
    // determine where you want the paste to go (ex: after current selection)
    // put data there
}

```

The `ListCutPaste` example, discussed on the 
[CCP in a non-Text Component](listpaste.html) page, supports both dnd and ccp. Here is its `importData` method (the `if`-`else` drop logic is bolded):

```

    public boolean importData(TransferHandler.TransferSupport info) {
        String data = null;

        //If we cannot handle the import, bail now.
        if (!canImport(info)) {
            return false;
        }

        JList list = (JList)info.getComponent();
        DefaultListModel model = (DefaultListModel)list.getModel();
        //Fetch the data -- bail if this fails
        try {
            data = (String)info.getTransferable().getTransferData(DataFlavor.stringFlavor);
        } catch (UnsupportedFlavorException ufe) {
            System.out.println("importData: unsupported data flavor");
            return false;
        } catch (IOException ioe) {
            System.out.println("importData: I/O exception");
            return false;
        }

        **if (info.isDrop()) { //This is a drop**
            JList.DropLocation dl = (JList.DropLocation)info.getDropLocation();
            int index = dl.getIndex();
            if (dl.isInsert()) {
                model.add(index, data);
                return true;
            } else {
                model.set(index, data);
                return true;
            }
        **} else { //This is a paste**
            int index = list.getSelectedIndex();
            // if there is a valid selection,
            // insert data after the selection
            if (index &gt;= 0) {
                model.add(list.getSelectedIndex()+1, data);
            // else append to the end of the list
            } else {
                model.addElement(data);
            }
            return true;
        **}**
    }

```

This is the only place where you need to install `if`-`else` logic to distinguish between dnd and ccp.
