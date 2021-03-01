
# How XPath Works

The XPath specification is the foundation for a variety of specifications, including XSLT and linking/addressing specifications such as <tt>XPointer</tt>. So an understanding of XPath is fundamental to a lot of advanced XML usage. This section provides an introduction to XPath in the context of XSLT.<a name="gchmd" id="gchmd"></a>

## XPath Expressions

In general, an XPath expression specifies a pattern that selects a set of XML nodes. XSLT templates then use those patterns when applying transformations. (<tt>XPointer</tt>, on the other hand, adds mechanisms for defining a **point** or a **range** so that XPath expressions can be used for addressing).

The nodes in an XPath expression refer to more than just elements. They also refer to text and attributes, among other things. In fact, the XPath specification defines an abstract document model that defines seven kinds of nodes:

<li>
Root
</li>
<li>
Element
</li>
<li>
Text
</li>
<li>
Attribute
</li>
<li>
Comment
</li>
<li>
Processing instruction
</li>
<li>
Namespace
</li>

The root element of the XML data is modeled by an **element** node. The XPath root node contains the document's root element as well as other information relating to the document.

<a name="gchlm" id="gchlm"></a>

## XSLT/XPath Data Model

Like the Document Object Model (DOM), the XSLT/XPath data model consists of a tree containing a variety of nodes. Under any given element node, there are text nodes, attribute nodes, element nodes, comment nodes, and processing instruction nodes.

In this abstract model, syntactic distinctions disappear, and you are left with a normalized view of the data. In a text node, for example, it makes no difference whether the text was defined in a CDATA section or whether it included entity references. The text node will consist of normalized data, as it exists after all parsing is complete. So the text will contain a <tt>&lt;</tt> character, whether or not an entity reference such as <tt>&lt;</tt> or a CDATA section was used to include it. (Similarly, the text will contain an <tt>&amp;</tt> character, whether it was delivered using <tt>&amp;</tt> or it was in a CDATA section).

In this section, we will deal mostly with element nodes and text nodes. For the other addressing mechanisms, see the XPath specification.

<a name="gchkv" id="gchkv"></a>

## Templates and Contexts

An XSLT template is a set of formatting instructions that apply to the nodes selected by an XPath expression. In a stylesheet, an XSLT template would look something like this:

```

&lt;xsl:template match="//LIST"&gt;
    ...
&lt;/xsl:template&gt;

```

The expression <tt>//LIST</tt> selects the set of <tt>LIST</tt> nodes from the input stream. Additional instructions within the template tell the system what to do with them.

The set of nodes selected by such an expression defines the context in which other expressions in the template are evaluated. That context can be considered as the whole set - for example, when determining the number of the nodes it contains.

The context can also be considered as a single member of the set, as each member is processed one by one. For example, inside the <tt>LIST</tt>-processing template, the expression <tt>@type</tt> refers to the type attribute of the current <tt>LIST</tt> node. (Similarly, the expression <tt>@*</tt> refers to all the attributes for the current LIST element).

<a name="gchlo" id="gchlo"></a>

## Basic XPath Addressing

An XML document is a tree-structured (hierarchical) collection of nodes. As with a hierarchical directory structure, it is useful to specify a *path* that points to a particular node in the hierarchy (hence the name of the specification: XPath). In fact, much of the notation of directory paths is carried over intact:

<li>
The forward slash (/) is used as a path separator.
</li>
<li>
An absolute path from the root of the document starts with a /.
</li>
<li>
A relative path from a given location starts with anything else.
</li>
<li>
A double period (..) indicates the parent of the current node.
</li>
<li>
A single period (.) indicates the current node.
</li>

For example, in an Extensible HTML (XHTML) document (an XML document that looks like HTML but is well formed according to XML rules), the path <tt>/h1/h2/</tt> would indicate an <tt>h2</tt> element under an <tt>h1</tt>. (Recall that in XML, element names are case-sensitive, so this kind of specification works much better in XHTML than it would in plain HTML, because HTML is case-insensitive).

In a pattern-matching specification such as XPath, the specification <tt>/h1/h2</tt> selects all <tt>h2</tt> elements that lie under an <tt>h1</tt> element. To select a specific <tt>h2</tt> element, you use square brackets <tt>[]</tt> for indexing (like those used for arrays). The path <tt>/h1[4]/h2[5]</tt> would therefore select the fifth <tt>h2</tt> element under the fourth <tt>h1</tt> element.

**Note -** In XHTML, all element names are in lowercase. That is a fairly common convention for XML documents. However, uppercase names are easier to read in a tutorial like this one. So for the remainder of the XSLT lesson, all XML element names will be in uppercase. (Attribute names, on the other hand, will remain in lowercase).

A name specified in an XPath expression refers to an element. For example, <tt>h1</tt> in <tt>/h1/h2</tt> refers to an <tt>h1</tt> element. To refer to an attribute, you prefix the attribute name with an <tt>@</tt> sign. For example, <tt>@type</tt> refers to the type attribute of an element. Assuming that you have an XML document with LIST elements, for example, the expression <tt>LIST/@type</tt> selects the type attribute of the <tt>LIST</tt> element.

**Note -** Because the expression does not begin with <tt>/</tt>, the reference specifies a list node relative to the current context-whatever position in the document that happens to be.

<a name="gchly" id="gchly"></a>

## Basic XPath Expressions

The full range of XPath expressions takes advantage of the wild cards, operators, and functions that XPath defines. You will learn more about those shortly. Here, we look at a couple of the most common XPath expressions simply to introduce them.

The expression <tt>@type="unordered"</tt> specifies an attribute named <tt>type</tt> whose value is <tt>unordered</tt>. An expression such as <tt>LIST/@type</tt> specifies the type attribute of a <tt>LIST</tt> element.

You can combine those two notations to get something interesting. In XPath, the square-bracket notation (<tt>[]</tt>) normally associated with indexing is extended to specify selection criteria. So the expression <tt>LIST[@type="unordered"]</tt> selects all <tt>LIST</tt> elements whose type value is unordered.

Similar expressions exist for elements. Each element has an associated string-value, which is formed by concatenating all the text segments that lie under the element. (A more detailed explanation of how that process works is presented in [String-Value of an Element](#gchmn)).

Suppose you model what is going on in your organization using an XML structure that consists of <tt>PROJECT</tt> elements and <tt>ACTIVITY</tt> elements that have a text string with the project name, multiple <tt>PERSON</tt> elements to list the people involved and, optionally, a <tt>STATUS</tt> element that records the project status. Here are other examples that use the extended square-bracket notation:

<li>
<tt>/PROJECT[.="MyProject"]</tt>: Selects a <tt>PROJECT</tt> named <tt>"MyProject"</tt>.
</li>
<li>
<tt>/PROJECT[STATUS]</tt>: Selects all projects that have a <tt>STATUS</tt> child element.
</li>
<li>
<tt>/PROJECT[STATUS="Critical"]</tt>: Selects all projects that have a <tt>STATUS</tt> child element with the string-value <tt>Critical</tt>.
</li>

<a name="gchml" id="gchml"></a>

## Combining Index Addresses

The XPath specification defines quite a few addressing mechanisms, and they can be combined in many different ways. As a result, XPath delivers a lot of expressive power for a relatively simple specification. This section illustrates other interesting combinations:

<li>
<tt>LIST[@type="ordered"][3]</tt>: Selects all <tt>LIST</tt> elements of the type <tt>ordered</tt>, and returns the third.
</li>
<li>
<tt>LIST[3][@type="ordered"]</tt>: Selects the third <tt>LIST</tt> element, but only if it is of the type <tt>ordered</tt>.
</li>

**Note -** Many more combinations of address operators are listed in section 2.5 of the 
[XPath specification](http://www.w3.org/TR/xpath/). This is arguably the most useful section of the specification for defining an XSLT transform.

<a name="gchlg" id="gchlg"></a>

## Wild Cards

By definition, an unqualified XPath expression selects a set of XML nodes that matches that specified pattern. For example, <tt>/HEAD</tt> matches all top-level <tt>HEAD</tt> entries, whereas <tt>/HEAD[1]</tt> matches only the first. [Table&#160;4-1](#gchlc) lists the wild cards that can be used in XPath expressions to broaden the scope of the pattern matching.

<a name="gchlc" id="gchlc"></a>

Table&#160;4-1 XPath Wild Cards
<th id="h1" align="left" valign="top">Wild card</th><th id="h2" align="left" valign="top">Meaning</th>

Meaning
<td headers="h1" align="left" valign="top"><tt>*</tt></td><td headers="h2" align="left" valign="top">Matches any element node (not attributes or text).</td>

Matches any element node (not attributes or text).
<td headers="h1" align="left" valign="top"><tt>node()</tt></td><td headers="h2" align="left" valign="top">Matches any node of any kind: element node, text node, attribute node, processing instruction node, namespace node, or comment node.</td>

Matches any node of any kind: element node, text node, attribute node, processing instruction node, namespace node, or comment node.
<td headers="h1" align="left" valign="top"><tt>@*</tt></td><td headers="h2" align="left" valign="top">Matches any attribute node.</td>

Matches any attribute node.

In the project database example, <tt>/*/PERSON[.="Fred"]</tt> matches any <tt>PROJECT</tt> or <tt>ACTIVITY</tt> element that names Fred.

<a name="gchnx" id="gchnx"></a>

## Extended-Path Addressing

So far, all the patterns you have seen have specified an exact number of levels in the hierarchy. For example, <tt>/HEAD</tt> specifies any <tt>HEAD</tt> element at the first level in the hierarchy, whereas <tt>/*/*</tt> specifies any element at the second level in the hierarchy. To specify an indeterminate level in the hierarchy, use a double forward slash (<tt>//</tt>). For example, the XPath expression <tt>//PARA</tt> selects all paragraph elements in a document, wherever they may be found.

The <tt>//</tt> pattern can also be used within a path. So the expression <tt>/HEAD/LIST//PARA</tt> indicates all paragraph elements in a subtree that begins from /<tt>HEAD/LIST</tt>.

<a name="gchmu" id="gchmu"></a>

## XPath Data Types and Operators

XPath expressions yield either a set of nodes, a string, a Boolean (a true/false value), or a number. [Table&#160;4-2](#gchmt) lists the operators that can be used in an Xpath expression:

<a name="gchmt" id="gchmt"></a>

Table&#160;4-2 XPath Operators
<th id="h101" align="left" valign="top">Operator</th><th id="h102" align="left" valign="top">Meaning</th>

Meaning
<td headers="h101" align="left" valign="top"><tt>|</tt></td><td headers="h102" align="left" valign="top">Alternative. For example, <tt>PARA|LIST</tt> selects all <tt>PARA</tt> and <tt>LIST</tt> elements.</td>

Alternative. For example, <tt>PARA|LIST</tt> selects all <tt>PARA</tt> and <tt>LIST</tt> elements.
<td headers="h101" align="left" valign="top"><tt>or</tt>, <tt>and</tt></td><td headers="h102" align="left" valign="top">Returns the or/and of two Boolean values.</td>

Returns the or/and of two Boolean values.
<td headers="h101" align="left" valign="top"><tt>=</tt>, <tt>!=</tt></td><td headers="h102" align="left" valign="top">Equal or not equal, for Booleans, strings, and numbers.</td>

Equal or not equal, for Booleans, strings, and numbers.
<td headers="h101" align="left" valign="top"><tt>&lt;</tt>, <tt>&gt;</tt>, <tt>&lt;=</tt>, <tt>&gt;=</tt></td><td headers="h102" align="left" valign="top">Less than, greater than, less than or equal to, greater than or equal to, for numbers.</td>

Less than, greater than, less than or equal to, greater than or equal to, for numbers.
<td headers="h101" align="left" valign="top"><tt>+</tt>, <tt>-</tt>, <tt>*</tt>, <tt>div</tt>, <tt>mod</tt></td><td headers="h102" align="left" valign="top">Add, subtract, multiply, floating-point divide, and modulus (remainder) operations (e.g., 6 mod 4 = 2).</td>

Add, subtract, multiply, floating-point divide, and modulus (remainder) operations (e.g., 6 mod 4 = 2).

Expressions can be grouped in parentheses, so you do not have to worry about operator precedence.

**Note -** Operator precedence is a term that answers the question, "If you specify a + b * c, does that mean (a+b) * c or a + (b*c)?" (The operator precedence is roughly the same as that shown in the table).

<a name="gchmn" id="gchmn"></a>

## String-Value of an Element

The string-value of an element is the concatenation of all descendent text nodes, no matter how deep. Consider this mixed-content XML data:

```

&lt;PARA&gt;This paragraph contains a &lt;b&gt;bold&lt;/b&gt; word&lt;/PARA&gt;

```

The string-value of the <tt>&lt;PARA&gt;</tt> element is **This paragraph contains a bold word**. In particular, note that <tt>&lt;B&gt;</tt> is a child of <tt>&lt;PARA&gt;</tt> and that the text <tt>bold</tt> is a child of <tt>&lt;B&gt;</tt>.

The point is that all the text in all children of a node joins in the concatenation to form the string-value.

Also, it is worth understanding that the text in the abstract data model defined by XPath is fully normalized. So whether the XML structure contains the entity reference <tt>&amp;lt;</tt> or <tt>&lt;</tt> in a <tt>CDATA</tt> section, the element's string-value will contain the <tt>&lt;</tt> character. Therefore, when generating HTML or XML with an XSLT stylesheet, you must convert occurrences of <tt>&lt;</tt> to <tt>&amp;lt;</tt> or enclose them in a <tt>CDATA</tt> section. Similarly, occurrences of <tt>&amp;</tt> must be converted to <tt>&amp;amp;</tt>.

<a name="gchnl" id="gchnl"></a>

## XPath Functions

This section ends with an overview of the XPath functions. You can use XPath functions to select a collection of nodes in the same way that you would use an element specification such as those you have already seen. Other functions return a string, a number, or a Boolean value. For example, the expression <tt>/PROJECT/text()</tt> gets the string-value of <tt>PROJECT</tt> nodes.

Many functions depend on the current context. In the preceding example, the context for each invocation of the <tt>text()</tt> function is the <tt>PROJECT</tt> node that is currently selected.

There are many XPath functions - too many to describe in detail here. This section provides a brief listing that shows the available XPath functions, along with a summary of what they do. For more information about functions, see section 4 of the 
[XPath specification](http://www.w3.org/TR/xpath/).

<a name="gchoa" id="gchoa"></a>

### Node-Set Functions

Many XPath expressions select a set of nodes. In essence, they return a node-set. One function does that, too. The <tt>id(...)</tt> function returns the node with the specified ID. (Elements have an ID only when the document has a DTD, which specifies which attribute has the ID type).

<a name="gchob" id="gchob"></a>

### Positional Functions

These functions return positionally based numeric values.

<li>
<tt>last()</tt>: Returns the index of the last element. For example, <tt>/HEAD[last()]</tt> selects the last <tt>HEAD</tt> element.
</li>
<li>
<tt>position()</tt>: Returns the index position. For example, <tt>/HEAD[position() &lt;= 5]</tt> selects the first five <tt>HEAD</tt> elements.
</li>
<li>
<tt>count(...)</tt>: Returns the count of elements. For example, <tt>/HEAD[count(HEAD)=0]</tt> selects all <tt>HEAD</tt> elements that have no subheads.
</li>

<a name="gchmp" id="gchmp"></a>

### String Functions

These functions operate on or return strings.

<li>
<tt>concat(string, string, ...)</tt>: Concatenates the string values.
</li>
<li>
<tt>starts-with(string1, string2)</tt>: Returns true if <tt>string1</tt> starts with <tt>string2</tt>.
</li>
<li>
<tt>contains(string1, string2)</tt>: Returns true if <tt>string1</tt> contains <tt>string2</tt>.
</li>
<li>
<tt>substring-before(string1, string2)</tt>: Returns the start of <tt>string1</tt> before <tt>string2</tt> occurs in it.
</li>
<li>
<tt>substring-after(string1, string2)</tt>: Returns the remainder of <tt>string1</tt> after <tt>string2</tt> occurs in it.
</li>
<li>
<tt>substring(string, idx)</tt>: Returns the substring from the index position to the end, where the index of the first <tt>char</tt> = 1.
</li>
<li>
<tt>substring(string, idx, len)</tt>: Returns the substring of the specified length from the index position.
</li>
<li>
<tt>string-length()</tt>: Returns the size of the context node's string-value; the context node is the currently selected node-the node that was selected by an XPath expression in which a function such as <tt>string-length()</tt> is applied.
</li>
<li>
<tt>string-length(string)</tt>: Returns the size of the specified string.
</li>
<li>
<tt>normalize-space()</tt>: Returns the normalized string-value of the current node (no leading or trailing white space, and sequences of white space characters converted to a single space).
</li>
<li>
<tt>normalize-space(string)</tt>: Returns the normalized string-value of the specified string.
</li>
<li>
<tt>translate(string1, string2, string3)</tt>: Converts <tt>string1</tt>, replacing occurrences of characters in <tt>string2</tt> with the corresponding character from <tt>string3</tt>.
</li>

**Note -** XPath defines three ways to get the text of an element: <tt>text()</tt>, <tt>string(object)</tt>, and the string-value implied by an element name in an expression like this: <tt>/PROJECT[PERSON="Fred"]</tt>.

<a name="gchny" id="gchny"></a>

### Boolean Functions

These functions operate on or return Boolean values.

<li>
<tt>not(...)</tt>: Negates the specified Boolean value.
</li>
<li>
<tt>true()</tt>: Returns true.
</li>
<li>
<tt>false()</tt>: Returns false.
</li>
<li>
<tt>lang(string)</tt>: Returns true if the language of the context node (specified by <tt>xml:Lang</tt> attributes) is the same as (or a sub-language of) the specified language; for example, Lang("en") is true for <tt>&lt;PARA_xml:Lang="en"&gt;...&lt;/PARA&gt;</tt>.
</li>

<a name="gchta" id="gchta"></a>

### Numeric Functions

These functions operate on or return numeric values.

<li>
<tt>sum(...)</tt>: Returns the sum of the numeric value of each node in the specified node-set.
</li>
<li>
<tt>floor(N)</tt>: Returns the largest integer that is not greater than *N*.
</li>
<li>
<tt>ceiling(N)</tt>: Returns the smallest integer that is not less than *N*.
</li>
<li>
<tt>round(N)</tt>: Returns the integer that is closest to *N*.
</li>

<a name="gchsp" id="gchsp"></a>

### Conversion Functions

These functions convert one data type to another.

<li>
<tt>string(...)</tt>: Returns the string value of a number, Boolean, or node-set.
</li>
<li>
<tt>boolean(...)</tt>: Returns a Boolean value for a number, string, or node-set (a non-zero number, a non-empty node-set, and a non-empty string are all true).
</li>
<li>
<tt>number(...)</tt>: Returns the numeric value of a Boolean, string, or node-set (true is 1, false is 0, a string containing a number becomes that number, the string-value of a node-set is converted to a number).
</li>

<a name="gchtb" id="gchtb"></a>

### Namespace Functions

These functions let you determine the namespace characteristics of a node.

<li>
<tt>local-name()</tt>: Returns the name of the current node, minus the namespace prefix.
</li>
<li>
<tt>local-name(...)</tt>: Returns the name of the first node in the specified node set, minus the namespace prefix.
</li>
<li>
<tt>namespace-uri()</tt>: Returns the namespace URI from the current node.
</li>
<li>
<tt>namespace-uri(...)</tt>: Returns the namespace URI from the first node in the specified node-set.
</li>
<li>
<tt>name()</tt>: Returns the expanded name (URI plus local name) of the current node.
</li>
<li>
<tt>name(...)</tt>: Returns the expanded name (URI plus local name) of the first node in the specified node-set.
</li>

<a name="gchsu" id="gchsu"></a>

## Summary

XPath operators, functions, wild cards, and node-addressing mechanisms can be combined in wide variety of ways. The introduction you have had so far should give you a good head start at specifying the pattern you need for any particular purpose.
