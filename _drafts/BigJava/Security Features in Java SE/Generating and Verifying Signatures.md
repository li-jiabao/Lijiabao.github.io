
# Generating a Digital Signature

The `GenSig` program you are about to create will use the JDK Security API to generate keys and a digital signature for data using the private key and to export the public key and the signature to files. The application gets the data file name from the command line.

The following steps create the `GenSig` sample program.

<li>
[Prepare Initial Program Structure](step1.html)
Create a text file named `GenSig.java`. Type in the initial program structure (import statements, class name, `main` method, and so on).
</li>
<li>
[Generate Public and Private Keys](step2.html)
Generate a key pair (public key and private key). The private key is needed for signing the data. The public key will be used by the [`VerSig`](versig.html) program for verifying the signature.
</li>
<li>
[Sign the Data](step3.html)
Get a `Signature` object and initialize it for signing. Supply it with the data to be signed, and generate the signature.
</li>
<li>
[Save the Signature and the Public Key in Files](step4.html)
Save the signature bytes in one file and the public key bytes in another.
</li>
<li>
[Compile and Run the Program](step5.html)
</li>
