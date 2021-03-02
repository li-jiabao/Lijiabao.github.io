
# Lesson: All About Datagrams

Some applications that you write to communicate over the network will not require the reliable, point-to-point channel provided by TCP. Rather, your applications might benefit from a mode of communication that delivers independent packages of information whose arrival and order of arrival are not guaranteed.

The UDP protocol provides a mode of network communication whereby applications send packets of data, called datagrams, to one another. A datagram is an independent, self-contained message sent over the network whose arrival, arrival time, and content are not guaranteed. The `DatagramPacket` and `DatagramSocket` classes in the `java.net` package implement system-independent datagram communication using UDP.

## [What Is a Datagram?](definition.html)

A datagram is an independent, self-contained message sent over the network whose arrival, arrival time, and content are not guaranteed.

## [Writing a Datagram Client and Server](clientServer.html)

This section walks you through an example that contains two Java programs that use datagrams to communicate. The server side is a quote server that listens to its `DatagramSocket` and sends a quotation to a client whenever the client requests it. The client side is a simple program that simply makes a request of the server.

## [Broadcasting to Multiple Recipients](broadcasting.html)

This section modifies the quote server so that instead of sending a quotation to a single client upon request, the quote server broadcasts a quote every minute to as many clients as are listening. The client program must be modified accordingly.

Many firewalls and routers are configured not to allow UDP packets. If you have trouble connecting to a service outside your firewall, or if clients have trouble connecting to your service, ask your system administrator if UDP is permitted.
