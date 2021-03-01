
# Listing Network Interface Addresses

One of the most useful pieces of information you can get from a network interface is the list of IP addresses that are assigned to it. You can obtain this information from a `NetworkInterface` instance by using one of two methods. The first method, `getInetAddresses()`, returns an `Enumeration` of `InetAddress`. The other method, `getInterfaceAddresses()`, returns a list of 
[`java.net.InterfaceAddress`](https://docs.oracle.com/javase/8/docs/api/java/net/InterfaceAddress.html) instances. This method is used when you need more information about an interface address beyond its IP address. For example, you might need additional information about the subnet mask and broadcast address when the address is an IPv4 address, and a network prefix length in the case of an IPv6 address.

The following example program lists all the network interfaces and their addresses on a machine:

```

import java.io.*;
import java.net.*;
import java.util.*;
import static java.lang.System.out;

public class ListNets {

    public static void main(String args[]) throws SocketException {
        Enumeration&lt;NetworkInterface&gt; nets = NetworkInterface.getNetworkInterfaces();
        for (NetworkInterface netint : Collections.list(nets))
            displayInterfaceInformation(netint);
    }

    static void displayInterfaceInformation(NetworkInterface netint) throws SocketException {
        out.printf("Display name: %s\n", netint.getDisplayName());
        out.printf("Name: %s\n", netint.getName());
        Enumeration&lt;InetAddress&gt; inetAddresses = netint.getInetAddresses();
        for (InetAddress inetAddress : Collections.list(inetAddresses)) {
            out.printf("InetAddress: %s\n", inetAddress);
        }
        out.printf("\n");
     }
}  

```

The following is sample output from the example program:

```

Display name: TCP Loopback interface
Name: lo
InetAddress: /127.0.0.1

Display name: Wireless Network Connection
Name: eth0
InetAddress: /192.0.2.0

```
