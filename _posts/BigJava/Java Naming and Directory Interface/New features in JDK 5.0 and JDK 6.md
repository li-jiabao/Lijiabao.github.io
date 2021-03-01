
# Retrieving Distinguished Name

In the JDK releases prior to 5.0, there is no direct way of obtaining the Distinguished Name (DN) from the search results. The <tt>SearchResults.getName()</tt> method always returns the name that is relative to the context on which the search is performed. In order to get the absolute, or full name of the search entry some amount of book-keeping is required to track the ancestor contexts. Two new APIs below are added in JDK 5.0 to retrieve the absolute name from the NameClassPair, whenever a search, list, or listBindings operation is performed on a context:

<li>
[<tt>NameClassPair.getNameInNameSpace(Name name)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getNameInNamespace-Name-)</li>
<li>
[<tt>NameClassPair.getNameInNameSpace(String name)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getNameInNamespace-String-)</li>

Here is an example that retrieves DNs from an LDAP search:

```

     public static void printSearchEnumeration(NamingEnumeration retEnum) {
         try {
             while (retEnum.hasMore()) {
                 SearchResult sr = (SearchResult) retEnum.next();
                 System.out.println("&gt;&gt;" + sr.getNameInNamespace());
             }
         } catch (NamingException e) {
             e.printStackTrace();
         }
     }

```

The complete example can be obtained from 
[`here`](examples/FullName.java). This program generates the output as below:

```

        &gt;&gt;cn=Jon Ruiz, ou=People, o=JNDITutorial
        &gt;&gt;cn=Scott Seligman, ou=People, o=JNDITutorial
        &gt;&gt;cn=Samuel Clemens, ou=People, o=JNDITutorial
        &gt;&gt;cn=Rosanna Lee, ou=People, o=JNDITutorial
        &gt;&gt;cn=Maxine Erlund, ou=People, o=JNDITutorial
        &gt;&gt;cn=Niels Bohr, ou=People, o=JNDITutorial
        &gt;&gt;cn=Uri Geller, ou=People, o=JNDITutorial
        &gt;&gt;cn=Colleen Sullivan, ou=People, o=JNDITutorial
        &gt;&gt;cn=Vinnie Ryan, ou=People, o=JNDITutorial
        &gt;&gt;cn=Rod Serling, ou=People, o=JNDITutorial
        &gt;&gt;cn=Jonathan Wood, ou=People, o=JNDITutorial
        &gt;&gt;cn=Aravindan Ranganathan, ou=People, o=JNDITutorial
        &gt;&gt;cn=Ian Anderson, ou=People, o=JNDITutorial
        &gt;&gt;cn=Lao Tzu, ou=People, o=JNDITutorial
        &gt;&gt;cn=Don Knuth, ou=People, o=JNDITutorial
        &gt;&gt;cn=Roger Waters, ou=People, o=JNDITutorial
        &gt;&gt;cn=Ben Dubin, ou=People, o=JNDITutorial
        &gt;&gt;cn=Spuds Mackenzie, ou=People, o=JNDITutorial
        &gt;&gt;cn=John Fowler, ou=People, o=JNDITutorial
        &gt;&gt;cn=Londo Mollari, ou=People, o=JNDITutorial
        &gt;&gt;cn=Ted Geisel, ou=People,o=JNDITutorial 

```
