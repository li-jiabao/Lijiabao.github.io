
# Network Interface Parameters

You can access network parameters about a network interface beyond the name and IP addresses assigned to it

You can discover if a network interface is &#8220;up&#8221; (that is, running) with the `isUP()` method. The following methods indicate the network interface type:

- `isLoopback()` indicates if the network interface is a loopback interface.
- `isPointToPoint()` indicates if the interface is a point-to-point interface.
- `isVirtual()` indicates if the interface is a virtual interface.

The `supportsMulticast()` method indicates whether the network interface supports multicasting. The `getHardwareAddress()` method returns the network interface's physical hardware address, usually called MAC address, when it is available. The `getMTU()` method returns the Maximum Transmission Unit (MTU), which is the largest packet size.

The following example expands on the example in 
[Listing Network Interface Addresses](listing.html) by adding the additional network parameters described on this page:

```

import java.io.*;
import java.net.*;
import java.util.*;
import static java.lang.System.out;

public class ListNetsEx {

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
       
        out.printf("Up? %s\n", netint.isUp());
        out.printf("Loopback? %s\n", netint.isLoopback());
        out.printf("PointToPoint? %s\n", netint.isPointToPoint());
        out.printf("Supports multicast? %s\n", netint.supportsMulticast());
        out.printf("Virtual? %s\n", netint.isVirtual());
        out.printf("Hardware address: %s\n",
                    Arrays.toString(netint.getHardwareAddress()));
        out.printf("MTU: %s\n", netint.getMTU());
        out.printf("\n");
     }
}  

```

The following is sample output from the example program:

```

Display name: bge0
Name: bge0
InetAddress: /fe80:0:0:0:203:baff:fef2:e99d%2
InetAddress: /129.156.225.59
Up? true
Loopback? false
PointToPoint? false
Supports multicast? false
Virtual? false
Hardware address: [0, 3, 4, 5, 6, 7]
MTU: 1500

Display name: lo0
Name: lo0
InetAddress: /0:0:0:0:0:0:0:1%1
InetAddress: /127.0.0.1
Up? true
Loopback? true
PointToPoint? false
Supports multicast? false
Virtual? false
Hardware address: null
MTU: 8232

```
