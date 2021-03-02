
# Using the <tt>DTDHandler</tt> and <tt>EntityResolver</tt>

This section presents the two remaining SAX event handlers: <tt>DTDHandler</tt> and <tt>EntityResolver</tt>. The <tt>DTDHandler</tt> is invoked when the DTD encounters an unparsed entity or a notation declaration. The <tt>EntityResolver</tt> comes into play when a URN (public ID) must be resolved to a URL (system ID).

<a name="gcylk" id="gcylk"></a>

## The <tt>DTDHandler</tt> API


[Choosing the Parser Implementation](validation.html) showed a method for referencing a file that contains binary data, such as an image file, using MIME data types. That is the simplest, most extensible mechanism. For compatibility with older SGML-style data, though, it is also possible to define an unparsed entity.

The <tt>NDATA</tt> keyword defines an unparsed entity:

`&lt;!ENTITY myEntity SYSTEM "..URL.." NDATA gif&gt;`

The <tt>NDATA</tt> keyword says that the data in this entity is not parseable XML data but instead is data that uses some other notation. In this case, the notation is named <tt>gif</tt>. The DTD must then include a declaration for that notation, which would look something like the following.

`&lt;!NOTATION gif SYSTEM "..URL.."&gt;`

When the parser sees an unparsed entity or a notation declaration, it does nothing with the information except to pass it along to the application using the <tt>DTDHandler</tt> interface. That interface defines two methods.

<li>
<tt>notationDecl(String name, String publicId, String systemId)</tt>
</li>
<li>
<tt>unparsedEntityDecl(String name, String publicId, String systemId, String notationName</tt>
</li>

The <tt>notationDecl</tt> method is passed the name of the notation and either the public or the system identifier, or both, depending on which is declared in the DTD. The <tt>unparsedEntityDecl</tt> method is passed the name of the entity, the appropriate identifiers, and the name of the notation it uses.

**Note -** The <tt>DTDHandler</tt> interface is implemented by the <tt>DefaultHandler</tt> class.

Notations can also be used in attribute declarations. For example, the following declaration requires notations for the GIF and PNG image-file formats.

```

&lt;!ENTITY image EMPTY&gt;
&lt;!ATTLIST image ...  type  NOTATION  (gif | png) "gif"&gt;

```

Here, the type is declared as being either gif or png. The default, if neither is specified, is gif.

Whether the notation reference is used to describe an unparsed entity or an attribute, it is up to the application to do the appropriate processing. The parser knows nothing at all about the semantics of the notations. It only passes on the declarations.

<a name="gcymm" id="gcymm"></a>

## The <tt>EntityResolver</tt> API

The <tt>EntityResolver</tt> API lets you convert a public ID (URN) into a system ID (URL). Your application may need to do that, for example, to convert something like <tt>href="urn:/someName"</tt> into <tt>"http://someURL"</tt>.

The <tt>EntityResolver</tt> interface defines a single method:

`resolveEntity(String publicId, String systemId)`

This method returns an <tt>InputSource</tt> object, which can be used to access the entity's contents. Converting a URL into an <tt>InputSource</tt> is easy enough. But the URL that is passed as the system ID will be the location of the original document which is, as likely as not, somewhere out on the web. To access a local copy, if there is one, you must maintain a catalog somewhere on the system that maps names (public IDs) into local URLs.
