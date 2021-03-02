
# Lesson: Writing JavaBeans Components

Writing JavaBeans components is surprisingly easy. You don't need a special tool and you don't have to implement any interfaces. Writing beans is simply a matter of following certain coding conventions. All you have to do is make your class *look* like a bean &#8212; tools that *use* beans will be able to recognize and use your bean.

However, NetBeans provides some features that make it easier to write beans. In addition, the Java SE API includes some support classes to help implement common tasks.

The code examples in this lesson are based on a simple graphic component called `FaceBean`.

A bean is a Java class with method names that follow the JavaBeans guidelines. A bean builder tool uses *introspection* to examine the bean class. Based on this inspection, the bean builder tool can figure out the bean's properties, methods, and events.

The following sections describe the JavaBeans guidelines for [properties](properties.html), [methods](methods.html), and [events](events.html). Finally, a section on
[`BeanInfo`](beaninfo.html) shows how you can customize the developer's experience with your bean.
