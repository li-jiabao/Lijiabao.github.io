
# Method Naming Conventions


The Date-Time API offers a rich set of methods within a rich set of classes. The method names are made consistent between classes wherever possible. For example, many of the classes offer a <tt>now</tt> method that captures the date or time values of the current moment that are relevant to that class. There are <tt>from</tt> methods that allow conversion from one class to another.


There is also standardization regarding the method name prefixes. Because most of the classes in the Date-Time API are immutable, the API does not include <tt>set</tt> methods. (After its creation, the value of an immutable object cannot be changed. The immutable equivalent of a <tt>set</tt> method is <tt>with</tt>.) The following table lists the commonly used prefixes:
<th id="h1">Prefix</th><th id="h2">Method Type</th><th id="h3">Use</th>
<td headers="h1"><tt>of</tt></td><td headers="h2">static factory</td><td headers="h3">Creates an instance where the factory is primarily validating the input parameters, not converting them.</td>
<td headers="h1"><tt>from</tt></td><td headers="h2">static factory</td><td headers="h3">Converts the input parameters to an instance of the target class, which may involve losing information from the input.</td>
<td headers="h1"><tt>parse</tt></td><td headers="h2">static factory</td><td headers="h3">Parses the input string to produce an instance of the target class.</td>
<td headers="h1"><tt>format</tt></td><td headers="h2">instance</td><td headers="h3">Uses the specified formatter to format the values in the temporal object to produce a string.</td>
<td headers="h1"><tt>get</tt></td><td headers="h2">instance</td><td headers="h3">Returns a part of the state of the target object.</td>
<td headers="h1"><tt>is</tt></td><td headers="h2">instance</td><td headers="h3">Queries the state of the target object.</td>
<td headers="h1"><tt>with</tt></td><td headers="h2">instance</td><td headers="h3">Returns a copy of the target object with one element changed; this is the immutable equivalent to a <tt>set</tt> method on a JavaBean.</td>
<td headers="h1"><tt>plus</tt></td><td headers="h2">instance</td><td headers="h3">Returns a copy of the target object with an amount of time added.</td>
<td headers="h1"><tt>minus</tt></td><td headers="h2">instance</td><td headers="h3">Returns a copy of the target object with an amount of time subtracted.</td>
<td headers="h1"><tt>to</tt></td><td headers="h2">instance</td><td headers="h3">Converts this object to another type.</td>
<td headers="h1"><tt>at</tt></td><td headers="h2">instance</td><td headers="h3">Combines this object with another.</td>
