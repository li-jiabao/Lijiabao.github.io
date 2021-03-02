
# Lesson: Signing Code and Granting It Permissions

This lesson shows how to use use `keytool`, `jarsigner`, `Policy Tool` and `jar` to place files into JAR (Java ARchive) files for subsequent signing by the `jarsigner` tool.

This lesson has two parts. First, you will create and deploy an application. Second; you will act as the recipient of a signed application.

Here are the steps to create and deploy an application:

**Note:**&#160;&#160;For convenience, you pretend to be a user/developer named Susan Jones. You need to define Susan Jones when you generate the keys.

- Put Java class files comprising your application into a JAR file
- Sign the JAR file
- Export the public key certificate corresponding to the private key used to sign the JAR file

Here are the steps to grant permissions to an application

**Note:**&#160;&#160;For convenience, you pretend to be a user named Ray.

- You see how the signed application cannot normally read a file when it is run under a security manager.
- Use `keytool` to import a certificate into Ray's keystore in an entry aliased by `susan`
- Use the Policy Tool to create an entry in Ray's policy file to permit code signed by `susan` to read the specified file.
- Finally, you see how the application running under a security manager can now read the file, since it has been granted permission to do so.

For more information about digital signatures, certificates, keystores, and the tools, see the 
[API and Tools Use for Secure Code and File Exchanges](../sigcert/index.html) lesson.

Here are the steps:

- [Steps for the Code Signer](signer.html)
- [Steps for the Code Receiver](receiver.html)
