
# Lesson: All About Sockets

`URL`s and `URLConnection`s provide a relatively high-level mechanism for accessing resources on the Internet. Sometimes your programs require lower-level network communication, for example, when you want to write a client-server application.

In client-server applications, the server provides some service, such as processing database queries or sending out current stock prices. The client uses the service provided by the server, either displaying database query results to the user or making stock purchase recommendations to an investor. The communication that occurs between the client and the server must be reliable. That is, no data can be dropped and it must arrive on the client side in the same order in which the server sent it.

TCP provides a reliable, point-to-point communication channel that client-server applications on the Internet use to communicate with each other. To communicate over TCP, a client program and a server program establish a connection to one another. Each program binds a socket to its end of the connection. To communicate, the client and the server each reads from and writes to the socket bound to the connection.

## [What Is a Socket?](definition.html)

A socket is one end-point of a two-way communication link between two programs running on the network. Socket classes are used to represent the connection between a client program and a server program. The java.net package provides two classes--Socket and ServerSocket--that implement the client side of the connection and the server side of the connection, respectively.

## [Reading from and Writing to a Socket](readingWriting.html)

This page contains a small example that illustrates how a client program can read from and write to a socket.

## [Writing a Client/Server Pair](clientServer.html)

The previous page showed an example of how to write a client program that interacts with an existing server via a Socket object. This page shows you how to write a program that implements the other side of the connection--a server program.
