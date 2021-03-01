
# Trail: Security Features in Java SE

In this trail you'll learn how the built-in Java&#8482; security features protect you from malevolent programs. You'll see how to use tools to control access to resources, to generate and to check digital signatures, and to create and to manage keys needed for signature generation and checking. You'll also see how to incorporate cryptography services, such as digital signature generation and checking, into your programs.

The security features provided by the Java Development Kit (JDK&#8482;) are intended for a variety of audiences:

<li>
**Users running programs**:
Built-in security functionality protects you from malevolent programs (including viruses), maintains the privacy of your files and information about you, and authenticates the identity of each code provider. You can subject applications and applets to security controls when you need to.
</li>
<li>
**Developers**:
You can use API methods to incorporate security functionality into your programs, including cryptography services and security checks. The API framework enables you to define and integrate your own permissions (controlling access to specific resources), cryptography service implementations, security manager implementations, and policy implementations. In addition, classes are provided for management of your public/private key pairs and public key certificates from people you trust.
</li>
<li>
**Systems administrators, developers, and users**:
JDK tools manage your keystore (database of keys and certificates); generate digital signatures for JAR files, and verify the authenticity of such signatures and the integrity of the signed contents; and create and modify the policy files that define your installation's security policy.
</li>

## Trail Lessons


<img alt="Trail icon" src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" />&#160;
[Creating a Policy File](./tour1/index.html) shows how resource accesses can be controlled by a policy file. For latest information on policy configuration files, see 

[Policy Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/security/PolicyGuide.html) page.


<img alt="Trail icon" src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" />&#160;
[Quick Tour of Controlling Applications](./tour2/index.html) builds on the previous lesson, showing how resource accesses, such as reading or writing a file, are not permitted for applications that are run under a security manager unless explicitly allowed by a permission in a policy file.

<img alt="Trail icon" src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" />&#160;
[API and Tools Use for Secure Code and File Exchanges](./sigcert/index.html) defines digital signatures, certificates, and keystores and discusses why they are needed. It also reviews information applicable to the next three lessons regarding the steps commonly needed for using the tools or the API to generate signatures, export/import certificates, and so on.

<img alt="Trail icon" src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" />&#160;
[Signing Code and Granting It Permissions](./toolsign/index.html) illustrates the use of all the security-related tools. It shows the steps that a developer would take to sign and to distribute code for others to run. The lesson also shows how someone who will run the code (or a system administrator) could add an entry in a policy file to grant the code permission for the resource accesses it needs.

<img alt="Trail icon" src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" />&#160;
[Exchanging Files](./toolfilex/index.html) shows use of the tools by one person to sign an important document, such as a contract, and to export the public key certificate for the public key corresponding to the private key used to sign the contract. Then the lesson shows how another person, who receives the contract, the signature, and the public key certificate, can import the certificate and verify the signature.


<img alt="Trail icon" src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" />&#160;
[Generating and Verifying Signatures](./apisign/index.html) walks you step by step through an example of writing a Java program using the JDK Security API to generate keys, to generate a digital signature for data using the private key, and to export the public key and the signature to files. Then the example shows writing a second program, which may be expected to run on a different person's computer, that imports the public key and verifies the authenticity of the signature. Finally, the example discusses potential weaknesses of the approach used by the basic programs and demonstrates possible alternative approaches and methods of supplying and importing keys, including in certificates.


<img alt="Trail icon" src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" />&#160;
[Implementing Your Own Permission](./userperm/index.html) demonstrates how to write a class that defines its own special permission.

## For More Information

JDK security release documentation can be found at the 
[Security](https://docs.oracle.com/javase/8/docs/technotes/guides/security/index.html) guides page. This index page lists Specifications which present detailed information about latest security features, including architecture specifications, usage guides, API documentation, and tool documentation.
