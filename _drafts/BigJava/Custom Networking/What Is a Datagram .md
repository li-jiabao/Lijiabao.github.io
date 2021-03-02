
# Writing a Datagram Client and Server

The example featured in this section consists of two applications: a client and a server. The server continuously receives datagram packets over a datagram socket. Each datagram packet received by the server indicates a client request for a quotation. When the server receives a datagram, it replies by sending a datagram packet that contains a one-line "quote of the moment" back to the client.

The client application in this example is fairly simple. It sends a single datagram packet to the server indicating that the client would like to receive a quote of the moment. The client then waits for the server to send a datagram packet in response.

Two classes implement the server application: `QuoteServer` and `QuoteServerThread`. A single class implements the client application: `QuoteClient`.

Let's investigate these classes, starting with the class that contains the `main` method for the server application. 

[Working With a Server-Side Application ](../../deployment/applet/server.html)
 contains an applet version of the `QuoteClient` class.

## <a name="mainprogram" id="mainprogram">The QuoteServer Class</a>

The 
[`QuoteServer`](examples/QuoteServer.java) class, shown here in its entirety, contains a single method: the `main` method for the quote server application. The `main` method simply creates a new `QuoteServerThread` object and starts it:

```

import java.io.*;

public class QuoteServer {
    public static void main(String[] args) throws IOException {
        new QuoteServerThread().start();
    }
}

```

The `QuoteServerThread` class implements the main logic of the quote server.

## The `QuoteServerThread` Class

When created, the 
[`QuoteServerThread`](examples/QuoteServerThread.java) creates a `DatagramSocket` on port 4445 (arbitrarily chosen). This is the `DatagramSocket` through which the server communicates with all of its clients.

```

public QuoteServerThread() throws IOException {
    this("QuoteServer");
}

public QuoteServerThread(String name) throws IOException {
    super(name);
    socket = new DatagramSocket(4445);

    try {
        in = new BufferedReader(new FileReader("one-liners.txt"));
    }   
    catch (FileNotFoundException e){
        System.err.println("Couldn't open quote file.  Serving time instead.");
    }
}  

```

Remember that certain ports are dedicated to well-known services and you cannot use them. If you specify a port that is in use, the creation of the `DatagramSocket` will fail.

The constructor also opens a `BufferedReader` on a file named 
[`one-liners.txt`](examples/one-liners.txt)<!--
[one-liners.txt](examples/one-liners.txt) 
-->
which contains a list of quotes. Each quote in the file is on a line by itself.

Now for the interesting part of the `QuoteServerThread`: its `run` method. The `run` method overrides `run` in the `Thread` class and provides the implementation for the thread. For information about threads, see 
[Defining and Starting a Thread](../../essential/concurrency/runthread.html).

The `run` method contains a `while` loop that continues as long as there are more quotes in the file. During each iteration of the loop, the thread waits for a `DatagramPacket` to arrive over the `DatagramSocket`. The packet indicates a request from a client. In response to the client's request, the `QuoteServerThread` gets a quote from the file, puts it in a `DatagramPacket` and sends it over the `DatagramSocket` to the client that asked for it.

Let's look first at the section that receives the requests from clients:

```

byte[] buf = new byte[256];
DatagramPacket packet = new DatagramPacket(buf, buf.length);
socket.receive(packet);

```

The first statement creates an array of bytes which is then used to create a `DatagramPacket`. The `DatagramPacket` will be used to receive a datagram from the socket because of the constructor used to create it. This constructor requires only two arguments: a byte array that contains client-specific data and the length of the byte array. When constructing a `DatagramPacket` to send over the `DatagramSocket`, you also must supply the Internet address and port number of the packet's destination. You'll see this later when we discuss how the server responds to a client request.

The last statement in the previous code snippet receives a datagram from the socket (the information received from the client gets copied into the packet). The receive method waits forever until a packet is received. If no packet is received, the server makes no further progress and just waits.

Now assume that, the server has received a request from a client for a quote. Now the server must respond. This section of code in the run method constructs the response:

```

String dString = null;
if (in == null)
    dString = new Date().toString();
else
    dString = getNextQuote();
buf = dString.getBytes();

```

If the quote file did not get opened for some reason, then `in` equals null. If this is the case, the quote server serves up the time of day instead. Otherwise, the quote server gets the next quote from the already opened file. Finally, the code converts the string to an array of bytes.

Now, the `run` method sends the response to the client over the `DatagramSocket` with this code:

```

InetAddress address = packet.getAddress();
int port = packet.getPort();
packet = new DatagramPacket(buf, buf.length, address, port);
socket.send(packet);

```

The first two statements in this code segment get the Internet address and the port number, respectively, from the datagram packet received from the client. The Internet address and port number indicate where the datagram packet came from. This is where the server must send its response. In this example, the byte array of the datagram packet contains no relevant information. The arrival of the packet itself indicates a request from a client that can be found at the Internet address and port number indicated in the datagram packet.

The third statement creates a new `DatagramPacket` object intended for sending a datagram message over the datagram socket. You can tell that the new `DatagramPacket` is intended to send data over the socket because of the constructor used to create it. This constructor requires four arguments. The first two arguments are the same required by the constructor used to create receiving datagrams: a byte array containing the message from the sender to the receiver and the length of this array. The next two arguments are different: an Internet address and a port number. These two arguments are the complete address of the destination of the datagram packet and must be supplied by the sender of the datagram. The last line of code sends the `DatagramPacket` on its way.

When the server has read all the quotes from the quote file, the `while` loop terminates and the `run` method cleans up:

```

socket.close();

```

## The QuoteClient Class

The 
[`QuoteClient`](examples/QuoteClient.java) class implements a client application for the `QuoteServer`. This application sends a request to the `QuoteServer`, waits for the response, and, when the response is received, displays it to the standard output. Let's look at the code in detail.

The `QuoteClient` class contains one method, the `main` method for the client application. The top of the `main` method declares several local variables for its use:

```

int port;
InetAddress address;
DatagramSocket socket = null;
DatagramPacket packet;
byte[] sendBuf = new byte[256];

```

First, the `main` method processes the command-line arguments used to invoke the `QuoteClient` application:

```

if (args.length != 1) {
    System.out.println("Usage: java QuoteClient &lt;hostname&gt;");
    return;
}

```

The `QuoteClient` application requires one command-line arguments: the name of the machine on which the `QuoteServer` is running.

Next, the `main` method creates a `DatagramSocket`:

```

DatagramSocket socket = new DatagramSocket();

```

The client uses a constructor that does not require a port number. This constructor just binds the `DatagramSocket` to any available local port. It doesn't matter what port the client is bound to because the `DatagramPacket`s contain the addressing information. The server gets the port number from the `DatagramPacket`s and send its response to that port.

Next, the `QuoteClient` program sends a request to the server:

```

byte[] buf = new byte[256];
InetAddress address = InetAddress.getByName(args[0]);
DatagramPacket packet = new DatagramPacket(buf, buf.length, 
                                address, 4445);
socket.send(packet);

```

The code segment gets the Internet address for the host named on the command line (presumably the name of the machine on which the server is running). This `InetAddress` and the port number 4445 (the port number that the server used to create its `DatagramSocket`) are then used to create `DatagramPacket` destined for that Internet address and port number. Therefore the `DatagramPacket` will be delivered to the quote server.

Note that the code creates a `DatagramPacket` with an empty byte array. The byte array is empty because this datagram packet is simply a request to the server for information. All the server needs to know to send a response--the address and port number to which reply--is automatically part of the packet.

Next, the client gets a response from the server and displays it:

```

packet = new DatagramPacket(buf, buf.length);
socket.receive(packet);
String received = new String(packet.getData(), 0, packet.getLength());
System.out.println("Quote of the Moment: " + received);

```

To get a response from the server, the client creates a "receive" packet and uses the `DatagramSocket` receive method to receive the reply from the server. The receive method waits until a datagram packet destined for the client comes through the socket. Note that if the server's reply is somehow lost, the client will wait forever because of the no-guarantee policy of the datagram model. Normally, a client sets a timer so that it doesn't wait forever for a reply; if no reply arrives, the timer goes off and the client retransmits.

When the client receives a reply from the server, the client uses the getData method to retrieve that data from the packet. The client then converts the data to a string and displays it.

## Running the Server and Client

After you've successfully compiled the server and the client programs, you run them. You have to run the server program first. Just use the Java interpreter and specify the `QuoteServer` class name.

Once the server has started, you can run the client program. Remember to run the client program with one command-line argument: the name of the host on which the `QuoteServer` is running.

After the client sends a request and receives a response from the server, you should see output similar to this:

```

Quote of the Moment:
Good programming is 99% sweat and 1% coffee.

```
