
# Background


The JAXP secure-processing feature places resource limits on XML processors in order to counter certain type of denial-of-service attacks. It does not, however, limit the means by which external resources may be fetched, which is also useful when attempting to process XML documents securely. The current JAXP implementation supports implementation-specific properties that can be used to enforce such restrictions, but there is a need for a standard way to do so.


JAXP 1.5 adds three new properties along with their corresponding System Properties to allow users to specify the type of external connections that can or can not be permitted. The property values are a list of protocols. The JAXP processors check if a given external connection is allowed by matching the protocol with those in the list. Processors will attempt to establish the connection if it is on the list, or reject it if not.


JAXP 1.5 has been integrated into 7u40 and JDK8.
