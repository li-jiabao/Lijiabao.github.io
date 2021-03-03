---
title: Extracting the Contents of a JAR File
author: lijiabao
date: 2020-12-06 12:45:25.139297700 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Extracting the Contents of a JAR File

The basic command to use for extracting the contents of a JAR file is:

```

jar xf *jar-file [archived-file(s)]*

```

Let's look at the options and arguments in this command:

- The <tt>x</tt> option indicates that you want to **extract** files from the JAR archive.
- The <tt>f</tt> options indicates that the JAR **file** from which files are to be extracted is specified on the command line, rather than through stdin.
- The <tt>jar-file</tt> argument is the filename (or path and filename) of the JAR file from which to extract files.
- <tt>archived-file(s)</tt> is an optional argument consisting of a space-separated list of the files to be extracted from the archive. If this argument is not present, the Jar tool will extract all the files in the archive.

As usual, the order in which the <tt>x</tt> and <tt>f</tt> options appear in the command doesn't matter, but there must not be a space between them.

When extracting files, the Jar tool makes copies of the desired files and writes them to the current directory, reproducing the directory structure that the files have in the archive. The original JAR file remains unchanged.

## An Example

Let's extract some files from the TicTacToe JAR file we've been using in previous sections. Recall that the contents of <tt>TicTacToe.jar</tt> are:

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

Suppose you want to extract the <tt>TicTacToe</tt> class file and the <tt>cross.gif</tt> image file. To do so, you can use this command:

```

jar xf TicTacToe.jar TicTacToe.class images/cross.gif

```

This command does two things:

- It places a copy of <tt>TicTacToe.class</tt> in the current directory.
- It creates the directory <tt>images</tt>, if it doesn't already exist, and places a copy of <tt>cross.gif</tt> within it.

The original TicTacToe JAR file remains unchanged.

As many files as desired can be extracted from the JAR file in the same way. When the command doesn't specify which files to extract, the Jar tool extracts all files in the archive. For example, you can extract all the files in the TicTacToe archive by using this command:

```

jar xf TicTacToe.jar

```
