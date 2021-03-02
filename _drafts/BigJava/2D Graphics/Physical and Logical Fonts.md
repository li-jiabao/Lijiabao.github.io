
# Physical and Logical Fonts

There are two types of fonts: physical fonts and logical fonts. Physical fonts are the actual font libraries consisting of, for example, TrueType or PostScript Type 1 fonts. The physical fonts may be Time, Helvetica, Courier, or any number of other fonts, including international fonts. Logical fonts are the following five font families: Serif, SansSerif, Monospaced, Dialog, and DialogInput. These logical fonts are not actual font libraries. Instead, the logical font names are mapped to physical fonts by the Java runtime environment.

This section helps you determine which type of font to use in your application. It covers the following topics:


  <li>[Physical Fonts](#physical-fonts)
    <ul>
      - [Lucidia Fonts](#lucidia-fonts)
      - [Bundling Physical Fonts with Your Application](#bundling-physical-fonts-with-your-application)
    
## <a name="physical-fonts" id="physical-fonts">Physical Fonts</a>

Physical fonts are the actual font libraries containing glyph data and tables to map from character sequences to glyph sequences, using a font technology such as TrueType or PostScript Type 1. To obtain the names of all available font families installed in your system, call the following:

```

GraphicsEnvironment ge = GraphicsEnvironment.getLocalGraphicsEnvironment();
String []fontFamilies = ge.getAvailableFontFamilyNames();

```

The FontSelector sample program (available in
[`FontSelector.java`](examples/FontSelector.java)) illustrates how to locate and select these fonts.

<applet code="FontSelector" archive="examples/lib/FontSelectorApplet.jar" alt="FontSelector applet" width="500" height="300"><param name="permissions" value="sandbox" /></applet>

### <a name="lucidia-fonts" id="lucidia-fonts">Lucidia Fonts</a>

Oracle's JREs contain this family of physical fonts, which is also licensed for use in other implementations of the Java platform. These fonts are physical fonts, but do not depend on the host operating system.

Applications using these fonts can achieve the same look wherever these fonts are available. Also, these fonts cover a large range of languages (especially European and Middle Eastern), so you can create fully multilingual applications for the supported languages. However, these fonts may not be available in all JREs. Also, they currently do not cover the complete Unicode character set; in particular, Chinese, Japanese, and Korean are not supported.

### <a name="bundling-physical-fonts-with-your-application" id="bundling-physical-fonts-with-your-application">Bundling Physical Fonts with Your Application</a>

Sometimes, an application cannot depend on a font being installed on the system, usually because the font is a custom font that is not otherwise available. In this case, you must bundle the font files with your application.

Use one of these methods to create a `Font` object from an existing physical font:

```

Font java.awt.Font.createFont(int fontFormat, InputStream in);
Font java.awt.Font.createFont(int fontFormat, File fontFile);

```

To create a `Font` object from a TrueType font, the formal parameter `fontFormat` must be the constant `Font.TRUETYPE_FONT`. The following example creates a `Font` object from the TrueType font file `A.ttf`:

```

Font font = Font.createFont(Font.TRUETYPE_FONT, new File("A.ttf"));

```

Accessing the font directly from a file is simpler and more convenient. However, you might require an `InputStream` object if your code is
unable to access file system resources, or if the font is packaged in a Java Archive (JAR) file along with the rest of the application or applet.

The `createFont` method creates a new `Font` object with a point size of 1 and style `PLAIN`. This base font can then be used with the `Font.deriveFont` methods to derive new `Font` objects with varying sizes, styles, transforms and font features. For example:

```

try {
     //Returned font is of pt size 1
     Font font = Font.createFont(Font.TRUETYPE_FONT, new File("A.ttf"));

     //Derive and return a 12 pt version:
     //Need to use float otherwise
     //it would be interpreted as style

     return font.deriveFont(12f);

} catch (IOException|FontFormatException e) {
     // Handle exception
}

```

It is important to use the `deriveFont` method because fonts that are created by an application are not part of the set of fonts known to the underlying font system. Because the `deriveFont` method works from the originally created font, it does not have this limitation.

The solution for this problem is to register the created font with the graphics environment. For example:

```

try {
     GraphicsEnvironment ge = 
         GraphicsEnvironment.getLocalGraphicsEnvironment();
     ge.registerFont(Font.createFont(Font.TRUETYPE_FONT, new File("A.ttf"));
} catch (IOException|FontFormatException e) {
     //Handle exception
}

```

After registering the font with the graphics environment, the font is available in calls to `getAvailableFontFamilyNames()` and can be used in font constructors.

## <a name="logical-fonts" id="logical-fonts">Logical Fonts</a>

Java SE defines the following five logical font families:

- `Dialog`
- `DialogInput`
- `Monospaced`
- `Serif`
- `SansSerif`

These fonts are available on any Java platform and can be thought of as aliases for some underlying font that has the properties implied by its name. A Serif font is a font similar to Times New Roman, which is commonly used in print. A Sans Serif font is more typical for onscreen use.

These fonts can be customized for the locale of the user. In addition these fonts support the widest range of code points (Unicode characters).

Apart from the family, fonts have other attributes, the most important of which are **style** and **size**. Styles are **Bold** and **Italic**.

The default font used by the Java 2D API is 12 pt Dialog. This font is a typical point size for reading text on a normal 72&#8211;120 DPI display device. An application can create an instance of this font directly by specifying the following:

```

Font font = new Font("Dialog", Font.PLAIN, 12);

```

## <a name="advantages-and-disadvantages" id="advantages-and-disadvantages">Advantages and Disadvantages of Using Physical and Logical Fonts</a>

Physical fonts enable an application take full advantage of all available fonts, to accomplish both different text appearances and maximum language coverage. However, it is substantially more difficult to create applications that use physical fonts.

Bundling physical fonts with your application enables you to create applications that have the same look everywhere, and to have full control over which applications you want to support. However, bundled fonts may be quite big, especially if you want your applications to support Chinese, Japanese, and Korean. In addition, you may need to resolve licensing issues.

Logical font names are guaranteed to work anywhere, and they enable text rendering in at least the language that the host operating system is localized for (often a much larger range of languages). However, The physical fonts used for rendering the text vary between different implementations, host operating systems, and locales, so an application can not achieve the same look everywhere. Also, the mapping mechanisms occasionally limit the range of characters that can be rendered. The latter used to be a big problem on JRE versions before 5.0: for example, Japanese text could only be rendered on Japanese localized host operating systems, not on other localized systems even if Japanese fonts have been installed. For applications using 2D font rendering, this problem is much rarer with JRE version 5.0 and later, because the mapping mechanism now generally recognizes and uses fonts for all supported writing systems if they are installed.

## <a name="font-configuration-files" id="font-configuration-files">Font Configuration Files</a>

The Java SE runtime environment uses font configuration files to map logical font names to physical fonts. There are several files to support different mappings depending on the host operating system version. The files are located in the `lib` directory within the JRE installation. You can edit or create your own font configuration files to adjust the mappings to your particular system setup. See
[Font Configuration Files](https://docs.oracle.com/javase/8/docs/technotes/guides/intl/fontconfig.html) for more information.
