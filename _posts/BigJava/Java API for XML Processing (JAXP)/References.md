
# Lesson: Processing Limits


XML processing can sometimes be a memory intensive operation. Applications, especially those that accept XML, XSD and XSL from untrusted sources, should take steps to guard against excessive memory consumption by using the JAXP processing limits provided in the JDK.


Developers should evaluate their application's requirements and operating environment to determine the acceptable limits for their system configurations and set these limits accordingly. The size related limits can be used to prevent malformed XML sources from being processed without consuming large amount of memory, while an <tt>EntityExpansionLimit</tt> will allow an application to control memory consumption under an acceptable level.


In this tutorial, you are introduced to the limits, and you will learn how to use them properly.
