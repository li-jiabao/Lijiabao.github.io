
# Enabling the SDP Protocol

SDP support is disabled by default. To enable SDP support, set the `com.sun.sdp.conf` system property by providing the location of the configuration file. The following example starts an application using a configuration file named `sdp.conf`:

```

% java **-Dcom.sun.sdp.conf=sdp.conf** -Djava.net.preferIPv4Stack=true  **ExampleApplication**

```

**ExampleApplication** refers to the client application that is attempting to connect to the IB adaptor.

Note that this example specifies another system property, `java.net.preferIPv4Stack`. See the 
[Issues](issues.html) section for more information about why this property is used.
