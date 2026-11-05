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
styles independently of that language. Commands without input, such as
`\tsBullet`, now take no argument. Existing `\tsBullet{}` calls remain valid:
the empty group is a spacing convention, not a required argument. Commands
with input, such as `\tsTextBold{text}`, retain their arguments.
The package includes text formatting, symbols, captions, citations, footnotes,
images, appendix helpers, the `\tsToday{}` date format, and `tsLTItemize`.

`\tsToday{}` uses MM/DD/YYYY for `american` and DD.MM.YYYY for other languages,
with two-digit months and days. It follows `\tsDocumentLanguage` when defined;
otherwise it follows Babel's active language. Its format selection is local and
does not change subsequent `\today` or `\formatdate` output.
Babel's `english` language name is treated as American in standalone usage,
since Babel can report `english` when loaded with the `american` alias.

Document metadata, header/footer configuration, TOC leaders, the custom `P`
column type, and numeric minipage footnotes remain in
[TSTemplate-Commands.tex](./TSTemplate-Commands.tex). The package does not enable
appendix mode, build a bibliography, or set the document's layout; these remain
the document author's responsibility.

## Dependency and option contract

`tscommands` loads the following direct dependencies:

| Dependency | Used for | Fresh-load options |
|---|---|---|
| `babel` | Explicit quotation symbols and active-language detection | None |
| `graphicx` | Image inclusion | None |
| `fancyvrb` | Verbatim appendix files | None |
| `float` | Exact `H` figure placement | None |
| `datetime` | Numeric date formats | None |
| `ifthen` | Language and label selection | None |
| `ulem` | Strikethrough text | `normalem` |
| `enumitem` | Compact table lists | None |
| `overpic` | Image overlays | `percent` |
| `nicefrac` | Fractions | None |
| `wasysym` | Diameter symbol | `nointegrals` |
| `xcolor` | Overlay text colors | None |
| `contour` | Outlined overlay text | `outline` |

Dependencies may load additional packages themselves. All command families
remain available by default; there is no lightweight mode.

Load dependencies with your preferred options **before** `tscommands`. Existing
package configurations take precedence, including for `ulem`, `overpic`,
`wasysym`, and `contour`; their fresh-load defaults are not reapplied. For
example:

```latex
\usepackage[ngerman]{babel}
\usepackage[dvipsnames]{xcolor}
\usepackage{wasysym}
\usepackage{tscommands}
```

Alternatively, use `\PassOptionsToPackage{options}{package}` before
`tscommands` when the dependency has not yet been loaded. For the four
option-bearing dependencies, these passed options are combined with the listed
fresh-load defaults; preload the package when you want to avoid those defaults.
Adding new package options after `tscommands` has loaded that dependency can
still cause LaTeX's normal option-clash error.

Respecting a preloaded package also respects its behavior: `ulem` without
`normalem` may change `\emph`, and a differently configured `contour` may render
overlays differently. Image helpers explicitly use `permil` coordinates, so
their overlay position does not rely on `overpic`'s global coordinate default.
German/French quote styles and the explicit `\textit` formatting command remain
independent of the document language.

## Template feature switches

### Stable reference identifiers

Caption, image, and appendix helpers accept an optional identifier before their
existing arguments:

```latex
\tsCaptionLabelFigure[logo-example]{A formatted or repeated caption}
\tsCaptionLabelTable[results]{Results}
\tsCaptionLabelFormula[energy]{E = mc^2}
\tsImage[logo-example]{logo.png}{Company logo}{width=0.5\linewidth}
\tsAppendixSection[notes]{Additional notes}
\tsAppendixIncludeFile[readme]{README.md}
```

Use `\ref{fig:logo-example}`, `\ref{tab:results}`, `\ref{for:energy}`,
or `\ref{appendix:readme}` to reference them. IDs omit the prefix and should be
unique within that prefix; simple letters, numbers, and hyphens are recommended.
All four image helpers (`\tsImage`, `\tsImageF`, `\tsImageO`, `\tsImageOF`)
support this leading optional ID.

Omitting the ID, or supplying `[]`, preserves the old caption/filename-based
label for caption, image, and appendix helpers. Prefer explicit IDs for new
content: changing a caption then leaves references stable, and repeated
captions can have different IDs.

### Image placement

All four image helpers accept a second optional argument for figure placement:

```latex
\tsImage[logo][htbp]{logo.png}{Company logo}{width=0.5\linewidth}
\tsImage[][htbp]{logo.png}{Company logo}{width=0.5\linewidth}
```

The first optional argument is always the ID; use `[]` when specifying placement
without an ID. Omitting placement retains `H` (exactly here), including existing
calls with only an ID or neither optional argument. `htbp` allows LaTeX to place
the figure here, at the top or bottom of a page, or on a float page. `!` can relax
some float-layout restrictions (for example `!htbp`); it does not mean "exactly
here". Figures remain indivisible and must fit on a page.

### Numbered formulas

Despite its historical name, `\tsCaptionLabelFormula[id]{formula}` now creates
an `equation`, not a figure caption. Its argument is math source without `$`
delimiters. Call it in document text, not inside another math environment or
as a caption in a figure/table. It increments the equation counter, not the
figure counter, and `\ref{for:id}` returns its equation number.

Without an ID (or with `[]`), it creates a label from the formatted equation
number, for example `for:1`. Prefer an explicit ID to keep references stable
when equations are inserted or reordered. This intentionally replaces the
former caption-text argument and caption-based numbering. For a captioned
formula illustration in a figure, use `\tsCaptionLabelFigure` instead.

### Enabling and disabling features

In `TSTemplate-Meta.tex`, use the corresponding `...false` command to disable
a feature (for example, `\UseTsGlossaryfalse`).

- Glossary: disables package loading, entry definitions, and glossary output.
  Calls to `\gls` in your content must be guarded by `\ifUseTsGlossary`.
- Citations: disables bibliography output. The reusable `\tsCitation` command
  remains available, but live citations must be guarded by `\ifUseTsCitation`
  when this document is not producing a bibliography.
- Appendices: skips the appendix file. References to its labels must be guarded
  by `\ifUseTsAppendix`.
- Index: disables index creation and output. Guard index entries with
  `\ifUseTsIndex`.
- TOC, list of figures, and list of tables: control printing of those lists,
  not the availability of sections, figures, or tables.

The supplied examples follow these rules. Their verbatim teaching code remains
visible when a feature is off; disabled live glossary/citation examples are
identified in the text. No supporting commands are silently replaced with
no-ops.

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
