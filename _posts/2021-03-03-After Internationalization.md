---
title: After Internationalization
author: lijiabao
date: 2020-12-06 01:49:41.218991600 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# After Internationalization

The source code for the internationalized program follows. Notice that the text of the messages is not hardcoded.

```

import java.util.*;

public class I18NSample {

    static public void main(String[] args) {

        String language;
        String country;

        if (args.length != 2) {
            language = new String("en");
            country = new String("US");
        } else {
            language = new String(args[0]);
            country = new String(args[1]);
        }

        Locale currentLocale;
        ResourceBundle messages;

        currentLocale = new Locale(language, country);

        messages = ResourceBundle.getBundle("MessagesBundle", currentLocale);
        System.out.println(messages.getString("greetings"));
        System.out.println(messages.getString("inquiry"));
        System.out.println(messages.getString("farewell"));
    }
}

```

To compile and run this program, you need these source files:

<li>
[`<code>I18NSample.java`</code>](examples/I18NSample.java)</li>
<li>
[`<code>MessagesBundle.properties`</code>](examples/MessagesBundle.properties)</li>
<li>
[`<code>MessagesBundle_de_DE.properties`</code>](examples/MessagesBundle_de_DE.properties)</li>
<li>
[`<code>MessagesBundle_en_US.properties`</code>](examples/MessagesBundle_en_US.properties)</li>
<li>
[`<code>MessagesBundle_fr_FR.properties`</code>](examples/MessagesBundle_fr_FR.properties)</li>
