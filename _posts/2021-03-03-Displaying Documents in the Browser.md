---
title: Displaying Documents in the Browser
author: lijiabao
date: 2020-12-06 12:42:15.834316300 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Displaying Documents in the Browser

A Java applet can load a web page in a browser window using the `showDocument` methods in the 
[`java.applet.AppletContext`](https://docs.oracle.com/javase/8/docs/api/java/applet/AppletContext.html) class.

Here are the two forms of `showDocument`:

```

public void showDocument(java.net.URL **url**)
public void showDocument(java.net.URL **url**, String **targetWindow**)

```

The one-argument form of `showDocument` simply instructs the browser to display the document at the specified URL, without specifying the window in which to display the document.

The two-argument form of `showDocument` lets you specify the window or HTML frame in which to display the document. The second argument can have one of the folllowing values:

- `"_blank"` &#8211; Display the document in a new, nameless window.
- `"**windowName**"` &#8211; Displays the document in a window named **windowName**. This window is created if necessary.
- `"_self"` &#8211; Display the document in the window and frame that contain the applet.
- `"_parent"` &#8211; Display the document in parent frame of the applet's frame. If the applet frame has no parent frame, this acts the same as `"_self"`.
- `"_top"` &#8211; Display the document in the top-level frame. If the applet's frame is the top-level frame, this acts the same as `"_self"`.

The following applet enables you try every argument of both forms of `showDocument`. The applet opens a window that lets you type in a URL and choose an option for the `targetWindow` argument. When you press Return or click the Show document button, the applet calls `showDocument`.

Following is the applet code that calls `showDocument`. Here is the whole program, 
[`ShowDocument`](examples/applet_ShowDocument/src/ShowDocument.java).

```

        **...//In an Applet subclass:**
        urlWindow = new URLWindow(getAppletContext());
        . . .

class URLWindow extends Frame {
    ...
    public URLWindow(AppletContext appletContext) {
        ...
        this.appletContext = appletContext;
        ...
    }
    ...
    public boolean action(Event event, Object o) {
        ...
            String urlString =
                **/* user-entered string */**;
            URL url = null;
            try {
                url = new URL(urlString);
            } catch (MalformedURLException e) {
                **...//Inform the user and return...**
            }

            if (url != null) {
                if (<em>/* user doesn't want to specify
                           the window */</em>) {
                    appletContext.showDocument(url);
                } else {
                    appletContext.showDocument(url,
                        **/* user-specified window */**);
                }
            }
        ...

```


[Download source code](examplesIndex.html#ShowDocument) for the **Show Document** example to experiment further.
