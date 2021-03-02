
# Steps for the Contract Sender

The steps outlined here for the contract sender are *basically the same* as those listed for a code signer in the 
[Signing Code and Granting It Permissions](../toolsign/index.html) lesson. Here, however, you are pretending to be Stan Smith rather than Susan Jones and are storing a data file rather than a class file in the JAR file to be signed.

The steps you take as the contract sender are as follows.

1. [Create a JAR File Containing the Contract](step1.html), using the `jar` tool.
<li>[Generate Keys](step2.html) (if they don't already exist), using the `keytool` `-genkey` command.
<p>*Optional Step*: Generate a certificate signing request (CSR) for the public key certificate, and import the response from the certification authority. For simplicity and since you are only pretending to be Stan Smith, this step is omitted. See 
[Generating a Certificate Signing Request (CSR) for a Public Key Certificate](../sigcert/index.html#GenCSR) for more information.</p>
</li>
1. [Sign the JAR File](step3.html), using the `jarsigner` tool and the private key generated in step 2.
1. [Export the Public Key Certificate](step4.html), using the `keytool` `-export` command. Then supply the signed JAR file and the certificate to the receiver, Ruth.
