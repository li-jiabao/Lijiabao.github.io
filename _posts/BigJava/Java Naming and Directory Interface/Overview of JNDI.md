
# Naming Package

The 
[<tt>javax.naming</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-summary.html) package contains classes and interfaces for accessing naming services.

## Context

The <tt>javax.naming</tt> package defines a 
[<tt>Context</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html) interface, which is the core interface for looking up, binding/unbinding, renaming objects and creating and destroying subcontexts.

The JNDI defines the 
[<tt>Reference</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Reference.html) class to represent reference. A reference contains information on how to construct a copy of the object. The JNDI will attempt to turn references looked up from the directory into the Java objects that they represent so that JNDI clients have the illusion that what is stored in the directory are Java objects.

## The Initial Context

In the JNDI, all naming and directory operations are performed relative to a context. There are no absolute roots. Therefore the JNDI defines an 
[<tt>InitialContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html), which provides a starting point for naming and directory operations. Once you have an initial context, you can use it to look up other contexts and objects.

## Exceptions

The JNDI defines a class hierarchy for exceptions that can be thrown in the course of performing naming and directory operations. The root of this class hierarchy is 
[<tt>NamingException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html). Programs interested in dealing with a particular exception can catch the corresponding subclass of the exception. Otherwise, they should catch <tt>NamingException</tt>.
