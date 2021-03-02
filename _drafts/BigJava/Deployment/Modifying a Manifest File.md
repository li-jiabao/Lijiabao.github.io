
# Modifying a Manifest File

You use the <tt>m</tt> command-line option to add custom information to the manifest during creation of a JAR file. This section describes the <tt>m</tt> option.

The Jar tool automatically puts a [default manifest](defman.html) with the pathname <tt>META-INF/MANIFEST.MF</tt> into any JAR file you create. You can enable special JAR file functionality, such as [package sealing](sealman.html), by modifying the default manifest. Typically, modifying the default manifest involves adding special-purpose *headers* to the manifest that allow the JAR file to perform a particular desired function.

To modify the manifest, you must first prepare a text file containing the information you wish to add to the manifest. You then use the Jar tool's <tt>m</tt> option to add the information in your file to the manifest.

The basic command has this format:

```

jar cfm *jar-file manifest-addition input-file(s)*

```

Let's look at the options and arguments used in this command:

- The <tt>c</tt> option indicates that you want to **create** a JAR file.
- The <tt>m</tt> option indicates that you want to merge information from an existing file into the manifest file of the JAR file you're creating.
- The <tt>f</tt> option indicates that you want the output to go to a **file** (the JAR file you're creating) rather than to standard output.
- **<tt>manifest-addition</tt>** is the name (or path and name) of the existing text file whose contents you want to add to the contents of JAR file's manifest.
- **<tt>jar-file</tt>** is the name that you want the resulting JAR file to have.
- The **<tt>input-file(s)</tt>** argument is a space-separated list of one or more files that you want to be placed in your JAR file.

The <tt>m</tt> and <tt>f</tt> options must be in the same order as the corresponding arguments.

The remaining sections of this lesson demonstrate specific modifications you may want to make to the manifest file.
