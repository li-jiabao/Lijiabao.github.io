
# Trail: RMI

The Java Remote Method Invocation (RMI) system allows an object running in one Java virtual machine to invoke methods on an object running in another Java virtual machine. RMI provides for remote communication between programs written in the Java programming language.

This trail provides a brief overview of the RMI system and then walks through a complete client/server example that uses RMI's unique capabilities to load and to execute user-defined tasks at runtime. The server in the example implements a generic compute engine, which the client uses to compute the value of 
<img src="../figures/rmi/pi.gif " width="9 " height="9  " alt="the pi symbol" />.

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **An Overview of RMI Applications**](overview.html) describes the RMI system and lists its advantages. Additionally, this section provides a description of a typical RMI application, composed of a server and a client, and introduces important terms.

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **Writing an RMI Server**](server.html) walks through the code for the compute engine server. This section will teach you how to design and to implement an RMI server.

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **Creating A Client Program**](client.html) takes a look at one possible compute engine client and uses it to illustrate the important features of an RMI client.

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **Compiling and Running the Example**](example.html) shows you how to compile and to run both the compute engine server and its client.
