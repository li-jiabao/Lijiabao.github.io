---
title: Viewing the Contents of a JAR File
author: lijiabao
date: 2020-12-06 12:45:22.739610200 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Viewing the Contents of a JAR File

The basic format of the command for viewing the contents of a JAR file is:

```

jar tf *jar-file*

```

Let's look at the options and argument used in this command:

- The <tt>t</tt> option indicates that you want to view the **table** of contents of the JAR file.
- The <tt>f</tt> option indicates that the JAR file whose contents are to be viewed is specified on the command line.
- The <tt>jar-file</tt> argument is the path and name of the JAR file whose contents you want to view.

The <tt>t</tt> and <tt>f</tt> options can appear in either order, but there must not be any space between them.

This command will display the JAR file's table of contents to <tt>stdout</tt>.

You can optionally add the verbose option, <tt>v</tt>, to produce additional information about file sizes and last-modified dates in the output.

## An Example

Let's use the Jar tool to list the contents of the <tt>TicTacToe.jar</tt> file we created in the previous section:

```

jar tf TicTacToe.jar

```

This command displays the contents of the JAR file to <tt>stdout</tt>:

```

META-INF/MANIFEST.MF
TicTacToe.class
audio/
audio/beep.au
audio/ding.au
audio/return.au
audio/yahoo1.au
audio/yahoo2.au
images/
images/cross.gif
images/not.gif

```

The JAR file contains the <tt>TicTacToe</tt> class file and the audio and images directory, as expected. The output also shows that the JAR file contains a default manifest file, <tt>META-INF/MANIFEST.MF</tt>, which was automatically placed in the archive by the JAR tool. For more information, see the 
[Understanding the Default Manifest](defman.html) section.

All pathnames are displayed with forward slashes, regardless of the platform or operating system you're using. Paths in JAR files are always relative; you'll never see a path beginning with <tt>C:</tt>, for example.

The JAR tool will display additional information if you use the <tt>v</tt> option:

```

jar tvf TicTacToe.jar

```

For example, the verbose output for the TicTacToe JAR file would look similar to this:

```

    68 Thu Nov 01 20:00:40 PDT 2012 META-INF/MANIFEST.MF
   553 Mon Sep 24 21:57:48 PDT 2012 TicTacToe.class
  3708 Mon Sep 24 21:57:48 PDT 2012 TicTacToe.class
  9584 Mon Sep 24 21:57:48 PDT 2012 TicTacToe.java
     0 Mon Sep 24 21:57:48 PDT 2012 audio/
  4032 Mon Sep 24 21:57:48 PDT 2012 audio/beep.au
  2566 Mon Sep 24 21:57:48 PDT 2012 audio/ding.au
  6558 Mon Sep 24 21:57:48 PDT 2012 audio/return.au
  7834 Mon Sep 24 21:57:48 PDT 2012 audio/yahoo1.au
  7463 Mon Sep 24 21:57:48 PDT 2012 audio/yahoo2.au
   424 Mon Sep 24 21:57:48 PDT 2012 example1.html
     0 Mon Sep 24 21:57:48 PDT 2012 images/
   157 Mon Sep 24 21:57:48 PDT 2012 images/cross.gif
   158 Mon Sep 24 21:57:48 PDT 2012 images/not.gif

```
