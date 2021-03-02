
# Internationalized Domain Name

Historically, an Internet domain name contained ASCII symbols only.
As the Internet gained popularity and was adopted across the world, it became necessary to support internationalization of domain names, specifically to support domain names that include Unicode characters.

The **Internationalizing Domain Names** in Applications (IDNA) mechanism was adopted as the standard to convert Unicode characters to standard ASCII domain names and thus preserve the stability of the domain name system. This system performs a lookup service to translate user-friendly names into network addresses.

Examples of internationalized domain names:

- <tt>http://&#28165;&#21326;&#22823;&#23398;.cn</tt>
- <tt>http://www.&#1090;&#1088;&#1072;&#1085;&#1089;&#1087;&#1086;&#1088;&#1090;.com</tt>

If you follow these links you will see that the Unicode domain name represented in the address bar is substituted with the ASCII string.

To implement similar functionality in your application, the
[<tt>java.net.IDN</tt>](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html) class provides methods to convert domain names between ASCII and non ASCII formats.
<th id="h1">Method</th><th id="h2">Purpose</th>
<td headers="h1">[<tt>toASCII(String)</tt>](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html#toASCII-java.lang.String-)<br />[<tt>toASCII(String, flag)</tt>](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html#toASCII-java.lang.String-int-)</td><td headers="h2">Used before sending an IDN to the domain name resolving system or writing an IDN to a file where ASCII characters are expected, such as a DNS master file. If the input string doesn't conform to [RFC 3490](http://www.ietf.org/rfc/rfc3490.txt), these methods throw an <tt>IllegalArgumentException</tt>.</td>
<td headers="h1">[<tt>toUnicode(String)</tt>](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html#toUnicode-java.lang.String-)<br />[<tt>toUnicode(String, flag)</tt>](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html#toUnicode-java.lang.String-int-)</td><td headers="h2">Used when displaying names to users, for example names obtained from a DNS zone. This method translates a string from ASCII Compatible Encoding (ACE) to Unicode code points. This method never fails; in case of an error the input string remains the same and is returned unmodified.</td>

The optional <tt>flag</tt> parameter specifies the behavior of the conversion process.  The <tt>ALLOW_UNASSIGNED</tt> flag allows including code points that are unassigned in Unicode 3.2.  The <tt>USE_STD3_ASCII_RULES</tt> flag ensures that the STD-3 ASCII rules are observed.  You can use these flags separately or logically OR'ed together.  If neither flag is desired, use the single-parameter version of the method.
