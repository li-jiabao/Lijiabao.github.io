
# Standard LDAP Controls

In LDAP v3, a Control is a message that enhances the existing LDAP operation by associating it with more information useful to the server or the client. A control can be either a request control or a response control. A request control is sent from the client to the server along with an LDAP request. A response control is sent from the server to the client along with an LDAP response. Either is represented by the interface 
[<tt>javax.naming.ldap.Control</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Control.html).

See the controls lesson in the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/) if you haven't programmed using controls before.

In this lesson we will discuss the standard LDAP controls that are added to JDK 5.0. The necessary LDAP controls are already supported in the LDAP Booster Pack extension package available for the JNDI/LDAP service provider under <tt>com.sun.jndi.ldap.ctl</tt> package. The LDAP controls that are standardized by IETF are now made available in the <tt>javax.naming.ldap</tt> package of JDK through the following classes.

<li>
[<tt>ManageReferralControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/ManageReferralControl.html) (
[RFC 3296](http://www.ietf.org/rfc/rfc3296.txt)
)</li>
<li>
[<tt>PagedResultsControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/PagedResultsControl.html) (
[RFC 2696](http://www.ietf.org/rfc/rfc2696.txt)
)</li>
<li>
[<tt>PagedResultsResponseControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/PagedResultsResponseControl.html)</li>
<li>
[<tt>SortControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortControl.html) (
[RFC 2891](http://www.ietf.org/rfc/rfc2891.txt)
)</li>
<li>
[<tt>SortKey</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortKey.html)</li>
<li>
[<tt>SortResponseControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortResponseControl.html)</li>
<li>
[<tt>BasicControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/BasicControl.html)</li>
