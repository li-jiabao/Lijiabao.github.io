
# Lesson: Programmatic Access to Network Parameters

Systems often run with multiple active network connections, such as wired Ethernet, `802.11 b/g` (wireless), and bluetooth. Some applications might need to access this information to perform the particular network activity on a specific connection.

The 
[`java.net.NetworkInterface`](https://docs.oracle.com/javase/8/docs/api/java/net/NetworkInterface.html) class provides access to this information.

This lesson guides you through some of the more common uses of this class and provides examples that list all the network interfaces on a machine as well as their IP addresses and status.

## [What Is a Network Interface?](definition.html)

This page describes a network interface and explains why you might want to use it.

## [Retrieving Network Interfaces](retrieving.html)

This page contains an example that illustrates how a client program can retrieve all the network interfaces on a machine.

## [Listing Network Interface Addresses](listing.html)

This page shows you how to list the IP addresses assigned to all the network interfaces on a machine.

## [Network Interface Parameters](parameters.html)

This page shows you how to determine whether a network interface is running or if the network interface is a loopback interface, a point-to-point interface, or a virtual interface. You can also learn how to determine if the interface supports multicasting.
