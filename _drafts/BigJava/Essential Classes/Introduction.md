
# Introduction

## What Are Regular Expressions?

*Regular expressions* are a way to describe a set of strings based on common characteristics shared by each string in the set. They can be used to search, edit, or manipulate text and data. You must learn a specific syntax to create regular expressions &#151; one that goes beyond the normal syntax of the Java programming language. Regular expressions vary in complexity, but once you understand the basics of how they're constructed, you'll be able to decipher (or create) any regular expression.

This trail teaches the regular expression syntax supported by the 
[`java.util.regex`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/package-summary.html) API and presents several working examples to illustrate how the various objects interact. In the world of regular expressions, there are many different flavors to choose from, such as grep, Perl, Tcl, Python, PHP, and awk. The regular expression syntax in the `java.util.regex` API is most similar to that found in Perl.

## How Are Regular Expressions Represented in This Package?

The `java.util.regex` package primarily consists of three classes: 
[`Pattern`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html), 
[`Matcher`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html), and 
[`PatternSyntaxException`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/PatternSyntaxException.html).

- A `Pattern` object is a compiled representation of a regular expression. The `Pattern` class provides no public constructors. To create a pattern, you must first invoke one of its `public static compile` methods, which will then return a `Pattern` object. These methods accept a regular expression as the first argument; the first few lessons of this trail will teach you the required syntax.
- A `Matcher` object is the engine that interprets the pattern and performs match operations against an input string. Like the `Pattern` class, `Matcher` defines no public constructors. You obtain a `Matcher` object by invoking the `matcher` method on a `Pattern` object.
- A `PatternSyntaxException` object is an unchecked exception that indicates a syntax error in a regular expression pattern.

The last few lessons of this trail explore each class in detail. But first, you must understand how regular expressions are actually constructed. Therefore, the next section introduces a simple test harness that will be used repeatedly to explore their syntax.
