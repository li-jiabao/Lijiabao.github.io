
# Generate Keys

Before signing the `Contract.jar` JAR file containing the `contract` file, you need to generate keys, if you don't already have suitable keys available. You need to sign your JAR file using your private key, and your recipient needs your corresponding public key to verify your signature.

This lesson assumes that you don't have a key pair yet. You are going to create a keystore named `examplestanstore` and create an entry with a newly generated public/private key pair (with the public key in a certificate).

Now pretend that you are Stan Smith and that you work in the legal department of Example2 corporation. Type the following in your command window to create a keystore named `examplestanstore` and to generate keys for Stan Smith:

```

**keytool -genkey -alias signLegal -keystore examplestanstore**

```

The keystore tool prompts you for a keystore password, your distinguished-name information, and the key password. Following are the prompts; the bold indicates what you should type.

```

Enter keystore password:   **&lt;password&gt;**
What is your first and last name?
  [Unknown]:  **Stan Smith** 
What is the name of your organizational unit?
  [Unknown]:  **Legal** 
What is the name of your organization?
  [Unknown]:  **Example2** 
What is the name of your City or Locality?
  [Unknown]:  **New York**
What is the name of your State or Province?
  [Unknown]:  **NY** 
What is the two-letter country code for this unit?
  [Unknown]:  **US** 
Is &lt;CN=Stan Smith, OU=Legal, O=Example2, L=New York, ST=NY, C=US&gt; correct?
  [no]:  **y** 
    
Enter key password for &lt;signLegal&gt;
        (RETURN if same as keystore password):

```

The preceding `keytool` command creates the keystore named `examplestanstore` in the same directory in which the command is executed (assuming that the specified keystore doesn't already exist) and assigns it the entered password. The command generates a public/private key pair for the entity whose distinguished name has a common name of *Stan Smith* and an organizational unit of *Legal*.

The self-signed certificate you have just created includes the public key and the distinguished-name information. (A self-signed certificate is one signed by the private key corresponding to the public key in the certificate.) This certificate is valid for 90 days. This is the default validity period if you don't specify a *-validity* option. The certificate is associated with the private key in a keystore entry referred to by the alias `signLegal`. The private key is assigned the password that was entered.

Self-signed certificates are useful for developing and testing an application. However, users are warned that the application is signed with an untrusted certificate and asked if they want to run the application. To provide users with more confidence to run your application, use a certificate issued by a recognized certificate authority. 
