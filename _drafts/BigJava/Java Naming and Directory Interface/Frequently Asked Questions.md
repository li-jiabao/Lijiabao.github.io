
# Lesson: Java Objects in the Directory

Traditionally, directories have been used to store data. Users and programmers think of the directory as a hierarchy of directory entries, each containing a set of attributes. You look up an entry from the directory and extract the attribute(s) of interest.

For applications written in the Java programming language, Java objects may sometimes be shared across applications. For such applications, it makes sense to be able to use the directory as a repository for Java objects. The directory provides a centrally administered, and possibly replicated, service for use by Java applications distributed across the network. For example, an application server might use the directory for registering objects that represent the services that it manages so that a client can later search the directory to locate those services as needed. An example of JNDI used as a directory of services is Apache DS. More information about this can be found at 
[Apache Directory](http://directory.apache.org/).

The JNDI provides an object-oriented view of the directory, thereby allowing Java objects to be added to and retrieved from the directory without requiring the client to manage data representation issues. This lesson discusses the use of the directory for storing and retrieving Java objects at a basic level. The JNDI provides what are known as object and state factories for creating and storing the objects accessed from the directory. <a name="OBJFAC" id="OBJFAC"></a>

## Object Factory

An object factory is a producer of objects. It accepts some information about how to create an object, such as a reference, and then returns an instance of that object. For details about Object Factories and the format in which objects are stored in the directory please refer to the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/objects/factory/index.html). <a name="STATEFAC" id="STATEFAC"></a>

## State Factory

A state factory transforms an object into another object. The input is the object and optional attributes, supplied to Context.bind() and the output is another object and optional attributes, to be stored in the underlying naming service or directory. For details about State Factories and on how to write your own state factory please refer to the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/objects/state/index.html).

The next part of the lesson discusses how to access Objects in the Directory It describes how serializable objects can be stored and read in the directory. For other types of objects please check out the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/objects/index.html).
