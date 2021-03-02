
# Structure of the JNLP File

This topic describes the syntax of the Java Network Launch Protocol (JNLP) file for rich Internet applications (RIAs).

The following code snippet shows a sample JNLP file for a Java Web Start application:

```

&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;jnlp spec="1.0+" codebase="" href=""&gt;
    &lt;information&gt;
        &lt;title&gt;Dynamic Tree Demo&lt;/title&gt;
        &lt;vendor&gt;Dynamic Team&lt;/vendor&gt;
        &lt;icon href="sometree-icon.jpg"/&gt;
        &lt;offline-allowed/&gt;
    &lt;/information&gt;
    &lt;resources&gt;
        &lt;!-- Application Resources --&gt;
        &lt;j2se version="1.6+" href=
           "http://java.sun.com/products/autodl/j2se"/&gt;
        &lt;jar href="DynamicTreeDemo.jar"
            main="true" /&gt;

    &lt;/resources&gt;
    &lt;application-desc
         name="Dynamic Tree Demo Application"
         main-class="webstartComponentArch.DynamicTreeApplication"
         width="300"
         height="300"&gt;
     &lt;/application-desc&gt;
     &lt;update check="background"/&gt;
&lt;/jnlp&gt;

```

The following table describes the elements and attributes commonly used in JNLP files. Click the parent link to view an element's parent.
<th id="h1" width="386">Element</th><th id="h2" width="95">Attributes</th><th id="h3" width="435">Description</th><th id="h4" width="63">Since</th><th id="h5" width="62">Required</th>

Attributes

Since
<td headers="h1"><a name="jnlp" id="jnlp"></a>jnlp</td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">The topmost xml element for a JNLP file.</td><td headers="h4" width="63">1.0<br /></td><td headers="h5" width="62">Yes</td>

<br />

1.0<br />
<td headers="h1"><br /></td><td headers="h2" width="95">spec</td><td headers="h3" width="435">Value of the attribute can be 1.0, 1.5, or 6.0, or can use wildcards such as 1.0+. It denotes the minimum version of the JNLP Specification that this JNLP file can work with.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

spec

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">codebase</td><td headers="h3" width="435">The base location for all relative URLs specified in `href` attributes in the JNLP file.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

codebase

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">href</td><td headers="h3" width="435">The URL of the JNLP file itself.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

href

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">version</td><td headers="h3" width="435">The version of the RIA being launched, as well as the version of the JNLP file itself.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

version

1.0
<td headers="h1">&#160;&#160;&#160;&#160; [parent](#jnlp)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Contains other elements that describe the RIA and its source.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">os</td><td headers="h3" width="435">The operating system for which this information element should be considered.</td><td headers="h4" width="63">1.5.0<br /><br /></td><td headers="h5" width="62"><br /></td>

os

1.5.0<br />
<br />
<td headers="h1"><br /></td><td headers="h2" width="95">arch</td><td headers="h3" width="435">The architecture for which this information element should be considered.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

arch

1.5.0
<td headers="h1"><br /></td><td headers="h2" width="95">platform</td><td headers="h3" width="435">The platform for which this information element should be considered.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

platform

1.5.0
<td headers="h1"><br /></td><td headers="h2" width="95">locale</td><td headers="h3" width="435">The locale for which this information element should be considered.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

locale

1.5.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; title <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">The title of the RIA.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; vendor <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">The provider of the RIA.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; homepage <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">The homepage of the RIA.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">href</td><td headers="h3" width="435">A URL pointing to where more information about this RIA can be found.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

href

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; description <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">A short statement describing the RIA.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">kind</td><td headers="h3" width="435">An indicator as to the type of description. Legal values are one-line, short, and tooltip.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

kind

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; icon <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">An icon that can be used to identify the RIA to the user.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">href</td><td headers="h3" width="435">A URL pointing to the icon file. Can be in one of the following formats: gif, jpg, png, ico.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

href

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">kind</td><td headers="h3" width="435">Indicates the suggested use of the icon, can be: default, selected, disabled, rollover, splash, or shortcut.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

kind

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">width</td><td headers="h3" width="435">Can be used to indicate the resolution of the image.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

width

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">height</td><td headers="h3" width="435">Can be used to indicate the resolution of the image.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

height

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">depth</td><td headers="h3" width="435">Can be used to indicate the resolution of the image.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

depth

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; offline-allowed <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Indicates that this RIA can operate when the client system is disconnected from the network.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; shortcut <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Can be used to indicate the RIA's preferences for desktop integration.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

<br />

1.5.0
<td headers="h1"><br /></td><td headers="h2" width="95">online</td><td headers="h3" width="435">Can be used to describe the RIA's preference for creating a shortcut to run online or offline.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

online

1.5.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; desktop <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Can be used to indicate the RIA's preference for putting a shortcut on the user's desktop.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

<br />

1.5.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; menu <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Can be used to indicate the RIA's preference for putting a menu item in the user's start menus.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

<br />

1.5.0
<td headers="h1"><br /></td><td headers="h2" width="95">sub-menu</td><td headers="h3" width="435">Can be used to indicate the RIA's preference for where to place the menu item.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

sub-menu

1.5.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; association <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Can be used to hint to the JNLP client that the RIA wants to be registered with the operating system as the primary handler of certain extensions and a certain mime-type. If this element is included, either the offline-allowed element must also be included, or the href attribute must be set for the jnlp element.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

<br />

1.5.0
<td headers="h1"><br /></td><td headers="h2" width="95">extensions</td><td headers="h3" width="435">A list of file extensions (separated by spaces) that the RIA requests it be registered to handle.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

extensions

1.5.0
<td headers="h1"><br /></td><td headers="h2" width="95">mime-type</td><td headers="h3" width="435">The mime-type that the RIA requests it be registered to handle.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

mime-type

1.5.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; related-content <sup>[parent](#information)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">An additional piece of related content that can be integrated with the RIA.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62"><br /></td>

<br />

1.5.0
<td headers="h1"><br /></td><td headers="h2" width="95">href</td><td headers="h3" width="435">A URL pointing to the related content.</td><td headers="h4" width="63">1.5.0</td><td headers="h5" width="62">Yes</td>

href

1.5.0
<td headers="h1">&#160;&#160;&#160;&#160;update <sup>[parent](#jnlp)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">The preferences for how RIA updates should be handled by the JNLP client.</td><td headers="h4" width="63">1.6.0</td><td headers="h5" width="62"><br /></td>

<br />

1.6.0
<td headers="h1"><br /></td><td headers="h2" width="95">check</td><td headers="h3" width="435">The preference for when the JNLP client should check for updates. Value can be always, timeout, or background..</td><td headers="h4" width="63">1.6.0</td><td headers="h5" width="62"><br /></td>

check

1.6.0
<td headers="h1"><br /></td><td headers="h2" width="95">policy</td><td headers="h3" width="435">The preference for how the JNLP client should handle a RIA update when a new version is available before the RIA is launched. Values can be always, prompt-update, or prompt-run.</td><td headers="h4" width="63">1.6.0</td><td headers="h5" width="62"><br /></td>

policy

1.6.0
<td headers="h1"><br /></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435"><br /></td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;[parent](#jnlp)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Can be used to request enhanced permissions. If this element is not included, the application is run in the security sandbox.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; all-permissions <sup>[parent](#security)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Requests that the RIA be run with all permissions.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; j2ee-application-client-permissions <sup>[parent](#security)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Requests that the RIA be run with a permission set that meets the security specifications of the J2EE application client environment.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;[parent](#jnlp)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Describes all the resources that are needed for the RIA.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">os</td><td headers="h3" width="435">The operating system for which the resources element should be considered.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

os

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">arch</td><td headers="h3" width="435">The architecture for which the resources element should be considered.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

arch

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">locale</td><td headers="h3" width="435">The locales for which the resources element should be considered.</td><td headers="h4" width="63"><br /></td><td headers="h5" width="62"><br /></td>

locale

<br />
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; java or j2se <sup>[parent](#resources)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Versions of Java software to run the RIA with.</td><td headers="h4" width="63">1.6.0 (java)<br /></td><td headers="h5" width="62"><br /></td>

<br />

1.6.0 (java)<br />
<td headers="h1"><br /></td><td headers="h2" width="95">version</td><td headers="h3" width="435">Ordered list of version ranges to use.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

version

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">href</td><td headers="h3" width="435">The URL denoting the supplier of this version of Java software, and from where it can be downloaded.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

href

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">java-vm-args</td><td headers="h3" width="435">An additional set of standard and non-standard virtual machine arguments that the RIA would prefer the JNLP client use when launching the JRE software.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

java-vm-args

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">initial-heap-size</td><td headers="h3" width="435">The initial size of the Java heap.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

initial-heap-size

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">max-heap-size</td><td headers="h3" width="435">The maximum size of the Java heap.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

max-heap-size

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; jar <sup>[parent](#resources)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">A JAR file that is part of the RIA's classpath.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">href</td><td headers="h3" width="435">The URL of the JAR file.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

href

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">version</td><td headers="h3" width="435">The requested version of the JAR file. Requires using the version-based download protocol</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

version

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">main</td><td headers="h3" width="435">Indicates if this JAR file contains the class containing the `main` method of the RIA.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

main

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">download</td><td headers="h3" width="435">Indicates that this JAR file can be downloaded lazily, or when needed.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

download

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">size</td><td headers="h3" width="435">The downloadable size of the JAR file in bytes.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

size

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">part</td><td headers="h3" width="435">Can be used to group resources together so that they are downloaded at the same time.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

part

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; nativelib <sup>[parent](#resources)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">A JAR file that contains native libraries in its root directory.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">href</td><td headers="h3" width="435">The URL of the JAR file.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

href

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">version</td><td headers="h3" width="435">The requested version of the JAR file. Requires using the version-based download protocol</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

version

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">download</td><td headers="h3" width="435">Can be used to indicate this JAR file can be downloaded lazily.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

download

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">size</td><td headers="h3" width="435">The downloadable size of the JAR file in bytes.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

size

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">part</td><td headers="h3" width="435">Can be used to group resources together so they will be downloaded at the same time.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

part

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; [parent](#resources)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">A pointer to an additional component-desc or installer-desc to be used with this RIA.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">href</td><td headers="h3" width="435">The URL to the additional extension JNLP file.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

href

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">version</td><td headers="h3" width="435">The version of the additional extension JNLP file.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

version

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">name</td><td headers="h3" width="435">The name of the additional extension JNLP file</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

name

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; ext-download <sup>[parent](#extension)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Can be used in an extension element to denote the parts contained in a component-extension.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">ext-part</td><td headers="h3" width="435">The name of a part that can be expected to be found in the extension.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

ext-part

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">download</td><td headers="h3" width="435">Can be used to indicate this extension can be downloaded eagerly or lazily.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

download

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">part</td><td headers="h3" width="435">Denotes the name of a part in this JNLP file in which to include the extension.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

part

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; package <sup>[parent](#resources)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Can be used to indicate to the JNLP client which packages are implemented in which JAR files.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">name</td><td headers="h3" width="435">Package name contained in the JAR files of the given part.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

name

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">part</td><td headers="h3" width="435">Part name containing the JAR files that include the given package name.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

part

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">recursive</td><td headers="h3" width="435">Can be used to indicate that all package names, beginning with the given name, can be found in the given part.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

recursive

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; property <sup>[parent](#resources)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Defines a system property that will be available through the `System.getProperty` and `System.getProperties` methods.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">name</td><td headers="h3" width="435">Name of the system property.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

name

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">value</td><td headers="h3" width="435">Value of the system property.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

value

1.0
<td headers="h1"><br /></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Note: A JNLP file must contain one of the following: application-desc, applet-desc, component-desc, or installer-desc.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;[parent](#jnlp)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Denotes this is the JNLP file for an application.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">main-class</td><td headers="h3" width="435">The name of the class containing the `public static void main(String[])` method of the application.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

main-class

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; argument <sup>[parent](#applicationdesc)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Each argument contains (in order) an additional argument to be passed to the `main` method.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;[parent](#jnlp)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Denotes this is the JNLP file for an applet.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">main-class</td><td headers="h3" width="435">The name of the main applet class.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

main-class

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">documentbase</td><td headers="h3" width="435">The document base for the applet as a URL.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

documentbase

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">name</td><td headers="h3" width="435">Name of the applet.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

name

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">width</td><td headers="h3" width="435">The width of the applet in pixels.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

width

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">height</td><td headers="h3" width="435">The height of the applet in pixels.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

height

1.0
<td headers="h1">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; param <sup>[parent](#appletdesc)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">A set of parameters that can be passed to the applet.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">name</td><td headers="h3" width="435">The name of this parameter.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

name

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">value</td><td headers="h3" width="435">The value of this parameter.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

value

1.0
<td headers="h1">&#160;&#160;&#160;&#160;component-desc <sup>[parent](#jnlp)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Denotes this is the JNLP file for a component extension.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1">&#160;&#160;&#160;&#160;installer-desc <sup>[parent](#jnlp)</sup></td><td headers="h2" width="95"><br /></td><td headers="h3" width="435">Denotes this is the JNLP file for an installed extension.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62"><br /></td>

<br />

1.0
<td headers="h1"><br /></td><td headers="h2" width="95">main-class</td><td headers="h3" width="435">The name of the class containing the `public static void main(String[])` method of the installer.</td><td headers="h4" width="63">1.0</td><td headers="h5" width="62">Yes</td>

main-class

1.0

## Encoding JNLP Files

Java Web Start software supports encoding of JNLP files in any character encoding supported by the Java platform. For more information about character encoding in the Java platform, see the 
[Supported Encodings Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/intl/encoding.doc.html). To encode a JNLP file, specify an encoding in the XML prolog of that file. For example, the following line indicates that the JNLP file is encoded in UTF-16.

```

&lt;?xml version="1.0" encoding="utf-16"?&gt;

```
