
# Terminology

A **character** is a minimal unit of text that has semantic value.

A **character set** is a collection of characters that might be used by multiple languages. For example, the Latin character set is used by English and most European languages, though the Greek character set is used only by the Greek language.

A **coded character set** is a character set where each character is assigned a unique number.

A **code point** is a value that can be used in a coded character set. A code point is a 32-bit `int` data type, where the lower 21 bits represent a valid code point value and the upper 11 bits are 0.

A Unicode **code unit** is a 16-bit `char` value. For example, imagine a `String` that contains the letters "abc" followed by the Deseret LONG I, which is represented with two `char` values. That string contains four characters, four code points, but five code units.

To express a character in Unicode, the hexadecimal value is prefixed with the string U+. The valid code point range for the Unicode standard is U+0000 to U+10FFFF, inclusive. The code point value for the Latin character A is U+0041. The character &#8364; which represents the Euro currency, has the code point value U+20AC. The first letter in the Deseret alphabet, the LONG I, has the code point value U+10400.

The following table shows code point values for several characters:
<th id="h1">Character</th><th id="h2">Unicode Code Point</th><th id="h3">Glyph</th>
<td headers="h1">Latin A</td><td headers="h2">U+0041<br /></td><td headers="h3"><center><img src="../../figures/i18n/000041.gif" width="18" height="18" align="bottom" alt="The Latin character A" /></center></td>
<td headers="h1">Latin sharp S</td><td headers="h2">U+00DF<br /></td><td headers="h3"><center><img src="../../figures/i18n/0000df.gif" width="18" height="18" align="bottom" alt="The Latin small letter sharp S" /></center></td>
<td headers="h1">Han for East</td><td headers="h2">U+6771<br /></td><td headers="h3"><center><img src="../../figures/i18n/006771.gif" width="23" height="23" align="bottom" alt="The Han character for east, eastern or eastward" /></center></td>
<td headers="h1">Deseret, LONG I</td><td headers="h2">U+10400<br /></td><td headers="h3"><center><img src="../../figures/i18n/010400.gif" width="13" height="19" align="bottom" alt="The Deseret capital letter long I" /></center></td>

As previously described, characters that are in the range U+10000 to U+10FFFF are called supplementary characters. The set of characters from U+0000 to U+FFFF are sometimes referred to as the **Basic Multilingual Plane (BMP)**.

More terminology can be found in the **Glossary of Unicode Terms**, listed on the [More Information](info.html) page.
