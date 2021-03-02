
# Import the Certificate as a Trusted Certificate

Before you can grant the signed code permission to read a specified file, you need to import Susan's certificate as a trusted certificate in your keystore.

Suppose that you have received from Susan

- the signed JAR file `sCount.jar`, which contains the `Count.class` file, and
- the file `Example.cer`, which contains the public key certificate for the public key corresponding to the private key used to sign the JAR file.

Even though you created these files and they haven't actually been transported anywhere, you can simulate being someone other than the creater and sender, Susan. Pretend that you are now Ray. Acting as Ray, you will create a keystore named `exampleraystore` and will use it to import the certificate into an entry with an alias of `susan`.

A keystore is created whenever you use a `keytool` command specifying a keystore that doesn't yet exist. Thus we can create the `exampleraystore` and import the certificate via a single `keytool` command. Do the following in your command window.

1. Go to the directory containing the public key certificate file `Example.cer`. (You should actually already be there, since this lesson assumes that you stay in a single directory throughout.)
<li>Type the following command on one line:
<pre><code>
<b>keytool -import -alias susan
   -file Example.cer -keystore exampleraystore</b>
</code></pre>
</li>

Since the keystore doesn't yet exist, it will be created, and you will be prompted for a keystore password; type whatever password you want.

The `keytool` command will print out the certificate information and ask you to verify it, for example, by comparing the displayed certificate fingerprints with those obtained from another (trusted) source of information. (Each fingerprint is a relatively short number that uniquely and reliably identifies the certificate.) For example, in the real world you might call up Susan and ask her what the fingerprints should be. She can get the fingerprints of the `Example.cer` file she created by executing the command

```

**keytool -printcert -file Example.cer**

```

If the fingerprints she sees are the same as the ones reported to you by `keytool`, the certificate has not been modified in transit. In that case you let `keytool` proceed with placing a trusted certificate entry in the keystore. The entry contains the public key certificate data from the file `Example.cer` and is assigned the alias `susan`.
