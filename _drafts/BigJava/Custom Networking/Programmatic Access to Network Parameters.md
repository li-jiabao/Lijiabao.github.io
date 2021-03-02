
# What Is a Network Interface?

A *network interface* is the point of interconnection between a computer and a private or public network. A network interface is generally a network interface card (NIC), but does not have to have a physical form. Instead, the network interface can be implemented in software. For example, the loopback interface (<tt>127.0.0.1</tt> for IPv4 and <tt>::1</tt> for IPv6) is not a physical device but a piece of software simulating a network interface. The loopback interface is commonly used in test environments.

The 
[`java.net.NetworkInterface`](https://docs.oracle.com/javase/8/docs/api/java/net/NetworkInterface.html) class represents both types of interfaces.

<tt>NetworkInterface</tt> is useful for a multi-homed system, which is a system with multiple NICs. Using `NetworkInterface`, you can specify which NIC to use for a particular network activity.

For example, assume you have a machine with two configured NICs, and you want to send data to a server. You create a socket like this:

```

Socket soc = new java.net.Socket();
soc.connect(new InetSocketAddress(address, port));

```

To send the data, the system determines which interface is used. However, if you have a preference or otherwise need to specify which NIC to use, you can query the system for the appropriate interfaces and find an address on the interface you want to use. When you create the socket and bind it to that address, the system uses the associated interface. For example:

```

NetworkInterface nif = NetworkInterface.getByName("bge0");
Enumeration&lt;InetAddress&gt; nifAddresses = nif.getInetAddresses();

Socket soc = new java.net.Socket();
soc.bind(new InetSocketAddress(nifAddresses.nextElement(), 0));
soc.connect(new InetSocketAddress(address, port));

```

You can also use `NetworkInterface` to identify the local interface on which a multicast group is to be joined. For example:

```

NetworkInterface nif = NetworkInterface.getByName("bge0");
MulticastSocket ms = new MulticastSocket();
ms.joinGroup(new InetSocketAddress(hostname, port), nif);

```

NetworkInterface can be used with Java APIs in many other ways beyond the two uses described here.
