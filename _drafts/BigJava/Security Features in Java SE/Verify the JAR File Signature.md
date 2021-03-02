
# Lesson: Generating and Verifying Signatures

This lesson walks you through the steps necessary to use the JDK Security API to generate a digital signature for data and to verify that a signature is authentic. This lesson is meant for developers who wish to incorporate security functionality into their programs, including cryptography services.

This lesson demonstrates the use of the JDK Security API with respect to signing documents. The lesson shows what one program, executed by the person who has the original document, would do to generate keys, generate a digital signature for the document using the private key, and export the public key and the signature to files.

Then it shows an example of another program, executed by the receiver of the document, signature, and public key. It shows how the program could import the public key and verify the authenticity of the signature. The lesson also discusses and demonstrates possible alternative approaches and methods of supplying and importing keys, including in certificates.

For further information about the concepts and terminology (digital signatures, certificates, keystores), see the 
[API and Tools Use for Secure Code and File Exchanges](../sigcert/index.html) lesson.

In this lesson you create two basic applications, one for the digital signature generation and the other for the verification. This is followed by a discussion and demonstration of potential enhancements. The lesson contains three sections.

<li>
[Generating a Digital Signature](gensig.html) shows using the API to generate keys and a digital signature for data using the private key and to export the public key and the signature to files. The application gets the data file name from the command line.
</li>
<li>
[Verifying a Digital Signature](versig.html) shows using the API to import a public key and a signature that is alleged to be the signature of a specified data file and to verify the authenticity of the signature. The data, public key, and signature file names are specified on the command line.
</li>
<li>
[Weaknesses and Alternatives](enhancements.html) discusses potential weaknesses of the approach used by the basic programs. It then presents and demonstrates possible alternative approaches and methods of supplying and importing keys, including the use of files containing encoded key bytes and the use of certificates containing public keys.
</li>
