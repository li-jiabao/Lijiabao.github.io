
# What Is a Datagram?

Clients and servers that communicate via a reliable channel, such as a TCP socket, have a dedicated point-to-point channel between themselves, or at least the illusion of one. To communicate, they establish a connection, transmit the data, and then close the connection. All data sent over the channel is received in the same order in which it was sent. This is guaranteed by the channel.

In contrast, applications that communicate via datagrams send and receive completely independent packets of information. These clients and servers do not have and do not need a dedicated point-to-point channel. The delivery of datagrams to their destinations is not guaranteed. Nor is the order of their arrival.

A **datagram** is an independent, self-contained message sent over the network whose arrival, arrival time, and content are not guaranteed.

The `java.net` package contains three classes to help you write Java programs that use datagrams to send and receive packets over the network: 
[`DatagramSocket`](https://docs.oracle.com/javase/8/docs/api/java/net/DatagramSocket.html), 
[`DatagramPacket`](https://docs.oracle.com/javase/8/docs/api/java/net/DatagramPacket.html), and 
[`MulticastSocket`](https://docs.oracle.com/javase/8/docs/api/java/net/MulticastSocket.html)An application can send and receive `DatagramPacket`s through a `DatagramSocket`. In addition, `DatagramPacket`s can be broadcast to multiple recipients all listening to a `MulticastSocket`.
