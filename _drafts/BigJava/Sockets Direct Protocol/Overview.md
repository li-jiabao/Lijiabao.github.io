
# Creating an SDP Configuration File

An SDP configuration file is a text file, and you decide where on the file system this file will reside. Every line in the configuration file is either a comment or a rule. A comment is indicated by the hash character (#) at the beginning of the line, and everything following the hash character will be ignored.

There are two types of rules, as follows:

- A "bind" rule indicates that the SDP protocol transport should be used when a TCP socket binds to an address and port that match the rule.
- A "connect" rule indicates that the SDP protocol transport should be used when an unbound TCP socket attempts to connect to an address and port that match the rule.

A rule has the following form:

```

("bind"|"connect")1*LWSP-char(hostname|ipaddress)["/"prefix])1*LWSP-char("*"|port)["-"("*"|port)]

```

**1*LWSP-char** means that any number of linear whitespace characters (tabs or spaces) can separate the tokens. The square brackets indicate optional text. The notation (**xxx** | **yyy**) indicates that the token will include either **xxx** or **yyy**, but not both. Quoted characters indicate literal text.

The first keyword indicates whether the rule is a **bind** or a **connect** rule. The next token specifies either a host name or a literal IP address. When you specify a literal IP address, you can also specify a prefix, which indicates an IP address range. The third and final token is a port number or a range of port numbers.

Consider the following notation in this sample configuration file:

```

# Use SDP when binding to 192.0.2.1
bind 192.0.2.1 *

# Use SDP when connecting to all application services on 192.0.2.*
connect 192.0.2.0/24     1024-*

# Use SDP when connecting to the http server or a database on examplecluster
connect examplecluster.example.com   80
connect examplecluster.example.com   3306

```

The first rule in the sample file specifies that SDP is used for any port (*) on the local IP address `192.0.2.1`. You would add a bind rule for each local address assigned to an InfiniBand adaptor. (An **InfiniBand adaptor** is the equivalent of a network interface card (NIC) for InfiniBand.) If you had several IB adaptors, you would use a bind rule for each address that is assigned to those adaptors.

The second rule in the sample file specifies that whenever connecting to `192.0.2.*` and the target port is 1024 or greater, SDP is used. The prefix on the IP address `/24` indicates that the first 24 bits of the 32-bit IP address should match the specified address. Each portion of the IP address uses 8 bits, so 24 bits indicates that the IP address should match `192.0.2` and the final byte can be any value. The `-*` notation on the port token specifies "and above." A range of ports, such as 1024&#8212;2056, would also be valid and would include the end points of the specified range.

The final rules in the sample file specify a host name (`examplecluster`), first with the port assigned to an http server (80) and then with the port assigned to a database (3306). Unlike a literal IP address, a host name can translate into multiple addresses. When you specify a host name, it matches all addresses that the host name is registered to in the name service.
