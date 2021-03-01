
# User Acceptance of RIAs

For security, users are prompted for permission to run an RIA before launching for the first time, even if the application is signed or doesn't require access outside of the security sandbox. The prompt includes the following information, depending on the RIA being run:

- Name of the RIA, or notification that the application is unsigned.
- Information about the publisher, if the app is signed with a certificate from a trusted authority. If the certificate is expired, a warning is included. If the application is self-signed, the publisher is shown as UNKNOWN.
- Warnings if the certificate is expired, revoked, or the revocation status cannot be checked. 
- Location from which the application is accessed.
- Level of access required by the application. Limited access restricts the application to the security sandbox, unrestricted access provides the application with access to resources on the user's system. 
- Warning about missing JAR file manifest attributes if recommended attributes are not present.
- For unsigned or self-signed applications, a checkbox that the user must select to accept the application.
- In some cases, the option to not show the prompt again.

For a description of the prompts, see 
[What should I do when I see a security prompt from Java? ](http://java.com/faq-securityprompts)

Users are also warned if they are running an out-of-date JRE and are given the opportunity to update to the latest version before running an application. Users can also choose to run with the JRE on their system, or to block the application from running.

The security level setting in the Java Control Panel determines if users are given the opportunity to run RIAs. The default setting of High prompts users for permission to run applications that are signed with a valid certificate and include the Permissions attribute in the manifest for the main JAR file. If the revocation status of an application cannot be checked, the application is also allowed to run with the user's permsision.

Signing your RIA provides the user with a level of trust. Consider the following when preparing your application for deployment:


<li>
The best user experience is provided by an application that is signed with a certificate issued by a recognized certificate authority. 
</li>
<li>
Self-signed and unsigned applications are not allowed to be run unless an exception site list or deployment rule set has been created to explicitly allow the application to run.
</li>
<li>
Signed applications can be either privileged applications or sandbox applications. Privileged applications are provided unrestricted access to resources on the user's system. Sandbox applications are restricted to the Java security sandbox. Unsigned applications are restricted to the sandbox.
</li>
