
# Steps for the Code Signer

The code signer takes the following steps:

1. [Download and Try the Sample Application.](step1.html)
1. [Create a JAR File Containing the Class File](step2.html), using the `jar` tool.
<li>[Generate Keys](step3.html) (if they don't already exist), using the `keytool` `-genkey` command.
<hr />
*Optional Step* Generate a certificate signing request (CSR) for the public key certificate, and import the response from the certification authority (CA). For simplicity (and since you are only pretending to be Susan Jones), this step is omitted. See 
[Generating a Certificate Signing Request (CSR) for a Public Key Certificate](../sigcert/index.html#GenCSR) for more information.
<hr /></li>
1. [Sign the JAR File](step4.html), using the `jarsigner` tool and the private key.
1. [Export the Public Key Certificate](step5.html), using the `keytool` `-export` command. Then supply the signed JAR file and the certificate to the receiver Ray.
