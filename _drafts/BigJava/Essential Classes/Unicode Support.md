
# Unicode Support

As of the JDK 7 release, Regular Expression pattern matching has expanded functionality to support Unicode 6.0.

- [Matching a Specific Code Point](#matchingSpecific)
- [Unicode Character Properties](#properties)

## <a name="matchingSpecific" id="matchingSpecific">Matching a Specific Code Point</a>

You can match a specific Unicode code point using an escape sequence of the form `\uFFFF`, where `FFFF` is the hexidecimal value of the code point you want to match. For example, `\u6771` matches the Han character for east.

Alternatively, you can specify a code point using Perl-style hex notation, `\x{...}`. For example:

```

String hexPattern = "\x{" + Integer.toHexString(codePoint) + "}";

```

## <a name="properties" id="properties">Unicode Character Properties</a>

Each Unicode character, in addition to its value, has certain attributes, or properties. You can match a single character belonging to a particular category with the expression `\p{**prop**}`. You can match a single character **not** belonging to a particular category with the expression `\P{**prop**}`.

The three supported property types are scripts, blocks, and a "general" category.

### Scripts

To determine if a code point belongs to a specific script, you can either use the `script` keyword, or the `sc` short form, for example, `\p{script=Hiragana}`. Alternatively, you can prefix the script name with the string `Is`, such as `\p{IsHiragana}`.

Valid script names supported by `Pattern` are those accepted by 
[`UnicodeScript.forName`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.UnicodeScript.html#forName-java.lang.String-).

### Blocks

A block can be specified using the `block` keyword, or the `blk` short form, for example, `\p{block=Mongolian}`. Alternatively, you can prefix the block name with the string `In`, such as `\p{InMongolian}`.

Valid block names supported by `Pattern` are those accepted by 
[`UnicodeBlock.forName`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.UnicodeBlock.html#forName-java.lang.String-).

### General Category

Categories can be specified with optional prefix `Is`. For example, `IsL` matches the category of Unicode letters. Categories can also be specified by using the `general_category` keyword, or the short form `gc`. For example, an uppercase letter can be matched using `general_category=Lu` or `gc=Lu`.

Supported categories are those of 
[The Unicode Standard](http://www.unicode.org/unicode/standard/standard.html) in the version specified by the 
[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) class.
