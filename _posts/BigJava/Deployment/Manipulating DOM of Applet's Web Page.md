
# Manipulating DOM of Applet's Web Page

Every web page is composed of a series of nested objects. These objects make up the Document Object Model (DOM). A Java applet can traverse and modify objects of its parent web page using the 
[`Common DOM API`](https://docs.oracle.com/javase/8/docs/jre/api/plugin/dom/index.html).

Consider an example of a Java applet that dumps the contents of its parent web page.

In order to traverse and manipulate the DOM tree, you must first obtain a reference to the `Document` object for the web page. You can do so by using the `getDocument` method in the `com.sun.java.browser.plugin2.DOM` class. Here is a code snippet that retrieves a reference to a `Document` object in the 
[`<code>DOMDump`</code>](examples/applet_TraversingDOM/src/DOMDump.java) applet's `start` method. See inline comments in the code.

```

public void start() {
    try {
        // use reflection to get document
        Class c =
          Class.forName("com.sun.java.browser.plugin2.DOM");
        Method m = c.getMethod("getDocument",
          new Class[] { java.applet.Applet.class });
        
        // cast object returned as HTMLDocument;
        // then traverse or modify DOM
        HTMLDocument doc = (HTMLDocument) m.invoke(null,
            new Object[] { this });
        HTMLBodyElement body =
            (HTMLBodyElement) doc.getBody();
        dump(body, INDENT);
    } catch (Exception e) {
        System.out.println("New Java Plug-In not available");
        // In this case, you could fallback to the old
        // bootstrapping mechanism available in the
        // com.sun.java.browser.plugin.dom package
    }
}


```

Now that you have a reference to the `Document` object, you can traverse and modify the DOM tree using the Common DOM API. The `DOMDump` applet traverses the DOM tree and writes its contents to the Java Console log.

```

private void dump(Node root, String prefix) {
    if (root instanceof Element) {
        System.out.println(prefix +
            ((Element) root).getTagName() + 
            " / " + root.getClass().getName());
    } else if (root instanceof CharacterData) {
        String data =
            ((CharacterData) root).getData().trim();
        if (!data.equals("")) {
            System.out.println(prefix +
                "CharacterData: " + data);
        }
    } else {
        System.out.println(prefix +
            root.getClass().getName());
    }
    NamedNodeMap attrs = root.getAttributes();
    if (attrs != null) {
        int len = attrs.getLength();
        for (int i = 0; i &lt; len; i++) {
            Node attr = attrs.item(i);
            System.out.print(prefix + HALF_INDENT +
                "attribute " + i + ": " +
                attr.getNodeName());
            if (attr instanceof Attr) {
                System.out.print(" = " +
                    ((Attr) attr).getValue());
            }
            System.out.println();
        }
    }

    if (root.hasChildNodes()) {
        NodeList children = root.getChildNodes();
        if (children != null) {
            int len = children.getLength();
            for (int i = 0; i &lt; len; i++) {
                dump(children.item(i), prefix +
                    INDENT);
            }
        }
    }
}

```

Open 
[`<code>AppletPage.html`</code>](examples/dist/applet_TraversingDOM/AppletPage.html) in a browser to view the `DOMDump` applet running. Check the Java Console log for a dump of the DOM tree of the web page.


[Download the source code](examplesIndex.html#ManipulatingDOM) for the **DOM Dump** example to experiment further.
