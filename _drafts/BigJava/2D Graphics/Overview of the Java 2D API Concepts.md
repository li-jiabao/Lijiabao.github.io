
# Lesson: Overview of the Java 2D API Concepts

The Java 2D API provides two-dimensional graphics, text, and imaging capabilities for Java programs through extensions to the Abstract Windowing Toolkit (AWT). This comprehensive rendering package supports line art, text, and images in a flexible, full-featured framework for developing richer user interfaces, sophisticated drawing programs, and image editors. Java 2D objects exist on a plane called user coordinate space, or just [**user space**](coordinate.html). When objects are rendered on a screen or a printer, user space coordinates are transformed to [**device space coordinates**](coordinate.html). The following links are useful to start learning about the Java 2D API:

<li>
[`Graphics`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics.html) class</li>
<li>
[`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html) class</li>

The Java 2D API provides following capabilities:

- A uniform rendering model for display devices and printers
- A wide range of geometric primitives, such as curves, rectangles, and ellipses, as well as a mechanism for rendering virtually any geometric shape
- Mechanisms for performing hit detection on shapes, text, and images
- A compositing model that provides control over how overlapping objects are rendered
- Enhanced color support that facilitates color management
- Support for printing complex documents
- Control of the quality of the rendering through the use of rendering hints

These topics are discussed in the following sections:

- [Java 2D Rendering](rendering.html)
- [Geometric Primitives](primitives.html)
- [Text](text.html)
- [Images](images.html)
- [Printing](printing.html)
