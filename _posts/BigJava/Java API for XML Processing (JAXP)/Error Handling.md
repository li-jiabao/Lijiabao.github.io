
# StAX


StAX, JSR 173, does not support FSP. The StAX implementation in the JDK however, supports the new limit properties and their corresponding system properties. This means that, while there is no FSP to turn on and off the limits, the limits and system properties described work exactly the same.


For compatibility, the StAX specific properties always take effect before the new JAXP limits. The <tt>SupportDTD</tt> property when set to false, for example, causes an exception to be thrown when an input file contains an <tt>Entity</tt> reference. Therefore, the addition of the new limits will have no effect for applications that use the <tt>SupportDTD</tt> property to disable the DTD.
