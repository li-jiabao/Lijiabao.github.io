---
title: Signing JAR Files
author: lijiabao
date: 2020-12-06 12:45:59.746855900 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Signing JAR Files

You use the JAR Signing and Verification Tool to sign JAR files and time stamp the signature. You invoke the JAR Signing and Verification Tool by using the <tt>jarsigner</tt> command, so we'll refer to it as "Jarsigner" for short.

To sign a JAR file, you must first have a private key. Private keys and their associated public-key certificates are stored in password-protected databases called **keystores**. A keystore can hold the keys of many potential signers. Each key in the keystore can be identified by an **alias** which is typically the name of the signer who owns the key. The key belonging to Rita Jones might have the alias "rita", for example.

The basic form of the command for signing a JAR file is

```

jarsigner *jar-file alias*

```

In this command:

- <tt>jar-file</tt> is the pathname of the JAR file that's to be signed.
- <tt>alias</tt> is the alias identifying the private key that's to be used to sign the JAR file, and the key's associated certificate.

The Jarsigner tool will prompt you for the passwords for the keystore and alias.

This basic form of the command assumes that the keystore to be used is in a file named <tt>.keystore</tt> in your home directory. It will create signature and signature block files with names <tt>x.SF</tt> and <tt>x.DSA</tt> respectively, where <tt>x</tt> is the first eight letters of the alias, all converted to upper case. This basic command will **overwrite** the original JAR file with the signed JAR file.

In practice, you might want to use one or more of the command options that are available. For example, time stamping the signature is encouraged so that any tool used to deploy your application can verify that the certificate used to sign the JAR file was valid at the time that the file was signed. A warning is issued by the Jarsigner tool if a time stamp is not included.

Options precede the <tt>jar-file</tt> pathname. The following table describes the options that are available:<br />
<br />
<th id="h1">Option</th><th id="h2">Description</th>
<td headers="h1"><tt>-keystore</tt>&#160;*url*</td><td headers="h2">Specifies a keystore to be used if you don't want to use the <tt>.keystore</tt> default database.</td>
<td headers="h1"><tt>-sigfile</tt>&#160;*file*</td><td headers="h2">Specifies the base name for the .SF and .DSA files if you don't want the base name to be taken from your alias. *file* must be composed only of upper case letters (A-Z), numerals (0-9), hyphen (-), and underscore (_).</td>
<td headers="h1"><tt>-signedjar</tt>&#160;*file*</td><td headers="h2">Specifies the name of the signed JAR file to be generated if you don't want the original unsigned file to be overwritten with the signed file.</td>
<td headers="h1"><tt>-tsa</tt>&#160;*url*</td><td headers="h2">Generates a time stamp for the signature using the Time Stamping Authority (TSA) identified by the URL.</td>
<td headers="h1"><tt>-tsacert</tt>&#160;*alias*</td><td headers="h2">Generates a time stamp for the signature using the TSA's public key certificate identified by *alias*.</td>
<td headers="h1"><tt>-altsigner</tt>&#160;*class*</td><td headers="h2">Indicates that an alternative signing mechanism be used to time stamp the signature. The fully-qualified class name identifies the class used.</td>
<td headers="h1"><tt>-altsignerpath</tt>&#160;*classpathlist*</td><td headers="h2">Provides the path to the class identified by the <tt>altsigner</tt> option and any JAR files that the class depends on.</td>

## Example

Let's look at a couple of examples of signing a JAR file with the Jarsigner tool. In these examples, we will assume the following:

  - Your alias is "johndoe".
  - The keystore you want to use is in a file named "mykeys" in the current working directory.
  - The TSA that you want to use to time stamp the signature is located at `http://tsa.url.example.com`.

Under these assumptions, you could use this command to sign a JAR file named <tt>app.jar</tt>:

```

jarsigner -keystore mykeys -tsa http://tsa.url.example.com app.jar johndoe

```

You will be prompted to enter the passwords for both the keystore and your alias. Because this command doesn't make use of the <tt>-sigfile</tt> option, the .SF and .DSA files it creates would be named <tt>JOHNDOE.SF</tt> and <tt>JOHNDOE.DSA</tt>. Because the command doesn't use the <tt>-signedjar</tt> option, the resulting signed file will overwrite the original version of <tt>app.jar</tt>.

Let's look at what would happen if you used a different combination of options:

```

jarsigner -keystore mykeys -sigfile SIG -signedjar SignedApp.jar 
          -tsacert testalias app.jar johndoe

```

The signature and signature block files would be named <tt>SIG.SF</tt> and <tt>SIG.DSA</tt>, respectively, and the signed JAR file <tt>SignedApp.jar</tt> would be placed in the current directory. The original unsigned JAR file would remain unchanged. Also, the signature would be time stamped with the TSA's public key certificate identified as <tt>testalias</tt>.

## Additional Information

Complete reference pages for the JAR Signing and Verification Tool are on-line: 
[Summary of Security Tools](https://docs.oracle.com/javase/8/docs/technotes/guides/security/SecurityToolsSummary.html)
