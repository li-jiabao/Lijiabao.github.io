
# Manage Referral Control

The Manage Referral control (
[RFC 3296](http://www.ietf.org/rfc/rfc3296.txt)) enables manipulation of the referral and other special objects as normal objects when performing an LDAP operation. In other words, Manage Referral control tells the LDAP server to return referral entries as ordinary entries instead of returning "referral" error responses or continuation references. The new class in JDK 5.0 below enables you to send Manage Referral Control along with an LDAP request:


[<tt>javax.naming.ldap.ManageReferralControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/ManageReferralControl.html)

The LDAP service provider in the JDK will send this control automatically along with any request. You can also explicitly enable it setting Context.REFERRAL environment property to "ignore". For more information on Referral handling check the 
[Referrals in the JNDI](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/jndi.html) section of the JNDI Tutorial.

Here is an example that sends Manage Referral control along with an LDAP request.

```

         // Create initial context
         LdapContext ctx = (LdapContext) new InitialDirContext(env);
         ctx.setRequestControl(new Control[] new ManageReferralControl());

         // Set controls for performing subtree search
         SearchControls ctls = new SearchControls();
         ctls.setSearchScope(SearchControls.SUBTREE_SCOPE);

         // Perform search
         NamingEnumeration answer = ctx.search("", "(objectclass=*)", ctls);

         // Print the answer
         while (answer.hasMore()) {
             System.out.println("&gt;&gt;&gt;" + 
                 ((SearchResult)answer.next()).getName());
          }

          // Close the context when we're done
          ctx.close();

```

The complete example can be found 
[`here`](examples/ManageReferral.java).

**Note 1:** The above example will require you to set up a second server using the configuration file 
[`refserver.ldif`](../software/config/refserver.ldif). The server must support LDAP v3 and RFC 3296. If the server does not support referrals in this way, then the example won't work as shown. The configuration file contains referrals that point to the original server that you've set up. It assumes that the original server is on port 389 on the local machine. If you have set up the server on another machine or port, then you need to edit the "ref" entries in the refserver.ldif file and replace "localhost:389" with the appropriate setting. The second server is to be set up on port 489 on the local machine. If you set up the second server on another machine or port, then you need to adjust the setting of the Context.PROVIDER_URL environment property for the initial context accordingly.

Setting up a directory server is typically performed by the directory or system administrator. See the 
[Software Setup](../software/index.html) lesson for more information.

**Note 2:** Windows Active Directory: Because Active Directory does not support the Manage Referral control, none of the examples in this lesson will work against Active Directory.
