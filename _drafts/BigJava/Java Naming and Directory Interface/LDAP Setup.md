
# Java Application Setup

To use the JNDI in your program, you need to set up its compilation and execution environments.

## Importing the JNDI Classes

Following are the JNDI packages:

<li>
[<tt>javax.naming</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-summary.html)</li>
<li>
[<tt>javax.naming.directory</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/package-summary.html)</li>
<li>
[<tt>javax.naming.event</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/package-summary.html)</li>
<li>
[<tt>javax.naming.ldap</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/package-summary.html)</li>
<li>
[<tt>javax.naming.spi</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/package-summary.html)</li>

The examples in this trail use classes and interfaces from the first two packages. You need to import these two packages into your program or import individual classes and interfaces that you use. The following two lines import all of the classes and interfaces from the two packages <tt>javax.naming</tt> and <tt>javax.naming.directory</tt>.

```

import javax.naming.*;
import javax.naming.directory.*;

```

## Compilation Environment

To compile a program that uses the JNDI, you need access to the JNDI classes. The [Java SE 6]() already include the JNDI classes, so if you are using it you need not take further actions.

## Execution Environment

To run a program that uses the JNDI, you need access to the JNDI classes and classes for any service providers that the program uses. The [Java Runtime Environment (JRE) 6]() already includes the JNDI classes and service providers for LDAP, COS naming, the RMI registry and the DNS .

If you are using some other service providers, then you need to download and install their archive files in the **JAVA_HOME**<tt>/jre/lib/ext</tt> directory, where **JAVA_HOME** is the directory that contains the JRE. The 
[JNDI page](http://www.oracle.com/technetwork/java/jndi/index.html#download) lists some service providers. You may download these providers or use providers from other vendors.
