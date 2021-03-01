
# Customizing Resource Bundle Loading

Earlier in this lesson you have learned how to create and access objects of the `ResourceBundle` class. This section extents your knowledge and explains how to take an advantage from the 
[`ResourceBundle.Control`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.Control.html) class capabilities.

The `ResourceBundle.Control` was created to specify how to locate and instantiate resource bundles. It defines a set of callback methods that are invoked by the 
[`ResourceBundle.getBundle`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html#getBundle-java.lang.String-java.util.Locale-java.util.ResourceBundle.Control-) factory methods during the bundle loading process.

Unlike a 
[`ResourceBundle.getBundle`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html#getBundle-java.lang.String-java.util.Locale-) method described earlier, this `ResourceBundle.getBundle` method defines a resource bundle using the specified base name, the default locale and the specified control.

```

public static final ResourceBundle getBundle(
    String baseName,
    ResourceBundle.Control cont
    // ...

```

The specified control provide information for the resource bundle loading process.

The following sample program called 
[`RBControl.java`](examples/RBControl.java) illustrates how to define your own search paths for Chinese locales.

## 1. Create the `properties` Files.

As it was described before you can load your resources either from classes or from `properties` files. These files contain descriptions for the following locales:

<li><code>
[`RBControl.properties`](examples/RBControl.properties)</code> &#150; Global</li>
<li><code>
[`RBControl_zh.properties`](examples/RBControl_zh.properties)</code> &#150; Language only: Simplified Chinese</li>
<li><code>
[`RBControl_zh_cn.properties`](examples/RBControl_zh_CN.properties)</code> &#150; Region only: China</li>
<li><code>
[`RBControl_zh_hk.properties`](examples/RBControl_zh_HK.properties)</code> &#150; Region only: Hong Kong</li>
<li><code>
[`RBControl_zh_tw.properties`](examples/RBControl_zh_TW.properties)</code> &#150; Taiwan</li>

In this example an application creates a new locale for the Hong Kong region.

## 2. Create a `ResourceBundle` instance.

As in the example in the previous section, this application creates a `ResourceBundle` instance by invoking the `getBundle` method:

```

private static void test(Locale locale) {
    ResourceBundle rb = ResourceBundle.getBundle(
                            "RBControl",
                            locale,
                            new ResourceBundle.Control() {
                                    // ...
                            }
                        );

```

The `getBundle` method searches for `properties` files with the RBControl prefix. However, this method contains a `Control` parameter, which drives the process of searching the Chineese locales.

## 3. Invoke the `getCandidateLocales` method

The `getCandidateLocales` method returns a list of the `Locales` objects as candidate locales for the base name and locale.

```

new ResourceBundle.Control() {
    @Override
    public List&lt;Locale&gt; getCandidateLocales(
                            String baseName,
                            Locale locale) {
                // ...                                        
    }
}

```

The default implementation returns a list of the `Locale` objects as follows: Locale(language, country).

However, this method is overriden to implement the following specific behavior:

```

if (baseName == null)
    throw new NullPointerException();

if (locale.equals(new Locale("zh", "HK"))) {
    return Arrays.asList(
               locale,
               Locale.TAIWAN,
               // no Locale.CHINESE here
               Locale.ROOT);
} else if (locale.equals(Locale.TAIWAN)) {
    return Arrays.asList(
               locale,
               // no Locale.CHINESE here
               Locale.ROOT);
}

```

Note, that the last element of the sequence of candidate locales must be a root locale.

## 4. Call the `test` class

Call the `test` class for the following four different locales:

```

public static void main(String[] args) {
    test(Locale.CHINA);
    test(new Locale("zh", "HK"));
    test(Locale.TAIWAN);
    test(Locale.CANADA);
}

```

## 5. Run the Sample Program

You will see the program output as follows:

```

locale: zh_CN
        region: China
        language: Simplified Chinese
locale: zh_HK
        region: Hong Kong
        language: Traditional Chinese
locale: zh_TW
        region: Taiwan
        language: Traditional Chinese
locale: en_CA
        region: global
        language: English

```

Note that the newly created was assigned the Hong Kong region, because it was specified in an appropriate `properties` file. Traditional Chinese was assigned as the language for the Taiwan locale.

Two other interesting methods of the `ResourceBundle.Control` class were not used in the `RBControl` example, but they deserved to be mentioned. The `getTimeToLive` method is used to determine how long the resource bundle can exist in the cache. If the time limit for a resource bundle in the cache has expired, the `needsReload` method is invoked to determine whether the resource bundle needs to be reloaded.
