---
title: Normalizing Text
author: lijiabao
date: 2020-12-06 01:51:32.836286400 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Normalizing Text

**Normalization** is the process by which you can perform certain transformations of text to make it reconcilable in a way which it may not have been before. Let's say, you would like searching or sorting text, in this case you need to normalize that text to account for code points that should be represented as the same text.

What can be normalized? The normalization is applicable when you need to convert characters with diacritical marks, change all letters case, decompose ligatures, or convert half-width katakana characters to full-width characters and so on.

In accordance with the 
[Unicode Standard Annex #15](http://www.unicode.org/reports/tr15/) the Normalizer's API supports all of the following four Unicode text normalization forms that are defined in the 
[`java.text.Normalizer.Form`](https://docs.oracle.com/javase/8/docs/api/java/text/Normalizer.Form.html):

- Normalization Form D (NFD): Canonical Decomposition
- Normalization Form C (NFC): Canonical Decomposition, followed by Canonical Composition
- Normalization Form KD (NFKD): Compatibility Decomposition
- Normalization Form KC (NFKC): Compatibility Decomposition, followed by Canonical Composition

Let's examine how the latin small letter "o" with diaeresis can be normalized by using these normalization forms:
<th id="h1">Original word</th><th id="h2">NFC</th><th id="h3">NFD</th><th id="h4">NFKC</th><th id="h5">NFKD</th>
<td headers="h1">"sch&#246;n"</td><td headers="h2">"sch&#246;n"</td><td headers="h3">"scho\u0308n"</td><td headers="h4">"sch&#246;n"</td><td headers="h5">"scho\u0308n"</td>

You can notice that an original word is left unchanged in NFC and NFKC. This is because with NFD and NFKD, composite characters are mapped to their canonical decompositions. But with NFC and NFKC, combining character sequences are mapped to composites, if possible. There is no composite for diaeresis, so it is left decomposed in NFC and NFKC.

In the code example, 
[`NormSample.java`](examples/NormSample.java), which is represented later, you can also notice another normalization feature. The half-width and full-width katakana characters will have the same compatibility decomposition and are thus compatibility equivalents. However, they are not canonical equivalents.

To be sure that you really need to normalize the text you may use the `isNormalized` method to determine if the given sequence of char values is normalized. If this method returns false, it means that you have to normalize this sequence and you should use the `normalize` method which normalizes a `char` values according to the specified normalization form. For example, to transform text into the canonical decomposed form you will have to use the following `normalize` method:

```

normalized_string = Normalizer.normalize(target_chars, Normalizer.Form.NFD);

```

Also, the normalize method rearranges accents into the proper canonical order, so that you do not have to worry about accent rearrangement on your own.

The following example represents an application that enables you to select a normalization form and a template to normalize:

<applet code="NormSample" archive="examples/lib/NormSample.jar" width="550" height="230" alt="NormSample applet"><param name="permissions" value="sandbox" /></applet>

The complete code for this applet is in 
[`NormSample.java`](examples/NormSample.java)
