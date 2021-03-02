
# Property Settings in the JDK


The following table shows the default values and behaviors of the new properties in the JDK.
<th id="h1">Values of access properties</th><th id="h2">Default Value</th><th id="h3">Set FSP(a)</th><th id="h4">jaxp.properties</th><th id="h5">system property</th><th id="h6">API property</th>
<td headers="h1">7u40</td><td headers="h2"><tt>all</tt></td><td headers="h3">no change</td><td headers="h4">override</td><td headers="h5">override</td><td headers="h6">override</td>
<td headers="h1">JDK8</td><td headers="h2"><tt>all</tt></td><td headers="h3">change to ""</td><td headers="h4">override</td><td headers="h5">override</td><td headers="h6">override</td>

(a) Set FSP means setting FEATURE_SECURE_PROCESSING explicitly using JAXP factories' <tt>setFeature</tt> method.

(b) The only behavioral difference between 7u40 and JDK8 is that setting FSP will not change <tt>accessExternal*</tt> properties in 7u40, but will set the value to an empty string in JDK8. Setting FSP is considered opt-in in JDK8.

(c) The order from left to right in the table reflects the overriding order. For example, if an <tt>accessExternal</tt> property is set through the API, it overrides any that may have been set by other means.
