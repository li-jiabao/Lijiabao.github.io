
# What Applets Can and Cannot Do

Java applets are loaded on a client when the user visits a page containing an applet. The security model behind Java applets has been designed with the goal of protecting the user from malicious applets. 

Applets are either sandbox applets or privileged applets. Sandbox applets are run in a security sandbox that allows only a set of safe operations. Privileged applets can run outside the security sandbox and have extensive capabilities to access the client.

Applets that are not signed are restricted to the security sandbox, and run only if the user accepts the applet. Applets that are signed by a certificate from a recognized certificate authority can either run only in the sandbox, or can request permission to run outside the sandbox. In either case, the user must accept the applet's security certificate, otherwise the applet is blocked from running.

It is recommended that you launch your applet using Java Network Launch Protocol (JNLP) to leverage expanded capabilities and improve user experience. See 
[Deploying an Applet](deployingApplet.html) for step by step instructions on applet deployment.

It is recommended that you deploy your applets to a web server, even for testing. To run applets locally, add the applets to the exception site list, which is managed from the Security tab of the Java Control Panel.

In this topic we will discuss the security restrictions and capabilities of applets.

## Sandbox Applets

Sandbox applets are restricted to the security sandbox and **can** perform the following operations:

- They can make network connections to the host and port they came from. Protocols must match, and if a domain name is used to load the applet, the domain name must be used to connect back to the host, not the IP address.
- They can easily display HTML documents using the `showDocument` method of the `java.applet.AppletContext` class.
- They can invoke public methods of other applets on the same page.
- Applets that are loaded from the local file system (from a directory in the user's `CLASSPATH`) have none of the restrictions that applets loaded over the network do.
<li>They can read secure system properties. See 
[System Properties](../doingMoreWithRIA/properties.html) for a list of secure system properties.</li>
<li>When launched by using JNLP, sandbox applets can also perform the following operations:
<ul>
- They can open, read, and save files on the client.
- They can access the shared system-wide clipboard.
- They can access printing functions.
<li>They can store data on the client, decide how applets should be downloaded and cached, and much more. See 
[JNLP API](../doingMoreWithRIA/jnlpAPI.html) for more information about developing applets by using the JNLP API.</li>

Sandbox applets **cannot** perform the following operations:

- They cannot access client resources such as the local filesystem, executable files, system clipboard, and printers.
- They cannot connect to or retrieve resources from any third party server (any server other than the server it originated from).
- They cannot load native libraries.
- They cannot change the SecurityManager.
- They cannot create a ClassLoader.
<li>They cannot read certain system properties. See 
[System Properties](../doingMoreWithRIA/properties.html) for a list of forbidden system properties.</li>

## Privileged applets

Privileged applets do not have the security restrictions that are imposed on sandbox applets and can run outside the security sandbox.

See 
[Security in Rich Internet Applications ](../doingMoreWithRIA/security.html) for information on how to work with applets.

## Additional Information

For more information about applet security dialog boxes, see 
[Exploring Security Warning Functionality](http://www.oracle.com/technetwork/articles/javase/appletwarning-135102.html) (article on oracle.com/technetwork)
