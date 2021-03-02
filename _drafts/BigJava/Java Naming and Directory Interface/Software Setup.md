
# LDAP Setup

Below are the steps involved in building a Java Application that accesses an LDAP Directory Server.

1. Install the [Java Platform]() Software.
<li>Get the Directory Server software as discussed 
[earlier](index.html#SERVER).</li>
1. Configure the Directory Server with the desired schema. For using the examples in this tutorial a special [schema](#SCHEMA) needs to be configured on the server.
1. Populate the directory server with the desired content. For using the examples in this tutorial a special [content](#LDIF) needs to be populated on the server.
<li>Write a JNDI application to access the Directory, compile and run it against the Directory Server to get your desired results. The JNDI examples are covered in the next 
[lesson](../ops/index.html).</li>

The **first two** steps are covered in the previous section. The rest of this lesson discusses steps **three** and part of step **four**. The step **five** that involves writing a JNDI application is covered in the next lesson that shows how to write JNDI applications to perform various operations on the directory.

Once you've set up the directory, or have directed your program to communicate with an existing directory, what sort of information can you expect to find there?

The directory can be viewed as consisting of name-to-object bindings. That is, each object in the directory has a corresponding name. You can retrieve an object in the directory by looking up its name.

Also stored in the directory are attributes. An object in the directory, in addition to having a name, also has an optional set of attributes. You can ask the directory for an object's attributes, as well as ask it to search for an object that has certain attributes.

## <a name="SCHEMA" id="SCHEMA">Step 3: Directory Schema</a>

A schema specifies the types of objects that a directory may contain. This tutorial populates the directory with entries, some of which require special schema definitions. To accommodate these entries, you must first either turn off schema-checking in the server or add the schema files that accompany this tutorial to the server. Both of these tasks are typically performed by the directory server's administrator.

This tutorial comes with two schema files that must be installed:

<li>
[`Schema for Java objects`](config/java.schema)</li>
<li>
[`Schema for CORBA objects`](config/corba.schema)</li>

The format of these files is a formal description that possibly cannot be directly copied and pasted into server configuration files. Specifically, the attribute syntaxes are described in terms of 
[RFC 2252](http://www.ietf.org/rfc/rfc2252.txt).

Different directory servers have different ways of configuring their schema. This tutorial includes some tools for installing the Java and CORBA schemas on directory servers that permit their schemas to be modified via the LDAP. Following is a list of tasks the tools can perform.

<li>
[`Create Java Schema`](config/CreateJavaSchema.java)
</li>
<li>
[`Create CORBA Schema`](config/CreateCorbaSchema.java)
</li>

Follow the instructions in the accompanying 
[`README file`](config/README-SCHEMA.TXT) to run these programs.

**Note: Windows Active Directory.** Active Directory manages its schema by using an internal format. To update the schema, you can use either the Active Directory Management Console snap-in, <tt>ADSIEdit</tt>, or the <tt>CreateJavaSchema</tt> utility, following the instructions for Active Directory.

## Step 4: Providing Directory Content for This Tutorial

<a name="LDIF" id="LDIF"></a>In the examples of this trail, the results shown reflect how the LDAP directory has been set up using the configuration file (
[`tutorial.ldif`](config/tutorial.ldif)
) that accompanies this tutorial. If you are using an existing server, or a server with a different setup, then you might see different results. Before you can load the configuration file (

[`tutorial.ldif`](config/tutorial.ldif) ) into the directory server, you must follow the instructions for updating the server's schema or you can use **ldapadd** or **ldapmodify** command if available on your UNIX system.

For example, using ldapmodify you could do (by plugging in appropriate values for the hostname, administrator DN (-D option), and the password):

```

ldapmodify -a -c -v -h hostname -p 389\
        -D "cn=Administrator, cn=users, dc=xxx, dc=xxx"\
        -w passwd -f tutorial.ldif

```

**Installation Note: Access Control.** Different directory servers handle access control differently. Some examples in this tutorial perform updates to the directory. Also, the part of the namespace where you have installed the tutorial might have read access restrictions. Therefore, you need to take server-specific actions to make the directory readable and/or updatable in order for those examples to work. For the 
[Oracle Directory Server](http://www.oracle.com/technetwork/testcontent/index-085178.html) add the <tt>aci</tt> entry suggested in the 

[`<tt>sunds.aci.ldif</tt>`](config/sunds.aci.ldif) file to the <tt>dn: o=JNDITutorial</tt> entry to make the entire directory readable and updatable. Alternatively, you may change the examples so that they authenticate to the directory. Details of how to do this are described in the 
[Security](../ldap/security.html) lesson.

**Installation Note: Namespace Setup.** The entries in the 
[`tutorial.ldif`](config/tutorial.ldif) file use the distinguished name (DN) "o=JNDITutorial" for the root naming context. If you have not configured your directory server to have "o=JNDITutorial" as a root naming context, then your attempt to import <tt>tutorial.ldif</tt> will fail. The easiest way to get around this problem is to add the DN of an existing root naming context to each "dn:" line in the <tt>tutorial.ldif</tt> file. For example, if your server already has the root naming context "dc=imc,dc=org", then you should change the line

```

dn: o=JNDITutorial

```

to

```

dn: o=JNDITutorial, dc=imc, dc=org

```

Make this change for each line that begins with "dn:" in the file. Then, in all of the examples in this tutorial, wherever it uses "o=JNDITutorial", use "o=JNDITutorial,dc=imc,dc=org" instead.

**Installation Note: File Format.** Depending on the operating system platform that you are using, you might need to edit <tt>tutorial.ldif</tt> so that it contains the correct newline characters for that platform. For example, if you find that <tt>tutorial.ldif</tt> contains Windows-style newline characters (CRLF) and you are importing this file into a directory server that is running on a UNIX platform, then you need to edit the file and replace CRLF with LF. A symptom of this problem is that the directory server rejects all of the entries in <tt>tutorial.ldif</tt>.

**Installation Note: Windows Active Directory.**

1. The root naming context is not going to be "o=jnditutorial". It will be of the form "dc=x,dc=y,dc=z". You need to follow the previous **Namespace Setup** note.
<li>Add the object classes and related attributes for the "inetOrgPerson" and "groupOfUniqueNames" object classes to the Active Directory schema by using the Active Directory Management Console snap-in, <tt>ADSIEdit</tt>. "groupOfUniqueNames" is defined in 
[RFC 2256](http://www.ietf.org/rfc/rfc2256.txt)
, "inetOrgPerson" in 
[RFC 2798](http://www.ietf.org/rfc/rfc2798.txt).</li>
<li>Some of hierarchical relationships used by the tutorial are not allowed by default in Active Directory. To enable these relationships, add them by using the Active Directory Management Console snap-in, <tt>ADSIEdit</tt>.
<pre><code>
objectclass: organizationalUnit
possible superiors: domainDNS
                    inetOrgPerson
                    organizaton
                    organizationalPerson
                    organizationalUnit
                    person
                    top

objectclass: groupOfUniqueNames
possible superiors: top

objectclass: inetOrgPerson
possible superiors: container
                    organizationalPerson
                    person
                    top
</code></pre>
</li>
<li>Delete one of the two "sn" attributes from the Mark Twain entry in <tt>tutorial.ldif</tt>. Active Directory defines "sn" to be a single-valued attribute, contrary to 
[RFC 2256](http://www.ietf.org/rfc/rfc2256.txt).</li>
<li>Use the <tt>ldifde</tt> command-line utility to load the modified <tt>tutorial.ldif</tt> file. 
<pre><code>
# ldifde -i -v -k -f tutorial.ldif
</code></pre>
</li>
1. Most of the examples assume that the directory has been set up to permit unauthenticated read and update access. Your Active Directory setup might not allow you to do that. See the **Access Control** installation note.
1. Reading an entry sometimes produces more attributes than are shown in the tutorial because Active Directory often returns some internal attributes.
1. Creation of entries might require the specification of additional Active Directory-specific attributes or the use of other object classes.
