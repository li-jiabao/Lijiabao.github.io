
# Retrieving Network Interfaces

The `NetworkInterface` class has no public constructor. Therefore, you cannot just create a new instance of this class with the `new` operator. Instead, the following static methods are available so that you can retrieve the interface details from the system: `getByInetAddress()`, `getByName()`, and `getNetworkInterfaces()`. The first two methods are used when you already know the IP address or the name of the particular interface. The third method, `getNetworkInterfaces()` returns the complete list of interfaces on the machine.

Network interfaces can be hierarchically organized. The `NetworkInterface` class includes two methods, `getParent()` and `getSubInterfaces()`, that are pertinent to a network interface hierarchy. The `getParent()` method returns the parent `NetworkInterface` of an interface. If a network interface is a subinterface, `getParent()` returns a non-null value. The `getSubInterfaces()` method returns all the subinterfaces of a network interface.

The following example program lists the name of all the network interfaces and subinterfaces (if any exist) on a machine.

```

import java.io.*;
import java.net.*;
import java.util.*;
import static java.lang.System.out;

public class ListNIFs 
{
    public static void main(String args[]) throws SocketException {
        Enumeration&lt;NetworkInterface&gt; nets = NetworkInterface.getNetworkInterfaces();
        
        for (NetworkInterface netIf : Collections.list(nets)) {
            out.printf("Display name: %s\n", netIf.getDisplayName());
            out.printf("Name: %s\n", netIf.getName());
            displaySubInterfaces(netIf);
            out.printf("\n");
        }
    }

    static void displaySubInterfaces(NetworkInterface netIf) throws SocketException {
        Enumeration&lt;NetworkInterface&gt; subIfs = netIf.getSubInterfaces();
        
        for (NetworkInterface subIf : Collections.list(subIfs)) {
            out.printf("\tSub Interface Display name: %s\n", subIf.getDisplayName());
            out.printf("\tSub Interface Name: %s\n", subIf.getName());
        }
     }
}  

```

The following is sample output from the example program:

```

Display name: bge0
Name: bge0
    Sub Interface Display name: bge0:3
    Sub Interface Name: bge0:3
    Sub Interface Display name: bge0:2
    Sub Interface Name: bge0:2
    Sub Interface Display name: bge0:1
    Sub Interface Name: bge0:1

Display name: lo0
Name: lo0

```
