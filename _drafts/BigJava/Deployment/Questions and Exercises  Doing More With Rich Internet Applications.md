
# Questions and Exercises: Doing More With Rich Internet Applications

## Questions

1. True or False: Rich Internet applications (RIAs) can set secure properties by prefixing the property name with `"jnlp."`.
1. True or False: Only signed RIAs can use JNLP API to access files on the client.

## Exercises

<li>To the following JNLP file, add a secure property called `jnlp.foo` and set its value to `true`.
<pre><code>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;jnlp spec="1.0+" codebase="" href=""&gt;
    &lt;information&gt;
        &lt;title&gt;Dynamic Tree Demo&lt;/title&gt;
        &lt;vendor&gt;Dynamic Team&lt;/vendor&gt;

    &lt;/information&gt;
    &lt;resources&gt;
        &lt;!-- Application Resources --&gt;
        &lt;j2se version="1.6+" href=
            "http://java.sun.com/products/autodl/j2se" /&gt;
        &lt;jar href="DynamicTreeDemo.jar" main="true" /&gt;
    &lt;/resources&gt;
    &lt;applet-desc 
       name="Dynamic Tree Demo Applet"
       main-class="components.DynamicTreeApplet"
       width="300"
       height="300"&gt;
     &lt;/applet-desc&gt;
     &lt;update check="background"/&gt;
&lt;/jnlp&gt;                           
</code></pre>
</li>


[Check your answers.](answers.html)
