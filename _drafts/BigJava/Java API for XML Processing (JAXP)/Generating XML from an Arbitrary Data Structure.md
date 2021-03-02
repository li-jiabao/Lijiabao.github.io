
# Transforming XML Data with XSLT

The Extensible Stylesheet Language Transformations (XSLT) APIs can be used for many purposes. For example, with a sufficiently intelligent stylesheet, you could generate PDF or PostScript output from the XML data. But generally, XSLT is used to generate formatted HTML output, or to create an alternative XML representation of the data.

In this section, an XSLT transform is used to translate XML input data to HTML output.

**Note -** The 
[XSLT specification](http://www.w3.org/TR/xslt20/) is large and complex, so this tutorial can only scratch the surface. It will give you a little background so you can understand simple XSLT processing tasks, but it does not examine in detail how to write an XSLT transform, rather concentrating on how to use JAXP's XSLT transform API. For a more thorough grounding in XSLT, consult a good reference manual, such as Michael Kay's *XSLT 2.0 and XPath 2.0: Programmer's Reference* (Wrox, 2008).

<a name="ggyvk" id="ggyvk"></a>

## Defining a Simple Document Type

Start by defining a very simple document type that can be used for writing articles. Our <tt>article</tt> documents will contain these structure tags:

<li>
<tt>&lt;TITLE&gt;</tt>: The title of the article
</li>
<li>
<tt>&lt;SECT&gt;</tt>: A section, consisting of a heading and a body
</li>
<li>
<tt>&lt;PARA&gt;</tt>: A paragraph
</li>
<li>
<tt>&lt;LIST&gt;</tt>: A list
</li>
<li>
<tt>&lt;ITEM&gt;</tt>: An entry in a list
</li>
<li>
<tt>&lt;NOTE&gt;</tt>: An aside, that is offset from the main text
</li>

The slightly unusual aspect of this structure is that we will not create a separate element tag for a section heading. Such elements are commonly created to distinguish the heading text (and any tags it contains) from the body of the section (that is, any structure elements underneath the heading).

Instead, we will allow the heading to merge seamlessly into the body of a section. That arrangement adds some complexity to the stylesheet, but it will give us a chance to explore XSLT's template-selection mechanisms. It also matches our intuitive expectations about document structure, where the text of a heading is followed directly by structure elements, an arrangement that can simplify outline-oriented editing.

**Note -** This kind of structure is not easily validated, because XML's mixed-content model allows text anywhere in a section, whereas we want to confine text and inline elements so that they appear only before the first structure element in the body of the section. The assertion-based validator can do it, but most other schema mechanisms cannot. So we will dispense with defining a DTD for the document type.

In this structure, sections can be nested. The depth of the nesting will determine what kind of HTML formatting to use for the section heading (for example, <tt>h1</tt> or <tt>h2</tt>). Using a plain <tt>SECT</tt> tag (instead of numbered sections) is also useful with outline-oriented editing, because it lets you move sections around at will without having to worry about changing the numbering for any of the affected sections.

For lists, we will use a type attribute to specify whether the list entries are unordered (bulleted), alpha (enumerated with lowercase letters), ALPHA (enumerated with uppercase letters), or numbered.

We will also allow for some inline tags that change the appearance of the text.

<li>
<tt>&lt;B&gt;</tt>: Bold
</li>
<li>
<tt>&lt;I&gt;</tt>: Italics
</li>
<li>
<tt>&lt;U&gt;</tt>: Underline
</li>
<li>
<tt>&lt;DEF&gt;</tt>: Definition
</li>
<li>
<tt>&lt;LINK&gt;</tt>: Link to a URL
</li>

**Note -** An inline tag does not generate a line break, so a style change caused by an inline tag does not affect the flow of text on the page (although it will affect the appearance of that text). A structure tag, on the other hand, demarcates a new segment of text, so at a minimum it always generates a line break in addition to other format changes.

The <tt>&lt;DEF&gt;</tt> tag will be used for terms that are defined in the text. Such terms will be displayed in italics, the way they ordinarily are in a document. But using a special tag in the XML will allow an index program to find such definitions and add them to an index, along with keywords in headings. In the preceding Note, for example, the definitions of inline tags and structure tags could have been marked with <tt>&lt;DEF&gt;</tt> tags for future indexing.

Finally, the <tt>LINK</tt> tag serves two purposes. First, it will let us create a link to a URL without having to put the URL in twice; so we can code <tt>&lt;link&gt;http//...&lt;/link&gt;</tt> instead of <tt>&lt;a href="http//..."&gt;http//...&lt;/a&gt;</tt>. Of course, we will also want to allow a form that looks like <tt>&lt;link target="..."&gt;...name...&lt;/link&gt;</tt>. That leads to the second reason for the <tt>&lt;link&gt;</tt> tag. It will give us an opportunity to play with conditional expressions in XSLT.

**Note -** Although the article structure is exceedingly simple (consisting of only eleven tags), it raises enough interesting problems to give us a good view of XSLT's basic capabilities. But we will still leave large areas of the specification untouched. In [What Else Can XSLT Do?](#ggyut), we will point out the major features we skipped.

<a name="gghmv" id="gghmv"></a>

## Creating a Test Document

Here, you will create a simple test document using nested <tt>&lt;SECT&gt;</tt> elements, a few &lt;PARA&gt; elements, a <tt>&lt;NOTE&gt;</tt> element, a <tt>&lt;LINK&gt;</tt>, and a <tt>&lt;LIST type="unordered"&gt;</tt>. The idea is to create a document with one of everything so that we can explore the more interesting translation mechanisms.

**Note -** The code discussed in this section is in <tt>article1.xml</tt>, which is found in the <tt>xslt/data</tt> directory after you unzip [XSLT examples](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

**Note -** The code discussed in this section is in <tt>article1.xml</tt>, which is found in the <tt>xslt/data</tt> directory after you unzip
[`XSLT examples`](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

To make the test document, create a file called <tt>article.xml</tt> and enter the following XML data.

```

&lt;?xml version="1.0"?&gt;
&lt;ARTICLE&gt;
   &lt;TITLE&gt;A Sample Article&lt;/TITLE&gt;
   &lt;SECT&gt;The First Major Section
      &lt;PARA&gt;This section will introduce a subsection.&lt;/PARA&gt;
      &lt;SECT&gt;The Subsection Heading
         &lt;PARA&gt;This is the text of the subsection.
         &lt;/PARA&gt;
      &lt;/SECT&gt;
   &lt;/SECT&gt;
&lt;/ARTICLE&gt;

```

Note that in the XML file, the subsection is totally contained within the major section. (In HTML, on the other hand, headings do not contain the body of a section). The result is an outline structure that is harder to edit in plain text form, like this, but is much easier to edit with an outline-oriented editor.

Someday, given a tree-oriented XML editor that understands inline tags such as <tt>&lt;B&gt;</tt> and <tt>&lt;I&gt;</tt>, it should be possible to edit an article of this kind in outline form, without requiring a complicated stylesheet. (Such an editor would allow the writer to focus on the structure of the article, leaving layout until much later in the process). In such an editor, the article fragment would look something like this:

```

&lt;ARTICLE&gt; 
 &lt;TITLE&gt;A Sample Article 
  &lt;SECT&gt;The First Major Section 
   &lt;PARA&gt;This section will 
            introduce a subsection.
    &lt;SECT&gt;The Subheading 
     &lt;PARA&gt;This is the text of the subsection. 
         Note that ...

```

**Note -** At the moment, tree-structured editors exist, but they treat inline tags such as <tt>&lt;B&gt;</tt> and <tt>&lt;I&gt;</tt> in the same way that they treat structure tags, and that can make the "outline" a bit difficult to read.

<a name="gghnb" id="gghnb"></a>

## Writing an XSLT Transform

Now it is time to begin writing an XSLT transform that will convert the XML article and render it in HTML.

**Note -** The code discussed in this section is in <tt>article1a.xsl</tt>, which is found in the <tt>xslt/data</tt> directory after you unzip [XSLT examples](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

**Note -** The code discussed in this section is in <tt>article1a.xsl</tt>, which is found in the <tt>xslt/data</tt> directory after you unzip
[`XSLT examples`](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

Start by creating a normal XML document:

```

&lt;?xml version="1.0" encoding="ISO-8859-1"?&gt;

```

Then add the following highlighted lines to create an XSL stylesheet:

```

&lt;?xml version="1.0" encoding="ISO-8859-1"?&gt;
**&lt;xsl:stylesheet **
<b> xmlns:xsl=
    "http://www.w3.org/1999/XSL/Transform" </b>
** version="1.0"**
** &gt;**
****
**&lt;/xsl:stylesheet&gt;**

```

Now set it up to produce HTML-compatible output.

```

&lt;xsl:stylesheet 
[...]

   &gt;
**&lt;xsl:output method="html"/&gt;**

[...]

&lt;/xsl:stylesheet&gt;

```

We will get into the detailed reasons for that entry later in this section. For now, note that if you want to output anything other than well-formed XML, then you will need an <tt>&lt;xsl:output&gt;</tt> tag like the one shown, specifying either <tt>text</tt> or <tt>html</tt>. (The default value is <tt>xml</tt>).

**Note -** When you specify XML output, you can add the indent attribute to produce nicely indented XML output. The specification looks like this: <tt>&lt;xsl:output method="xml" indent="yes"/&gt;</tt>.

<a name="ggyvc" id="ggyvc"></a>

## Processing the Basic Structure Elements

You will start filling in the stylesheet by processing the elements that go into creating a table of contents: the root element, the title element, and headings. You will also process the <tt>PARA</tt> element defined in the test document.

**Note -** If on first reading you skipped the section that discusses the XPath addressing mechanisms, 
[How XPath Works](xpath.html), now is a good time to go back and review that section.

Begin by adding the main instruction that processes the root element:

```

** &lt;xsl:template match="/"&gt;**
      &lt;html&gt;&lt;body&gt;
         **&lt;xsl:apply-templates/&gt;**
      &lt;/body&gt;&lt;/html&gt;
   **&lt;/xsl:template&gt;**

&lt;/xsl:stylesheet&gt;

```

The new XSL commands are shown in bold. (Note that they are defined in the <tt>xsl</tt> namespace). The instruction <tt>&lt;xsl:apply-templates&gt;</tt> processes the children of the current node. In this case, the current node is the root node.

Despite its simplicity, this example illustrates a number of important ideas, so it is worth understanding thoroughly. The first concept is that a stylesheet contains a number of templates, defined with the <tt>&lt;xsl:template&gt;</tt> tag. Each template contains a match attribute, which uses the XPath addressing mechanisms described in 
[How XPath Works](xpath.html) to select the elements that the template will be applied to.

Within the template, tags that do not start with the <tt>xsl: namespace</tt> prefix are simply copied. The newlines and whitespace that follow them are also copied, and that helps to make the resulting output readable.

**Note -** When a newline is not present, whitespace is generally ignored. To include whitespace in the output in such cases, or to include other text, you can use the <tt>&lt;xsl:text&gt;</tt> tag. Basically, an XSLT stylesheet expects to process tags. So everything it sees needs to be either an <tt>&lt;xsl:..&gt;</tt> tag, some other tag, or whitespace.

In this case, the non-XSL tags are HTML tags. So when the root tag is matched, XSLT outputs the HTML start tags, processes any templates that apply to children of the root, and then outputs the HTML end tags.

<a name="ggyxi" id="ggyxi"></a>

## Process the <tt>&lt;TITLE&gt;</tt> Element

Next, add a template to process the article title:

```

** &lt;xsl:template match="/ARTICLE/TITLE"&gt;**
<b> &lt;h1 align="center"&gt; 
    &lt;xsl:apply-templates/&gt; &lt;/h1&gt;</b>
** &lt;/xsl:template&gt;**

&lt;/xsl:stylesheet&gt;

```

In this case, you specify a complete path to the TITLE element and output some HTML to make the text of the title into a large, centered heading. In this case, the <tt>apply-templates</tt> tag ensures that if the title contains any inline tags such as italics, links, or underlining, they also will be processed.

More importantly, the <tt>apply-templates</tt> instruction causes the text of the title to be processed. Like the DOM data model, the XSLT data model is based on the concept of text nodes contained in element nodes (which, in turn, can be contained in other element nodes, and so on). That hierarchical structure constitutes the source tree. There is also a result tree, which contains the output.

XSLT works by transforming the source tree into the result tree. To visualize the result of XSLT operations, it is helpful to understand the structure of those trees, and their contents. (For more on this subject, see 
[XSLT/XPath Data Model](xpath.html#gchlm)).

<a name="ggyuk" id="ggyuk"></a>

## Process Headings

To continue processing the basic structure elements, add a template to process the top-level headings:

```

<b> &lt;xsl:template match=
    "/ARTICLE/SECT"&gt;</b>
** &lt;h2&gt; &lt;xsl:apply-templates**
<b> select="text()|B|I|U|DEF|LINK"/&gt; 
&lt;/h2&gt;</b>
<b> &lt;xsl:apply-templates select=
    "SECT|PARA|LIST|NOTE"/&gt;</b>
** &lt;/xsl:template&gt;**

&lt;/xsl:stylesheet&gt;

```

Here, you specify the path to the topmost <tt>SECT</tt> elements. But this time, you apply templates in two stages using the <tt>select</tt> attribute. For the first stage, you select text nodes, as well as inline tags such as bold and italics, using the XPath <tt>text()</tt> function. (The vertical pipe (<tt>|</tt>) is used to match multiple items: text or a bold tag or an italics tag, etc). In the second stage, you select the other structure elements contained in the file, for sections, paragraphs, lists, and notes.

Using the select attribute lets you put the text and inline elements between the <tt>&lt;h2&gt;...&lt;/h2&gt;</tt> tags, while making sure that all the structure tags in the section are processed afterward. In other words, you make sure that the nesting of the headings in the XML document is not reflected in the HTML formatting, a distinction that is important for HTML output.

In general, using the select clause lets you apply all templates to a subset of the information available in the current context. As another example, this template selects all attributes of the current node:

```

&lt;xsl:apply-templates select="@*"/&gt;&lt;/attributes&gt;

```

Next, add the virtually identical template to process subheadings that are nested one level deeper:

```

<b> &lt;xsl:template match=
    "/ARTICLE/SECT/SECT"&gt;</b>
** &lt;h3&gt; &lt;xsl:apply-templates**
<b> select="text()|B|I|U|DEF|LINK"/&gt; 
&lt;/h3&gt;</b>
<b> &lt;xsl:apply-templates select=
    "SECT|PARA|LIST|NOTE"/&gt;</b>
** &lt;/xsl:template&gt;**

&lt;/xsl:stylesheet&gt;

```

<a name="ggyuo" id="ggyuo"></a>

## Generate a Runtime Message

You could add templates for deeper headings, too, but at some point you must stop, if only because HTML goes down only to five levels. For this example, you will stop at two levels of section headings. But if the XML input happens to contain a third level, you will want to deliver an error message to the user. This section shows you how to do that.

**Note -** We could continue processing <tt>SECT</tt> elements that are further down, by selecting them with the expression <tt>/SECT/SECT//SECT</tt>. The <tt>//</tt> selects any <tt>SECT</tt> elements, at any depth, as defined by the XPath addressing mechanism. But instead we will take the opportunity to play with messaging.

Add the following template to generate an error when a section is encountered that is nested too deep:

```

<b> &lt;xsl:template match=
    "/ARTICLE/SECT/SECT/SECT"&gt;</b>
** &lt;xsl:message terminate="yes"&gt;**
** Error: Sections can only be nested 2 deep.**
** &lt;/xsl:message&gt;**
** &lt;/xsl:template&gt;**

&lt;/xsl:stylesheet&gt;

```

The <tt>terminate="yes"</tt> clause causes the transformation process to stop after the message is generated. Without it, processing could still go on, with everything in that section being ignored.

As an additional exercise, you could expand the stylesheet to handle sections nested up to four sections deep, generating <tt>&lt;h2&gt;...&lt;h5&gt;</tt> tags. Generate an error on any section nested five levels deep.

Finally, finish the stylesheet by adding a template to process the <tt>PARA</tt> tag:

```

** &lt;xsl:template match="PARA"&gt;**
** &lt;p&gt;&lt;xsl:apply-templates/&gt;&lt;/p&gt;**
** &lt;/xsl:template&gt;**
&lt;/xsl:stylesheet&gt;

```

<a name="ggyvn" id="ggyvn"></a>

## Writing the Basic Program

Now you will modify the program that uses XSLT to echo an XML file unchanged, changing it so that it uses your stylesheet.

**Note -** The code discussed in this section is in <tt>Stylizer.java</tt>, which is found in the <tt>xslt</tt> directory after you unzip [XSLT examples](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. The result is <tt>stylizer1a.html</tt>, found in <tt>xslt/data</tt>.

**Note -** The code discussed in this section is in <tt>Stylizer.java</tt>, which is found in the <tt>xslt</tt> directory after you unzip
[`XSLT examples`](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. The result is <tt>stylizer1a.html</tt>, found in <tt>xslt/data</tt>.

The <tt>Stylizer</tt> example is adapted from <tt>TransformationApp02</tt>, which parses an XML file and writes to <tt>System.out</tt>. The main differences between the two programs are described below.

Firstly, <tt>Stylizer</tt> uses the stylesheet when creating the <tt>Transformer</tt> object.

```

// ...
import javax.xml.transform.dom.DOMSource; 
**import javax.xml.transform.stream.StreamSource;** 
import javax.xml.transform.stream.StreamResult; 
// ... 

public class Stylizer {
    // ...
    public static void main (String argv[]) {
        // ...
        try {
            **File stylesheet = new File(argv[0]);**
            **File datafile = new File(argv[1]);**

            DocumentBuilder builder = factory.newDocumentBuilder();
            document = builder.parse(**datafile**);
            // ...
            **StreamSource stylesource = new StreamSource(stylesheet); **
            Transformer transformer = Factory.newTransformer(**stylesource**);
        }
    }
}

```

This code uses the file to create a <tt>StreamSource</tt> object and then passes the source object to the factory class to get the transformer.

**Note -** You can simplify the code somewhat by eliminating the <tt>DOMSource</tt> class. Instead of creating a <tt>DOMSource</tt> object for the XML file, create a <tt>StreamSource</tt> object for it, as well as for the stylesheet.

<a name="ghbeu" id="ghbeu"></a>

### Running the <tt>Stylizer</tt> Sample

<li>**Navigate to the <tt>samples</tt> directory.**
<pre><code>
% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>.
</code></pre>
</li>
<!--
1. **[Download the XSLT examples by clicking this link](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.**
-->
<li><b>
[`Download the XSLT examples by clicking this link`](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.</b></li>
<li>**Navigate to the <tt>xslt</tt> directory.**
<pre><code>
cd xslt
</code></pre>
</li>
<li>**Compile the <tt>Stylizer</tt> sample.**
Type the following command:
<pre><code>
% javac Stylizer.java
</code></pre>
</li>
<li>**Run the <tt>Stylizer</tt> sample on <tt>article1.xml</tt> using the stylesheet <tt>article1a.xsl</tt>.**
<pre><code>
% java Stylizer data/article1a.xsl  data/article1.xml
</code></pre>
You will see the following output:
<pre><code>
&lt;html&gt;
&lt;body&gt;

&lt;h1 align="center"&gt;A Sample Article&lt;/h1&gt;
&lt;h2&gt;The First Major Section

&lt;/h2&gt;
&lt;p&gt;This section will introduce a subsection.&lt;/p&gt;
&lt;h3&gt;The Subsection Heading

&lt;/h3&gt;
&lt;p&gt;This is the text of the subsection.
&lt;/p&gt;

&lt;/body&gt;
&lt;/html&gt;
</code></pre>
At this point, there is quite a bit of excess whitespace in the output. In the next section, you will see how to eliminate most of it.
</li>

<a name="ggyxa" id="ggyxa"></a>

## Trimming the Whitespace

Recall that when you look at the structure of a DOM, there are many text nodes that contain nothing but ignorable whitespace. Most of the excess whitespace in the output comes from these nodes. Fortunately, XSL gives you a way to eliminate them. (For more about the node structure, see 
[XSLT/XPath Data Model](xpath.html#gchlm)).

**Note -** The stylesheet discussed in this section is in <tt>article1b.xsl</tt>, which is found in the <tt>xslt/data</tt> directory after you unzip [XSLT examples](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. The result is <tt>stylizer1b.html</tt>, found in <tt>xslt/data</tt>.

**Note -** The stylesheet discussed in this section is in <tt>article1b.xsl</tt>, which is found in the <tt>xslt/data</tt> directory after you unzip
[`XSLT examples`](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. The result is <tt>stylizer1b.html</tt>, found in <tt>xslt/data</tt>.

To remove some of the excess whitespace, add the following highlighted line to the stylesheet.

```

&lt;xsl:stylesheet ...
   &gt;
&lt;xsl:output method="html"/&gt; 
**&lt;xsl:strip-space elements="SECT"/&gt;**

[...]

```

This instruction tells XSL to remove any text nodes under <tt>SECT</tt> elements that contain nothing but whitespace. Nodes that contain text other than whitespace will not be affected, nor will other kinds of nodes.

<a name="ghbaw" id="ghbaw"></a>

### Running the <tt>Stylizer</tt> Sample with Trimmed Whitespace

<li>**Navigate to the <tt>samples</tt> directory.**
<pre><code>
% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>.
</code></pre>
</li>
<!--
1. **[Download the XSLT examples by clicking this link](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.**
-->
<li><b>
[`Download the XSLT examples by clicking this link`](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.</b></li>
<li>**Navigate to the <tt>xslt</tt> directory.**
<pre><code>
cd xslt
</code></pre>
</li>
<li>**Compile the <tt>Stylizer</tt> sample.**
Type the following command:
<pre><code>
% javac Stylizer.java
</code></pre>
</li>
<li>**Run the <tt>Stylizer</tt> sample on <tt>article1.xml</tt> using the stylesheet <tt>article1b.xsl</tt>.**
<pre><code>
% java Stylizer 
  data/article1b.xsl  
  data/article1.xml
</code></pre>
You will see the following output:
<pre><code>
&lt;html&gt;
&lt;body&gt;

&lt;h1 align="center"&gt;A Sample Article&lt;/h1&gt;

&lt;h2&gt;The First Major Section
   &lt;/h2&gt;
&lt;p&gt;This section will introduce a subsection.&lt;/p&gt;
&lt;h3&gt;The Subsection Heading
      &lt;/h3&gt;
&lt;p&gt;This is the text of the subsection.
      &lt;/p&gt;

&lt;/body&gt;
&lt;/html&gt;
</code></pre>
That is quite an improvement. There are still newline characters and whitespace after the headings, but those come from the way the XML is written:
<pre><code>
&lt;SECT&gt;The First Major Section
____&lt;PARA&gt;This section will introduce a subsection.&lt;/PARA&gt;
^^^^
</code></pre>
Here, you can see that the section heading ends with a newline and indentation space, before the PARA entry starts. That is not a big worry, because the browsers that will process the HTML compress and ignore the excess space routinely. But there is still one more formatting tool at our disposal.
</li>

<a name="ghbbz" id="ghbbz"></a>

## Removing the Last Whitespace

**Note -** The stylesheet discussed in this section is in <tt>article1c.xsl</tt>, which is found in the <tt>xslt/data</tt> directory after you unzip [XSLT examples](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. The result is <tt>stylizer1c.html</tt>, found in <tt>xslt/data</tt>.

**Note -** The stylesheet discussed in this section is in <tt>article1c.xsl</tt>, which is found in the <tt>xslt/data</tt> directory after you unzip
[`XSLT examples`](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory. The result is <tt>stylizer1c.html</tt>, found in <tt>xslt/data</tt>.

That last little bit of whitespace is disposed of by adding the following to the stylesheet:

```

   **&lt;xsl:template match="text()"&gt;**
** &lt;xsl:value-of select="normalize-space()"/&gt;**
** &lt;/xsl:template&gt;**

&lt;/xsl:stylesheet&gt;

```

Running <tt>Stylizer</tt> with this stylesheet will remove all remaining whitespace.

<a name="ghbah" id="ghbah"></a>

### Running the <tt>Stylizer</tt> Sample with All Whitespace Trimmed

<li>**Navigate to the <tt>samples</tt> directory.**
<pre><code>
% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>.
</code></pre>
</li>
<!--
1. **[Download the XSLT examples by clicking this link](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.**
-->
<li><b>
[`Download the XSLT examples by clicking this link`](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.</b></li>
<li>**Navigate to the <tt>xslt</tt> directory.**
<pre><code>
cd xslt
</code></pre>
</li>
<li>**Compile the <tt>Stylizer</tt> sample.**
Type the following command:
<pre><code>
% javac Stylizer.java
</code></pre>
</li>
<li>**Run the <tt>Stylizer</tt> sample on <tt>article1.xml</tt> using the stylesheet <tt>article1c.xsl</tt>.**
<pre><code>
% java Stylizer 
  data/article1c.xsl  
  data/article1.xml
</code></pre>
The output now looks like this:
<pre><code>
&lt;html&gt;
&lt;body&gt;
&lt;h1 align="center"&gt;A Sample Article
&lt;/h1&gt;
&lt;h2&gt;The First Major Section&lt;/h2&gt;
&lt;p&gt;This section will introduce a subsection.
&lt;/p&gt;
&lt;h3&gt;The Subsection Heading&lt;/h3&gt;
&lt;p&gt;This is the text of the subsection.
&lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>
That is quite a bit better. Of course, it would be nicer if it were indented, but that turns out to be somewhat harder than expected. Here are some possible avenues of attack, along with the difficulties:
<dl>
<dt>Indent option</dt>
<dd>
Unfortunately, the <tt>indent="yes"</tt> option that can be applied to XML output is not available for HTML output. Even if that option were available, it would not help, because HTML elements are rarely nested! Although HTML source is frequently indented to show the implied structure, the HTML tags themselves are not nested in a way that creates a real structure.
</dd>
<dt>Indent variables</dt>
<dd>
The <tt>&lt;xsl:text&gt;</tt> function lets you add any text you want, including whitespace. So it could conceivably be used to output indentation space. The problem is to vary the amount of indentation space. XSLT variables seem like a good idea, but they do not work here. The reason is that when you assign a value to a variable in a template, the value is known only within that template (statically, at compile time). Even if the variable is defined globally, the assigned value is not stored in a way that lets it be dynamically known by other templates at runtime. When <tt>&lt;apply-templates/&gt;</tt> invokes other templates, those templates are unaware of any variable settings made elsewhere.
</dd>
<dt>Parameterized templates</dt>
<dd>
Using a parameterized template is another way to modify a template's behavior. But determining the amount of indentation space to pass as the parameter remains the crux of the problem.
</dd>
</dl>
At the moment, then, there does not appear to be any good way to control the indentation of HTML formatted output. That would be inconvenient if you needed to display or edit the HTML as plain text. But it is not a problem if you do your editing on the XML form, using the HTML version only for display in a browser. (When you view <tt>stylizer1c.html</tt>, for example, you see the results you expect).
</li>

<a name="ghbam" id="ghbam"></a>

## Processing the Remaining Structure Elements

In this section, you will process the <tt>LIST</tt> and <tt>NOTE</tt> elements, which add more structure to an article.

**Note -** The sample document described in this section is <tt>article2.xml</tt>, and the stylesheet used to manipulate it is <tt>article2.xsl</tt>. The result is <tt>stylizer2.html</tt>. These files are found in the <tt>xslt/data</tt> directory after you unzip [XSLT examples](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

**Note -** The sample document described in this section is <tt>article2.xml</tt>, and the stylesheet used to manipulate it is <tt>article2.xsl</tt>. The result is <tt>stylizer2.html</tt>. These files are found in the <tt>xslt/data</tt> directory after you unzip
[`XSLT examples`](../examples/xslt_samples.zip) into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.

Start by adding some test data to the sample document:

```

&lt;?xml version="1.0"?&gt;
&lt;ARTICLE&gt;
&lt;TITLE&gt;A Sample Article&lt;/TITLE&gt;
 &lt;SECT&gt;The First Major Section
    ...
  &lt;/SECT&gt;
  &lt;SECT&gt;The Second Major Section
  &lt;PARA&gt;This section adds a LIST and a NOTE.
    &lt;PARA&gt;Here is the LIST:
      &lt;LIST type="ordered"&gt;
        &lt;ITEM&gt;Pears&lt;/ITEM&gt;
        &lt;ITEM&gt;Grapes&lt;/ITEM&gt;
      &lt;/LIST&gt;
  &lt;/PARA&gt;
  &lt;PARA&gt;And here is the NOTE:
  &lt;NOTE&gt;Don't forget to go to the 
           hardware store on your way
           to the grocery!
  &lt;/NOTE&gt;
  &lt;/PARA&gt;
 &lt;/SECT&gt; 
&lt;/ARTICLE&gt;

```

**Note -** Although the <tt>list</tt> and <tt>note</tt> in the XML file are contained in their respective paragraphs, it really makes no difference whether they are contained or not; the generated HTML will be the same either way. But having them contained will make them easier to deal with in an outline-oriented editor.

<a name="ggywf" id="ggywf"></a>

## Modify <tt>&lt;PARA&gt;</tt> Handling

Next, modify the <tt>PARA</tt> template to account for the fact that we are now allowing some of the structure elements to be embedded with a paragraph:

```

&lt;xsl:template match="PARA"&gt;
<b>&lt;p&gt; &lt;xsl:apply-templates select=
    "text()|B|I|U|DEF|LINK"/&gt;</b>
** &lt;/p&gt;**
<b> &lt;xsl:apply-templates select=
    "PARA|LIST|NOTE"/&gt;</b>
&lt;/xsl:template&gt;

```

This modification uses the same technique you used for section headings. The only difference is that <tt>SECT</tt> elements are not expected within a paragraph. (However, a paragraph could easily exist inside another paragraph-for example, as quoted material).

<a name="ggyua" id="ggyua"></a>

## Process <tt>&lt;LIST&gt;</tt> and <tt>&lt;ITEM&gt;</tt> Elements

Now you're ready to add a template to process <tt>LIST</tt> elements:

```

&lt;xsl:template match="LIST"&gt;
 &lt;xsl:if test="@type='ordered'"&gt; 
  &lt;ol&gt;
   &lt;xsl:apply-templates/&gt;
    &lt;/ol&gt;
    &lt;/xsl:if&gt;
    &lt;xsl:if test="@type='unordered'"&gt;
     &lt;ul&gt;
      &lt;xsl:apply-templates/&gt;
     &lt;/ul&gt;
 &lt;/xsl:if&gt;
&lt;/xsl:template&gt;

&lt;/xsl:stylesheet&gt;

```

The <tt>&lt;xsl:if&gt;</tt> tag uses the <tt>test=""</tt> attribute to specify a Boolean condition. In this case, the value of the type attribute is tested, and the list that is generated changes depending on whether the value is ordered or unordered.

Note two important things in this example:

<li>
There is no else clause, nor is there a return or exit statement, so it takes two <tt>&lt;xsl:if&gt;</tt> tags to cover the two options. (Or the <tt>&lt;xsl:choose&gt;</tt> tag could have been used, which provides case-statement functionality).
</li>
<li>
Single quotes are required around the attribute values. Otherwise, the XSLT processor attempts to interpret the word ordered as an XPath function instead of as a string.
</li>

Now finish LIST processing by handling ITEM elements:

```

** &lt;xsl:template match="ITEM"&gt;**
** &lt;li&gt;&lt;xsl:apply-templates/&gt;**
** &lt;/li&gt;**
** &lt;/xsl:template&gt;**

&lt;/xsl:stylesheet&gt;

```

<a name="ggyvs" id="ggyvs"></a>

## Ordering Templates in a Stylesheet

By now, you should have the idea that templates are independent of one another, so it does not generally matter where they occur in a file. So from this point on, we will show only the template you need to add. (For the sake of comparison, they're always added at the end of the example stylesheet).

Order does make a difference when two templates can apply to the same node. In that case, the one that is defined last is the one that is found and processed. For example, to change the ordering of an indented list to use lowercase alphabetics, you could specify a template pattern that looks like this: <tt>//LIST//LIST</tt>. In that template, you would use the HTML option to generate an alphabetic enumeration, instead of a numeric one.

But such an element could also be identified by the pattern <tt>//LIST</tt>. To make sure that the proper processing is done, the template that specifies <tt>//LIST</tt> would have to appear before the template that specifies <tt>//LIST//LIST</tt>.

<a name="ggywm" id="ggywm"></a>

## Process <tt>&lt;NOTE&gt;</tt> Elements

The last remaining structure element is the <tt>NOTE</tt> element. Add the following template to handle that.

```

** &lt;xsl:template match="NOTE"&gt;**
** &lt;blockquote&gt;&lt;b&gt;Note:&lt;/b&gt;&lt;br/&gt;**
** &lt;xsl:apply-templates/&gt;**
** &lt;/p&gt;&lt;/blockquote&gt;**
** &lt;/xsl:template&gt;**

&lt;/xsl:stylesheet&gt;

```

This code brings up an interesting issue that results from the inclusion of the <tt>&lt;br/&gt;</tt> tag. For the file to be well-formed XML, the tag must be specified in the stylesheet as <tt>&lt;br/&gt;</tt>, but that tag is not recognized by many browsers. And although most browsers recognize the sequence <tt>&lt;br&gt;&lt;/br&gt;</tt>, they all treat it like a paragraph break instead of a single line break.

In other words, the transformation must generate a <tt>&lt;br&gt;</tt> tag, but the stylesheet must specify <tt>&lt;br/&gt;</tt>. That brings us to the major reason for that special output tag we added early in the stylesheet:

```

&lt;xsl:stylesheet ... &gt;
   **&lt;xsl:output method="html"/&gt;**
   [...]
&lt;/xsl:stylesheet&gt;

```

That output specification converts empty tags such as <tt>&lt;br/&gt;</tt> to their HTML form, <tt>&lt;br&gt;</tt>, on output. That conversion is important, because most browsers do not recognize the empty tags. Here is a list of the affected tags:

```

area      frame   isindex
base      hr      link
basefont  img     meta
br        input   param
col

```

To summarize, by default XSLT produces well-formed XML on output. And because an XSL stylesheet is well-formed XML to start with, you cannot easily put a tag such as <tt>&lt;br&gt;</tt> in the middle of it. The <tt>&lt;xsl:output method="html"/&gt;</tt> tag solves the problem so that you can code <tt>&lt;br/&gt;</tt> in the stylesheet but get <tt>&lt;br&gt;</tt> in the output.

The other major reason for specifying <tt>&lt;xsl:output method="html"/&gt;</tt> is that, as with the specification <tt>&lt;xsl:output method="text"/&gt;</tt>, generated text is not escaped. For example, if the stylesheet includes the <tt>&lt;</tt> entity reference, it will appear as the <tt>&lt;</tt> character in the generated text. When XML is generated, on the other hand, the <tt>&lt;</tt> entity reference in the stylesheet would be unchanged, so it would appear as <tt>&lt;</tt> in the generated text.

**Note -** If you actually want <tt>&lt;</tt> to be generated as part of the HTML output, you will need to encode it as <tt>&amp;lt;</tt>. That sequence becomes <tt>&lt;</tt> on output, because only the <tt>&amp;</tt> is converted to an <tt>&amp;</tt> character.

<a name="ghbeb" id="ghbeb"></a>

### Running the <tt>Stylizer</tt> Sample With <tt>LIST</tt> and <tt>NOTE</tt> Elements Defined

<li>**Navigate to the <tt>samples</tt> directory.**
<pre><code>
% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>.
</code></pre>
</li>
<!--
1. **[Download the XSLT examples by clicking this link](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.**
-->
<li><b>
[`Download the XSLT examples by clicking this link`](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.</b></li>
<li>**Navigate to the <tt>xslt</tt> directory.**
<pre><code>
cd xslt
</code></pre>
</li>
<li>**Compile the <tt>Stylizer</tt> sample.**
Type the following command:
<pre><code>
% javac Stylizer.java
</code></pre>
</li>
<li>**Run the <tt>Stylizer</tt> sample on <tt>article2.xml</tt> using the stylesheet <tt>article2.xsl</tt>.**
<pre><code>
% java Stylizer data/article2.xsl  data/article2.xml
</code></pre>
Here is the HTML that is generated for the second section when you run the program now:
<pre><code>
...
&lt;h2&gt;The Second Major Section
&lt;/h2&gt;
&lt;p&gt;This section adds a LIST and a NOTE.
&lt;/p&gt;
&lt;p&gt;Here is the LIST:
&lt;/p&gt;
&lt;ol&gt;
&lt;li&gt;Pears&lt;/li&gt;
&lt;li&gt;Grapes&lt;/li&gt;
&lt;/ol&gt;
&lt;p&gt;And here is the NOTE:
&lt;/p&gt;
&lt;blockquote&gt;
&lt;b&gt;Note:&lt;/b&gt;
&lt;br&gt;Do not forget to go to the hardware store on your way to the grocery!
&lt;/blockquote&gt;
</code></pre>
</li>

<a name="ggywl" id="ggywl"></a>

## Process Inline (Content) Elements

The only remaining tags in the <tt>ARTICLE</tt> type are the inline tags-the ones that do not create a line break in the output, but instead are integrated into the stream of text they are part of.

Inline elements are different from structure elements in that inline elements are part of the content of a tag. If you think of an element as a node in a document tree, then each node has both content and structure. The content is composed of the text and inline tags it contains. The structure consists of the other elements (structure elements) under the tag.

**Note -** The sample document described in this section is <tt>article3.xml</tt>, and the stylesheet used to manipulate it is <tt>article3.xsl</tt>. The result is <tt>stylizer3.html</tt>.

Start by adding one more bit of test data to the sample document:

```

&lt;?xml version="1.0"?&gt;
&lt;ARTICLE&gt;
 &lt;TITLE&gt;A Sample Article&lt;/TITLE&gt;
  &lt;SECT&gt;The First Major Section
      [...]
  &lt;/SECT&gt;
  &lt;SECT&gt;The Second Major Section
      [...]
  &lt;/SECT&gt; 
<b>&lt;SECT&gt;The &lt;i&gt;Third&lt;/i&gt; 
    Major Section</b>
<b> &lt;PARA&gt;In addition to the inline tag
    in the heading, </b>
<b> this section defines the term  
    &lt;DEF&gt;inline&lt;/DEF&gt;,</b>
** which literally means "no line break". **
<b> It also adds a simple link to the main page 
    for the Java platform </b>
**(&lt;LINK&gt;http://java.sun.com&lt;/LINK&gt;),**
** as well as a link to the **
<b> &lt;LINK target="http://java.sun.com/xml"&gt;
   XML &lt;/LINK&gt;</b> 
** page.**
** &lt;/PARA&gt;**
** &lt;/SECT&gt;** 
&lt;/ARTICLE&gt;

```

Now process the inline <tt>&lt;DEF&gt;</tt> elements in paragraphs, renaming them to HTML italics tags:

```

**&lt;xsl:template match="DEF"&gt;**
** &lt;i&gt; &lt;xsl:apply-templates/&gt; &lt;/i&gt; **
**&lt;/xsl:template&gt;**

```

Next, comment out the text-node normalization. It has served its purpose, and now you are to the point that you need to preserve important spaces:

```

**&lt;!--**  
&lt;xsl:template match="text()"&gt;
  &lt;xsl:value-of select="normalize-space()"/&gt;
   &lt;/xsl:template&gt;
**--&gt;**

```

This modification keeps us from losing spaces before tags such as <tt>&lt;I&gt;</tt> and <tt>&lt;DEF&gt;</tt>. (Try the program without this modification to see the result).

Now process basic inline HTML elements such as <tt>&lt;B&gt;</tt>, <tt>&lt;I&gt;</tt>, and <tt>&lt;U&gt;</tt> for bold, italics, and underlining.

```

**&lt;xsl:template match="B|I|U"&gt;**
** &lt;xsl:element name="{name()}"&gt;**
** &lt;xsl:apply-templates/&gt;**
** &lt;/xsl:element&gt; **
**&lt;/xsl:template&gt;**

```

The <tt>&lt;xsl:element&gt;</tt> tag lets you compute the element you want to generate. Here, you generate the appropriate inline tag using the name of the current element. In particular, note the use of curly braces (<tt>{}</tt>) in the <tt>name=".."</tt> expression. Those curly braces cause the text inside the quotes to be processed as an XPath expression instead of being interpreted as a literal string. Here, they cause the XPath <tt>name()</tt> function to return the name of the current node.

Curly braces are recognized anywhere that an attribute value template can occur. (Attribute value templates are defined in section 7.6.2 of the XSLT specification, and they appear several places in the template definitions). In such expressions, curly braces can also be used to refer to the value of an attribute, <tt>{@foo}</tt>, or to the content of an element <tt>{foo}</tt>.

**Note -** You can also generate attributes using <tt>&lt;xsl:attribute&gt;</tt>. For more information, see section 7.1.3 of the XSLT Specification.

The last remaining element is the <tt>LINK</tt> tag. The easiest way to process that tag will be to set up a named template that we can drive with a parameter:

```

**&lt;xsl:template name="htmLink"&gt;**
<b> &lt;xsl:param name="dest" 
    select="UNDEFINED"/&gt; </b>
** &lt;xsl:element name="a"&gt;**
** &lt;xsl:attribute name="href"&gt;**
** &lt;xsl:value-of select=""/&gt;**
** &lt;/xsl:attribute&gt;**
** &lt;xsl:apply-templates/&gt; **
** &lt;/xsl:element&gt; **
**&lt;/xsl:template&gt;**

```

The major difference in this template is that, instead of specifying a match clause, you give the template a name using the <tt>name=""</tt> clause. So this template gets executed only when you invoke it.

Within the template, you also specify a parameter named <tt>dest</tt> using the <tt>&lt;xsl:param&gt;</tt> tag. For a bit of error checking, you use the select clause to give that parameter a default value of <tt>UNDEFINED</tt>. To reference the variable in the <tt>&lt;xsl:value-of&gt;</tt> tag, you specify <tt></tt>.

**Note -** Recall that an entry in quotes is interpreted as an expression unless it is further enclosed in single quotes. That is why the single quotes were needed earlier in <tt>"@type='ordered'"</tt> to make sure that ordered was interpreted as a string.

The <tt>&lt;xsl:element&gt;</tt> tag generates an element. Previously, you have been able to simply specify the element we want by coding something like <tt>&lt;html&gt;</tt>. But here you are dynamically generating the content of the HTML anchor (<tt>&lt;a&gt;</tt>) in the body of the <tt>&lt;xsl:element&gt;</tt> tag. And you are dynamically generating the <tt>href</tt> attribute of the anchor using the <tt>&lt;xsl:attribute&gt;</tt> tag.

The last important part of the template is the <tt>&lt;apply-templates&gt;</tt> tag, which inserts the text from the text node under the <tt>LINK</tt> element. Without it, there would be no text in the generated HTML link.

Next, add the template for the <tt>LINK</tt> tag, and call the named template from within it:

```

**&lt;xsl:template match="LINK"&gt;**
** &lt;xsl:if test="@target"&gt;**
** &lt;!--Target attribute specified.--&gt;**
<b> &lt;xsl:call-template 
    name="htmLink"&gt;</b>
<b> &lt;xsl:with-param name="dest" 
    select="@target"/&gt; </b>
** &lt;/xsl:call-template&gt;**
** &lt;/xsl:if&gt;**
**&lt;/xsl:template&gt;**
&lt;xsl:template name="htmLink"&gt;

[...]

```

The <tt>test="@target"</tt> clause returns true if the target attribute exists in the LINK tag. So this <tt>&lt;xsl-if&gt;</tt> tag generates HTML links when the text of the link and the target defined for it are different.

The <tt>&lt;xsl:call-template&gt;</tt> tag invokes the named template, whereas <tt>&lt;xsl:with-param&gt;</tt> specifies a parameter using the name clause and specifies its value using the select clause.

As the very last step in the stylesheet construction process, add the <tt>&lt;xsl-if&gt;</tt> tag to process <tt>LINK</tt> tags that do not have a target attribute.

```

&lt;xsl:template match="LINK"&gt;
   &lt;xsl:if test="@target"&gt;
      [...]
   &lt;/xsl:if&gt;

   **&lt;xsl:if test="not(@target)"&gt;**
** &lt;xsl:call-template name="htmLink"&gt;**
** &lt;xsl:with-param name="dest"&gt;**
** &lt;xsl:apply-templates/&gt;**
** &lt;/xsl:with-param&gt;**
** &lt;/xsl:call-template&gt;**
** &lt;/xsl:if&gt;**
&lt;/xsl:template&gt;

```

The <tt>not(...)</tt> clause inverts the previous test (remember, there is no else clause). So this part of the template is interpreted when the target attribute is not specified. This time, the parameter value comes not from a select clause, but from the contents of the <tt>&lt;xsl:with-param&gt;</tt> element.

**Note -** Just to make it explicit: Parameters and variables (which are discussed in a few moments in [What Else Can XSLT Do?](#ggyut) can have their value specified either by a select clause, which lets you use XPath expressions, or by the content of the element, which lets you use XSLT tags.

In this case, the content of the parameter is generated by the <tt>&lt;xsl:apply-templates/&gt;</tt> tag, which inserts the contents of the text node under the <tt>LINK</tt> element.

<a name="ghbdo" id="ghbdo"></a>

### Running the <tt>Stylizer</tt> Sample With Inline Elements Defined

<li>**Navigate to the <tt>samples</tt> directory.**
<pre><code>
% cd *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt>.
</code></pre>
</li>
<!--
1. **[Download the XSLT examples by clicking this link](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.**
-->
<li><b>
[`Download the XSLT examples by clicking this link`](../examples/xslt_samples.zip) and unzip them into the *install-dir*<tt>/jaxp-1_4_2-</tt>*release-date*<tt>/samples</tt> directory.</b></li>
<li>**Navigate to the <tt>xslt</tt> directory.**&lt;
<pre><code>
cd xslt
</code></pre>
</li>
<li>**Compile the <tt>Stylizer</tt> sample.**
Type the following command:
<pre><code>
% javac Stylizer.java
</code></pre>
</li>
<li>**Run the <tt>Stylizer</tt> sample on <tt>article3.xml</tt> using the stylesheet <tt>article3.xsl</tt>.**
<pre><code>
% java Stylizer data/article3.xsl  data/article3.xml
</code></pre>
When you run the program now, the results should look something like this:
<pre><code>
[...]
&lt;h2&gt;The &lt;i&gt;Third&lt;/i&gt; Major Section&lt;/h2&gt;
&lt;p&gt;In addition to the inline tag in the heading, this section  defines the term &lt;i&gt;inline&lt;/i&gt;, which literally means "no line break". It also adds a simple link to the main page for the Java platform (&lt;a href="http://java.sun.com"&gt;http://java.sun.com&lt;/a&gt;),  as well as a link to the &lt;a href="http://java.sun.com/xml"&gt;XML&lt;/a&gt; page.&lt;/p&gt;
</code></pre>
Good work! You have now converted a rather complex XML file to HTML. (As simple as it appears at first, it certainly provides a lot of opportunity for exploration).
</li>

<a name="ggywy" id="ggywy"></a>

## Printing the HTML

You have now converted an XML file to HTML. One day, someone will produce an HTML-aware printing engine that you will be able to find and use through the Java Printing Service API. At that point, you will have ability to print an arbitrary XML file by generating HTML. All you will have to do is to set up a stylesheet and use your browser.

<a name="ggyut" id="ggyut"></a>

## What Else Can XSLT Do?

As lengthy as this section has been, it has only scratched the surface of XSLT's capabilities. Many additional possibilities await you in the XSLT specification. Here are a few things to look for:

<tt>rt</tt> (Section 2.6.2) and include (section 2.6.1) Use these statements to modularize and combine XSLT stylesheets. The include statement simply inserts any definitions from the included file. The import statement lets you override definitions in the imported file with definitions in your own stylesheet.

Loop over a collection of items and process each one in turn.

Branch to one of multiple processing paths depending on an input value.

Dynamically generate numbered sections, numbered elements, and numeric literals. XSLT provides three numbering modes:

<li>
Single: Numbers items under a single heading, like an ordered list in HTML
</li>
<li>
Multiple: Produces multilevel numbering such as "A.1.3"
</li>
<li>
Any: Consecutively numbers items wherever they appear, as with footnotes in a lesson.
</li>

Control enumeration formatting so that you get numerics (<tt>format="1"</tt>), uppercase alphabetics (<tt>format="A"</tt>), lowercase alphabetics (<tt>format="a"</tt>), or compound numbers, like "A.1," as well as numbers and currency amounts suited for a specific international locale.

Produce output in a desired sorting order.

Process an element multiple times, each time in a different "mode." You add a mode attribute to templates and then specify <tt>&lt;apply-templates mode="..."&gt;</tt> to apply only the templates with a matching mode. Combine with the <tt>&lt;apply-templates select="..."&gt;</tt> attribute to apply mode-based processing to a subset of the input data.

Variables are something like method parameters, in that they let you control a template's behavior. But they are not as valuable as you might think. The value of a variable is known only within the scope of the current template or <tt>&lt;xsl:if&gt;</tt> tag (for example) in which it is defined. You cannot pass a value from one template to another, or even from an enclosed part of a template to another part of the same template.

These statements are true even for a "global" variable. You can change its value in a template, but the change applies only to that template. And when the expression used to define the global variable is evaluated, that evaluation takes place in the context of the structure's root node. In other words, global variables are essentially runtime constants. Those constants can be useful for changing the behavior of a template, especially when coupled with include and import statements. But variables are not a general-purpose data-management mechanism.

<a name="ggyuy" id="ggyuy"></a>

## The Trouble with Variables

It is tempting to create a single template and set a variable for the destination of the link, rather than go to the trouble of setting up a parameterized template and calling it two different ways. The idea is to set the variable to a default value (say, the text of the <tt>LINK</tt> tag) and then, if the target attribute exists, set the destination variable to the value of the target attribute.

That would be a good idea-if it worked. But again, the issue is that variables are known only in the scope within which they are defined. So when you code an <tt>&lt;xsl:if&gt;</tt> tag to change the value of the variable, the value is known only within the context of the <tt>&lt;xsl:if&gt;</tt> tag. Once <tt>&lt;/xsl:if&gt;</tt> is encountered, any change to the variable's setting is lost.

A similarly tempting idea is the possibility of replacing the <tt>text()|B|I|U|DEF|LINK</tt> specification with a variable (<tt></tt>). But because the value of the variable is determined by where it is defined, the value of a global inline variable consists of text nodes, <tt>&lt;B&gt;</tt> nodes, and so on, that happen to exist at the root level. In other words, the value of such a variable, in this case, is null.
