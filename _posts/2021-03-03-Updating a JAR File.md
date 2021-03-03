---
title: Updating a JAR File
author: lijiabao
date: 2020-12-06 12:45:27.537929300 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Updating a JAR File

The Jar tool provides a <tt>u</tt> option which you can use to update the contents of an existing JAR file by modifying its manifest or by adding files.

The basic command for adding files has this format:

```

jar uf *jar-file input-file(s)*

```

In this command:

- The <tt>u</tt> option indicates that you want to **update** an existing JAR file.
- The <tt>f</tt> option indicates that the JAR file to update is specified on the command line.
- <tt>jar-file</tt> is the existing JAR file that is to be updated.
- <tt>input-file(s)</tt> is a space-delimited list of one or more files that you want to add to the JAR file.

Any files already in the archive having the same pathname as a file being added will be overwritten.

When creating a new JAR file, you can optionally use the <tt>-C</tt> option to indicate a change of directory. For more information, see the 
[Creating a JAR File](build.html) section.

## Examples

Recall that <tt>TicTacToe.jar</tt> has these contents:

```

META-INF/MANIFEST.MF
TicTacToe.class
TicTacToe.class
TicTacToe.java
audio/
audio/beep.au
audio/ding.au
audio/return.au
audio/yahoo1.au
audio/yahoo2.au
example1.html
images/
images/cross.gif
images/not.gif

```

Suppose that you want to add the file <tt>images/new.gif</tt> to the JAR file. You could accomplish that by issuing this command from the parent directory of the <tt>images</tt> directory:

```

jar uf TicTacToe.jar images/new.gif

```

The revised JAR file would have this table of contents:

```

META-INF/MANIFEST.MF
TicTacToe.class
TicTacToe.class
TicTacToe.java
audio/
audio/beep.au
audio/ding.au
audio/return.au
audio/yahoo1.au
audio/yahoo2.au
example1.html
images/
images/cross.gif
images/not.gif
images/new.gif

```

You can use the <tt>-C</tt> option to "change directories" during execution of the command. For example:

```

jar uf TicTacToe.jar -C images new.gif

```

This command would change to the <tt>images</tt> directory before adding <tt>new.gif</tt> to the JAR file. The <tt>images</tt> directory would not be included in the pathname of <tt>new.gif</tt> when it's added to the archive, resulting in a table of contents that looks like this:

```

META-INF/MANIFEST.MF
META-INF/MANIFEST.MF
TicTacToe.class
TicTacToe.class
TicTacToe.java
audio/
audio/beep.au
audio/ding.au
audio/return.au
audio/yahoo1.au
audio/yahoo2.au
example1.html
images/
images/cross.gif
images/not.gif
new.gif

```
