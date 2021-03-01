
# Writing an RMI Server

The compute engine server accepts tasks from clients, runs the tasks, and returns any results. The server code consists of an interface and a class. The interface defines the methods that can be invoked from the client. Essentially, the interface defines the client's view of the remote object. The class provides the implementation.

[Designing a Remote Interface](designing.html)

This section explains the `Compute` interface, which provides the connection between the client and the server. You will also learn about the RMI API, which supports this communication.

[Implementing a Remote Interface](implementing.html)

This section explores the class that implements the `Compute` interface, thereby implementing a remote object. This class also provides the rest of the code that makes up the server program, including a `main` method that creates an instance of the remote object, registers it with the RMI registry, and sets up a security manager.
