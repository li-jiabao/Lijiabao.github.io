---
title: Long Term Persistence
author: lijiabao
date: 2020-12-06 01:47:22.307005900 +0800
category: JavaBeans(TM)
categories: JavaBeans(TM)
tags: JavaBeans(TM)
---

# Long Term Persistence

**Long-term persistence** is a model that enables beans to be saved in XML format.

Information on the XML format and on how to implement long-term persistence for non-beans can be found in 
[XML Schema](http://www.oracle.com/technetwork/java/persistence3-139471.html) and 
[Using XMLEncoder](http://www.oracle.com/technetwork/java/persistence4-140124.html).

## Encoder and Decoder

The 
[`XMLEncoder`](https://docs.oracle.com/javase/8/docs/api/java/beans/XMLEncoder.html) class is assigned to write output files for textual representation of `Serializable` objects. The following code fragment is an example of writing a Java bean and its properties in XML format:

```

XMLEncoder encoder = new XMLEncoder(
           new BufferedOutputStream(
           new FileOutputStream("Beanarchive.xml")));

encoder.writeObject(object);
encoder.close(); 

```

The 
[`XMLDecoder`](https://docs.oracle.com/javase/8/docs/api/java/beans/XMLDecoder.html) class reads an XML document that was created with XMLEncoder:

```

XMLDecoder decoder = new XMLDecoder(
    new BufferedInputStream(
    new FileInputStream("Beanarchive.xml")));

Object object = decoder.readObject();
decoder.close();

```

## What's in XML?

An XML bean archive has its own specific syntax, which includes the following tags to represent each bean element:

- an XML preamble to describe a version of XML and type of encoding
- a `**&lt;java&gt;**` tag to embody all object elements of the bean
<li>an `**&lt;object&gt;**` tag to represent a set of method calls needed to reconstruct an object from its serialized form
<pre><code>
&lt;object class="javax.swing.JButton" method="new"&gt;
    &lt;string&gt;Ok&lt;/string&gt;
&lt;/object&gt;
</code></pre>
or statements
<pre><code>
&lt;object class="javax.swing.JButton"&gt;
    &lt;void method="setText"&gt;
        &lt;string&gt;Cancel&lt;/string&gt;
            &lt;/void&gt;
    &lt;/object&gt;
</code></pre>
</li>
<li>tags to define appropriate primitive types:
<ul>
- `**&lt;boolean&gt;**`
- `**&lt;byte&gt;**`
- `**&lt;char&gt;**`
- `**&lt;short&gt;**`
- `**&lt;int&gt;**`
- `**&lt;long&gt;**`
- `**&lt;float&gt;**`
- `**&lt;double&gt;**`

```

&lt;int&gt;5555&lt;/int&gt;

```

```

&lt;class&gt;java.swing.JFrame&lt;/class&gt;

```

```

&lt;array class="java.lang.String" length="5"&gt;
&lt;/array&gt;

```

The following code represents an XML archive that will be generated for the `SimpleBean` component:

```

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;
&lt;java&gt;
  &lt;object class="javax.swing.JFrame"&gt;
    &lt;void method="add"&gt;
      &lt;object class="java.awt.BorderLayout" field="CENTER"/&gt;
      &lt;object class="SimpleBean"/&gt;
    &lt;/void&gt;
    &lt;void property="defaultCloseOperation"&gt;
      &lt;object class="javax.swing.WindowConstants" field="DISPOSE_ON_CLOSE"/&gt;
    &lt;/void&gt;
    &lt;void method="pack"/&gt;
    &lt;void property="visible"&gt;
      &lt;boolean&gt;true&lt;/boolean&gt;
    &lt;/void&gt;
  &lt;/object&gt;
&lt;/java&gt;

```
