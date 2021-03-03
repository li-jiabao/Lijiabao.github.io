---
title: Accessing Cookies
author: lijiabao
date: 2020-12-06 12:43:44.150095500 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Accessing Cookies

You can set and retrieve cookies in your rich Internet application (RIA). Cookies can enhance the capabilities of your RIA. For example, consider the scenario where you have applets on various web pages. An applet on a web page cannot directly access or share information with an applet on another web page. In this scenario, cookies provide an important connection between applets and help one applet pass information to another applet on a different web page. Java Web Start applications can also use cookies to store information on the client.

The Cookie Applet example has a 
[`CookieAccessor`](examples/applet_AccessingCookies/src/CookieAccessor.java ) class that retrieves and sets cookies.

## <a id="retrieving" name="retrieving"></a>Retrieving Cookies

The following code snippet shows the `getCookieUsingCookieHandler` method of the `CookieAccessor` class:

```

public void getCookieUsingCookieHandler() { 
    try {       
        // Instantiate CookieManager;
        // make sure to set CookiePolicy
        CookieManager manager = new CookieManager();
        manager.setCookiePolicy(CookiePolicy.ACCEPT_ALL);
        CookieHandler.setDefault(manager);

        // get content from URLConnection;
        // cookies are set by web site
        URL url = new URL("http://host.example.com");
        URLConnection connection = url.openConnection();
        connection.getContent();

        // get cookies from underlying
        // CookieStore
        CookieStore cookieJar =  manager.getCookieStore();
        List &lt;HttpCookie&gt; cookies =
            cookieJar.getCookies();
        for (HttpCookie cookie: cookies) {
          System.out.println("CookieHandler retrieved cookie: " + cookie);
        }
    } catch(Exception e) {
        System.out.println("Unable to get cookie using CookieHandler");
        e.printStackTrace();
    }
}  

```

The 
[`CookieManager`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieManager.html) class is the main entry point for cookie management. Create an instance of the `CookieManager` class and set its 
[`CookiePolicy`](https://docs.oracle.com/javase/8/docs/api/java/net/CookiePolicy.html). Set this instance of the `CookieManager` as the default 
[`CookieHandler`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieHandler.html).

Open a 
[`URLConnection`](https://docs.oracle.com/javase/8/docs/api/java/net/URLConnection.html) to the website of your choice.

Next, retrieve cookies from the underlying 
[`CookieStore`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieStore.html) by using the `getCookies` method.

## Setting Cookies

The following code snippet shows the `setCookieUsingCookieHandler` method of the `CookieAccessor` class:

```

public void setCookieUsingCookieHandler() {
    try {
        // instantiate CookieManager
        CookieManager manager = new CookieManager();
        CookieHandler.setDefault(manager);
        CookieStore cookieJar =  manager.getCookieStore();

        // create cookie
        HttpCookie cookie = new HttpCookie("UserName", "John Doe");

        // add cookie to CookieStore for a
        // particular URL
        URL url = new URL("http://host.example.com");
        cookieJar.add(url.toURI(), cookie);
        System.out.println("Added cookie using cookie handler");
    } catch(Exception e) {
        System.out.println("Unable to set cookie using CookieHandler");
        e.printStackTrace();
    }
}

```

As shown in [Retrieving Cookies](#retrieving), the 
[`CookieManager`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieManager.html) class is the main entry point for cookie management. Create an instance of the `CookieManager` class and set the instance as the default 
[`CookieHandler`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieHandler.html).

Create the desired 
[`HttpCookie`](https://docs.oracle.com/javase/8/docs/api/java/net/HttpCookie.html) with the necessary information. In our example, we have created a new `HttpCookie` that sets the `UserName` as `John Doe`.

Next, add the cookie to the underlying cookie store.

## Running the Cookie Applet Example

To access cookies, you must sign your RIA JAR file and request permission to run outside of the security sandbox. See the documentation for the 
[`jarsigner`](https://docs.oracle.com/javase/8/docs/technotes/tools/index.html#security) tool to learn how to sign JAR files. See
[Security in Rich Internet Applications ](../doingMoreWithRIA/security.html)for information on requesting permissions. 


[Download source code](examplesIndex.html#AccessingCookies) for the **Cookie Applet** example to experiment further.

## Running the Cookie Applet Example

Open 
[`AppletPage.html`](examples/dist/applet_AccessingCookies/AppletPage.html) in a browser to run the Cookie Applet example. Check the Java Console log for details of cookies that have been set and retrieved. You should see the following output in the Java Console log (session details vary).

```

=== Access cookies using CookieHandler ===
CookieHandler retrieved cookie: JSESSIONID=3bc935c18b8d36319be9497fb892
CookieHandler retrieved cookie: JROUTE=eKVJ4oW0NOer888s
Added cookie using cookie handler
...

```


[Download source code](examplesIndex.html#AccessingCookies) for the **Cookie Applet** example to experiment further.
