
# Lesson: Exchanging Files

If you want to electronically send an important document, (like a Contract) to someone else, it is a good idea to digitally "sign" the document, so your recipient can check that the document indeed came from you and was not altered in transit.

This lesson shows you how to use Security tools for the exchange of an important document, in this case a contract.

You first pretend that you are the contract sender, Stan Smith. This lesson shows the steps Stan would use to put the contract in a JAR file, sign it, and export the public key certificate for the public key corresponding to the private key used to sign the JAR file.

Then you pretend that you are Ruth, who has received the signed JAR file and the certificate. You'll use `keytool` to import the certificate into Ruth's keystore in an entry aliased by `stan`, and use the `jarsigner` tool to verify the signature.

For further information about digital signatures, certificates, keystores, and the tools, see the 
[API and Tools Use for Secure Code and File Exchanges](../sigcert/index.html) lesson.

Here are the steps:

- [Steps for the Contract Sender](sender.html)
- [Steps for the Contract Receiver](receiver.html)
