
# Technical Issues with SDP

<li>IPv4 and IPv6 incompatibility
Internet Protocol version 4 (IPv4) has long been the industry standard version of the Internet Protocol (IP) for delivering data over the Internet. Internet Protocol version 6 (IPv6) is the next generation Internet layer protocol. Both versions of IP are in use today.
IPv4 addresses are 32-bits long, written in decimal format, and separated by periods. IPv6 addresses are 128-bits long, written in hexadecimal format, and separated by colons. IPv4 addresses cannot be used as is in IPv6, but IPv6 does support a special class of addresses: the IPv4-mapped address. In an IPv4-mapped address, the first 80 bits are set to zero, the next 16 bits are set to 1, and the last 32 bits represent the IPv4 address.
For example, here is the same IP address expressed in both formats:
<pre><code>
**IPv4 address                  IPv4-mapped address** (for use in IPv6)
192.0.2.1                   ::ffff:192.0.2.1
</code></pre>
By default, if IPv6 is enabled on any of the IB adaptors, the Java platform uses IPv6. However, IPv4-mapped addresses are not currently available in the Solaris OS or under Linux. For this reason, if you want to use the IPv4 address format under JDK 7, you must specify the `java.net.preferIPv4Stack` property, as shown in this example:
<pre><code>
% java -Dcom.sun.sdp.conf=sdp.conf **-Djava.net.preferIPv4Stack=true**  **MyApplication**
</code></pre>
</li>
<li>Bugs
A few bugs were found in the early InfiniBand implementation. These bugs are fixed in the Solaris 10 10/09 release. Make sure that you are using at least this release.
</li>
