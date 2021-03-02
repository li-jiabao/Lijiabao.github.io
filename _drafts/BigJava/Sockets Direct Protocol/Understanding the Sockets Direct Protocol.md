
# Overview

SDP support is essentially a TCP bypass technology.

When SDP is enabled and an application attempts to open a TCP connection, the TCP mechanism is bypassed and communication goes directly to the IB network. For example, when your application attempts to bind to a TCP address, the underlying software will decide, based on information in the configuration file, if it should be rebound to an SDP protocol. This process can happen during the binding process or the connecting process (but happens only once for each socket).

There are no API changes required in your code to take advantage of the SDP protocol: the implementation is transparent and is supported by the classic networking (`java.net`) and the New I/O (`java.nio.channels`) packages. See the 
[Supported Java APIs](supported.html) section for a list of classes that support the SDP protocol.

SDP support is disabled by default. The steps to enable SDP support are:

- Create an SDP configuration file.
- Set the system property that specifies the location of the configuration file.
