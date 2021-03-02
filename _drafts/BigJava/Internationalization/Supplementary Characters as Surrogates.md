
# Supplementary Characters as Surrogates

To support supplementary characters without changing the `char` primitive data type and causing incompatibility with previous Java programs, supplementary characters are defined by a pair of code point values that are called **surrogates**. The first code point is from the **high surrogates** range of `U+D800` to `U+DBFF`, and the second code point is from the **low surrogates** range of `U+DC00` to `U+DFFF`. For example, the Deseret character LONG I, `U+10400`, is defined with this pair of surrogate values: `U+D801` and `U+DC00`.
