<?xml version="1.0"?>
    <!--

# An XSLT 1.0 Stylesheet for Partial Conversion of Semantically Marked-up FODT to Semantically Marked-up LaTeX

## License for this stylesheet and its documentation

Copyright (c) 2013 Christian Gagné

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Purpose of this document

This stylesheet is an attempt to produce a non-standalone LaTeX document with semantic command and environment names, derived from e.g. the `text:style-name` and `text:name` attributes of a semantically styled Flat OpenDocument Text. The burden of writing an appropriate LaTeX preamble with definitions for all commands and environments falls on the person who receives the LaTeX fragment. In many situations, this can actually be very desirable.

The use of a Unicode-compliant TeX engine is expected.

The stylesheet contains documentation comments written in Markdown. If the stylesheet is processed as though it was a Markdown document, all XSLT code blocks will be converted into preformatted code blocks in the target document. If the Markdown processor produces an FODT from the stylesheet, then the stylesheet can more or less *process itself*.

The documentation’s sections follow those of the OpenDocument 1.2 spec.

## Visualization of this XSLT stylesheet’s code in a browser

This stylesheet references my `xsltviz.css`, which is available under the same license as the present document.

    -->
    <?xml-stylesheet href="../xsltviz/xsltviz.css" type="text/css" ?>
    <!--

## Root element and namespace declarations

All of the namespaces present in a typical LibreOffice-produced FODT 1.2 extended document are declared.

    -->
    <xsl:stylesheet version="1.0"
    xmlns:office="urn:oasis:names:tc:opendocument:xmlns:office:1.0"
    xmlns:style="urn:oasis:names:tc:opendocument:xmlns:style:1.0"
    xmlns:text="urn:oasis:names:tc:opendocument:xmlns:text:1.0"
    xmlns:table="urn:oasis:names:tc:opendocument:xmlns:table:1.0"
    xmlns:draw="urn:oasis:names:tc:opendocument:xmlns:drawing:1.0"
    xmlns:fo="urn:oasis:names:tc:opendocument:xmlns:xsl-fo-compatible:1.0"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    xmlns:dc="http://purl.org/dc/elements/1.1/"
    xmlns:meta="urn:oasis:names:tc:opendocument:xmlns:meta:1.0"
    xmlns:number="urn:oasis:names:tc:opendocument:xmlns:datastyle:1.0"
    xmlns:svg="urn:oasis:names:tc:opendocument:xmlns:svg-compatible:1.0"
    xmlns:chart="urn:oasis:names:tc:opendocument:xmlns:chart:1.0"
    xmlns:dr3d="urn:oasis:names:tc:opendocument:xmlns:dr3d:1.0"
    xmlns:math="http://www.w3.org/1998/Math/MathML"
    xmlns:form="urn:oasis:names:tc:opendocument:xmlns:form:1.0"
    xmlns:script="urn:oasis:names:tc:opendocument:xmlns:script:1.0"
    xmlns:config="urn:oasis:names:tc:opendocument:xmlns:config:1.0"
    xmlns:ooo="http://openoffice.org/2004/office"
    xmlns:ooow="http://openoffice.org/2004/writer"
    xmlns:oooc="http://openoffice.org/2004/calc"
    xmlns:dom="http://www.w3.org/2001/xml-events"
    xmlns:xforms="http://www.w3.org/2002/xforms"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:rpt="http://openoffice.org/2005/report"
    xmlns:of="urn:oasis:names:tc:opendocument:xmlns:of:1.2"
    xmlns:xhtml="http://www.w3.org/1999/xhtml"
    xmlns:grddl="http://www.w3.org/2003/g/data-view#"
    xmlns:officeooo="http://openoffice.org/2009/office"
    xmlns:tableooo="http://openoffice.org/2009/table"
    xmlns:drawooo="http://openoffice.org/2010/draw"
    xmlns:calcext="urn:org:documentfoundation:names:experimental:calc:xmlns:calcext:1.0"
    xmlns:field="urn:openoffice:names:experimental:ooo-ms-interop:xmlns:field:1.0"
    xmlns:formx="urn:openoffice:names:experimental:ooxml-odf-interop:xmlns:form:1.0"
    xmlns:css3t="http://www.w3.org/TR/css3-text/"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <!--

## Output method: Plain TeX(t) which is actually just as *structured* as XML

We are producing something that is considered to be plain text from the XSLT processor’s perspective, but of course by inserting semantic commands and environments much structure is preserved.

    -->
    <xsl:output method="text" encoding="utf-8" indent="no" />
    <!--

## Document Structure

The transformation applies to the `office:text` element of the FODT document. Two things should be produced in the resulting LaTeX document:

- a `document` environment containing all block-level elements such as `text:p` and all span elements such as `text:span` inside *environments* and *commands*, respectively;
- before the document environment, an embryonic preamble containing only empty environment and command definitions.

Not even the document class can be provided by the transformation: this would mean presupposing too much about the given FODT. The burden of defining the commands and environments, choosing the document class, loading the packages and setting options falls on the *LaTeX designer*. Given modern expectations about what TeX engines can do, I deem this to be a sensible method.

    -->
    <xsl:template match="//office:text">
      <xsl:apply-templates />
    </xsl:template>
    <!--

## Metadata

    -->

    <!--

## Text Content

    -->
    <!--

### Headings

    -->
    <xsl:template match="//text:h">
      <xsl:text>\begin{</xsl:text>
      <xsl:value-of select="@text:style-name" />
	  <xsl:text>}</xsl:text>
	    <xsl:value-of select="./text()" />
	  <xsl:text>\end{</xsl:text>
      <xsl:value-of select="@text:style-name" />
	  <xsl:text>}</xsl:text>
    </xsl:template>
    <!--

### Paragraphs

    -->
    <xsl:template match="//text:p">
      <xsl:text>\begin{</xsl:text>
      <xsl:value-of select="@text:style-name" />
	  <xsl:text>}</xsl:text>
	    <xsl:value-of select="./text()" />
	  <xsl:text>\end{</xsl:text>
      <xsl:value-of select="@text:style-name" />
	  <xsl:text>}</xsl:text>
    </xsl:template>
    <!--

## Paragraph Elements Content

    -->

    <!--

## Text Fields

    -->

    <!--

## Text *Indices*

    -->

    <!--

## Tables

    -->

    <!--

## Graphic Content

    -->

    <!--

## Chart Content

    -->

    <!--

## Form Content

    -->
    </xsl:stylesheet>
