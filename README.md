# LatexTemplate

A basic template for latex documents. See the [PDF][pdf] for more details.

## Reusable commands

[tscommands.sty](./tscommands.sty) provides the beginner-friendly commands
independently of the template. Place it beside your document (or install it in
your TeX search path), then load it in the preamble:

```latex
\documentclass{article}
\usepackage[american]{babel}
\usepackage{tscommands}

\begin{document}
\tsTextBold{Important}
\tsTextItalic{Italic text}
\tsTextQuoteG{German-style quotation marks}
\end{document}
```

The package loads its command dependencies. Load Babel with your desired
language before `tscommands`; the quotation commands retain their explicit
styles independently of that language. Existing names and arguments are
unchanged, including the empty `{}` argument for commands such as `\tsBullet{}`.
The package includes text formatting, symbols, captions, citations, footnotes,
images, appendix helpers, the `\tsToday{}` date format, and `tsLTItemize`.

Document metadata, header/footer configuration, TOC leaders, the custom `P`
column type, and numeric minipage footnotes remain in
[TSTemplate-Commands.tex](./TSTemplate-Commands.tex). The package does not enable
appendix mode, build a bibliography, or set the document's layout; these remain
the document author's responsibility.

## License

```code
-----------------------------------------------------------------------------
MIT License

Copyright 2022 ThirtySomething

Permission is hereby granted, free of charge, to any person obtaining a
copy of this software and associated documentation files (the "Software"),
to deal in the Software without restriction, including without limitation
the rights to use, copy, modify, merge, publish, distribute, sublicense,
and/or sell copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
DEALINGS IN THE SOFTWARE.
-----------------------------------------------------------------------------
```

[pdf]: ./TSTemplate.pdf
