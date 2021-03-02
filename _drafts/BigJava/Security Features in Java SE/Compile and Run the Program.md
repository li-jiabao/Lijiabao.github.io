
# Weaknesses and Alternatives

The `GenSig` and `VerSig` programs in this lesson illustrate the use of the JDK Security API to generate a digital signature for data and to verify that a signature is authentic. However, the actual scenario depicted by those programs, in which a sender uses the JDK Security API to generate a new public/private key pair, the sender stores the encoded public key bytes in a file, and the receiver reads in the key bytes, is not necessarily realistic, and has a potential major flaw.

In many cases the keys do not need to be generated; they already exist, either as encoded keys in files or as entries in a keystore.

The potential major flaw is that nothing guarantees the authenticity of the public key the receiver receives, and the `VerSig` program correctly verifies the authenticity of a signature only if the public key it is supplied is *itself* authentic!

## Working with Encoded Key Bytes

Sometimes encoded key bytes already exist in files for the key pair to be used for signing and verification. If that's the case the `GenSig` program can import the encoded private key bytes and convert them to a `PrivateKey` needed for signing, via the following, assuming that the name of the file containing the private key bytes is in the `privkeyfile` `String` and that the bytes represent a DSA key that has been encoded by using the PKCS #8 standard.

```

FileInputStream keyfis = new FileInputStream(privkeyfile);
byte[] encKey = new byte[keyfis.available()];
keyfis.read(encKey);
keyfis.close();

PKCS8EncodedKeySpec privKeySpec = new PKCS8EncodedKeySpec(encKey);

KeyFactory keyFactory = KeyFactory.getInstance("DSA");
PrivateKey privKey = keyFactory.generatePrivate(privKeySpec);

```

`GenSig` no longer needs to save the public key bytes in a file, as they're already in one.

In this case the sender sends the receiver

- the already existing file containing the encoded public key bytes (unless the receiver already has this) and
- the data file and the signature file exported by `GenSig`.

The `VerSig` program remains unchanged, as it already expects encoded public key bytes in a file.

But what about the potential problem of a malicious user intercepting the files and replacing them all in such a way that their switch cannot be detected? In some cases this is not an issue, because people have already exchanged public keys face to face or via a trusted third party that does the face-to-face exchange. After that, multiple subsequent file and signature exchanges may be done remotely (that is, between two people in different locations), and the public keys may be used to verify their authenticity. If a malicious user tries to change the data or signature, this is detected by `VerSig`.

If a face-to-face key exchange is not possible, you can try other methods of increasing the likelihood of proper receipt. For example, you could send your public key via the most secure method possible prior to subsequent exchanges of data and signature files, perhaps using less secure mediums.

In general, sending the data and the signature separately from your public key greatly reduces the likelihood of an attack. Unless all three files are changed, and in a certain manner discussed in the next paragraph, `VerSig` will detect any tampering.

If all three files (data document, public key, and signature) were intercepted by a malicious user, that person could replace the document with something else, sign it with a private key, and forward on to you the replaced document, the new signature, and the public key corresponding to the private key used to generate the new signature. Then `VerSig` would report a successful verification, and you'd think that the document came from the original sender. Thus you should take steps to ensure that at least the public key is received intact (`VerSig` detects any tampering of the other files), or you can use certificates to facilitate authentication of the public key, as described in the next section.

## Working with Certificates

It is more common in cryptography to exchange *certificates* containing public keys rather than the keys themselves.

One benefit is that a certificate is signed by one entity (the *issuer*) to verify that the enclosed public key is the actual public key of another entity (the *subject* or *owner*). Typically a trusted third-party *certification authority* (CA) verifies the identity of the subject and then vouches for its being the owner of the public key by signing the certificate.

Another benefit of using certificates is that you can check to ensure the validity of a certificate you received by verifying its digital signature, using its issuer's (signer's) public key, which itself may be stored in a certificate whose signature can be verified by using the public key of that certificate issuer; that public key itself may be stored in a certificate, and so on, until you reach a public key that you already trust.

If you cannot establish a trust chain (perhaps because the required issuer certificates are not available to you), the certificate **fingerprint(s)** can be calculated. Each fingerprint is a relatively short number that uniquely and reliably identifies the certificate. (Technically it's a hash value of the certificate information, using a message digest, also known as a one-way hash function.) You can call up the certificate owner and compare the fingerprints of the certificate you received with the ones sent. If they're the same, the certificates are the same.

It would be more secure for `GenSig` to create a certificate containing the public key and for `VerSig` to then import the certificate and extract the public key. However, the JDK has no public certificate APIs that would allow you to create a certificate from a public key, so the `GenSig` program cannot create a certificate from the public key it generated. (There *are* public APIs for extracting a public key from a certificate, though.)

If you want, you can use the various security tools, not APIs, to sign your important document(s) and work with certificates from a keystore, as was done in the 
[Exchanging Files](../toolfilex/index.html) lesson.

Alternatively you can use the API to modify your programs to work with an already existing private key and corresponding public key (in a certificate) from your keystore. To start, modify the `GenSig` program to extract a private key from a keystore rather than generate new keys. First, let's assume the following:

- The keystore name is in the `String` `ksName`
- The keystore type is "JKS", the proprietary type from Oracle.
- The keystore password is in the char array `spass`
- The alias to the keystore entry containing the private key, and the public key certificate is in the `String` `alias`
- The private key password is in the char array `kpass`

Then you can extract the private key from the keystore via the following.

```

KeyStore ks = KeyStore.getInstance("JKS");
FileInputStream ksfis = new FileInputStream(ksName); 
BufferedInputStream ksbufin = new BufferedInputStream(ksfis);

ks.load(ksbufin, spass);
PrivateKey priv = (PrivateKey) ks.getKey(alias, kpass);

```

You can extract the public key certificate from the keystore and save its encoded bytes to a file named `suecert`, via the following.

```

java.security.cert.Certificate cert = ks.getCertificate(alias);
byte[] encodedCert = cert.getEncoded();

// Save the certificate in a file named "suecert" 

FileOutputStream certfos = new FileOutputStream("suecert");
certfos.write(encodedCert);
certfos.close();

```

Then you send the data file, the signature, and the certificate to the receiver. The receiver verifes the authenticity of the certificate by first getting the certificate's fingerprints, via the `keytool -printcert` command.

```

**keytool -printcert -file suecert**
Owner: CN=Susan Jones, OU=Purchasing, O=ABC, L=Cupertino, ST=CA, C=US
Issuer: CN=Susan Jones, OU=Purchasing, O=ABC, L=Cupertino, ST=CA, C=US
Serial number: 35aaed17
Valid from: Mon Jul 13 22:31:03 PDT 1998 until:
Sun Oct 11 22:31:03 PDT 1998
Certificate fingerprints:
MD5:  1E:B8:04:59:86:7A:78:6B:40:AC:64:89:2C:0F:DD:13
SHA1: 1C:79:BD:26:A1:34:C0:0A:30:63:11:6A:F2:B9:67:DF:E5:8D:7B:5E

```

Then the receiver verifies the fingerprints, perhaps by calling the sender up and comparing them with those of the sender's certificate or by looking them up in a public repository.

The receiver's verification program (a modified `VerSig`) can then import the certificate and extract the public key from it via the following, assuming that the certificate file name (for example, `suecert`) is in the `String` `certName`.

```

FileInputStream certfis = new FileInputStream(certName);
java.security.cert.CertificateFactory cf =
    java.security.cert.CertificateFactory.getInstance("X.509");
java.security.cert.Certificate cert =  cf.generateCertificate(certfis);
PublicKey pub = cert.getPublicKey();

```

## Ensuring Data Confidentiality

Suppose that you want to keep the contents of the data confidential so people accidentally or maliciously trying to view it in transit (or on your own machine or disk) cannot do so. To keep the data confidential, you should encrypt it and store and send only the encryption result (referred to as *ciphertext*). The receiver can decrypt the ciphertext to obtain a copy of the original data.
