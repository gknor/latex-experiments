# LaTeX experiments

Small experiments with LaTeX and the `memoir` class. Each file compiles on its own.

## Files

| File | Content |
|---|---|
| `book.tex` | A sample book with parts, chapters, sections and subsections. It also defines custom headings with their own editable numbering. |
| `gorilla.tex` | A gorilla drawn with TikZ shapes. `book.tex` loads it as a figure. |
| `asterisk-break.tex` | A separator sign between two fragments of text, with the same distances after a paragraph, a quotation, a list or a float. |
| `asterisk-break-flushbottom.tex` | The same separator with `\flushbottom`. It prints the measured distances on natural, stretched and shrunk pages. |

## Requirements

A TeX Live installation with `latexmk` and `pdflatex`. On macOS with Homebrew:

```bash
brew install texlive
```

## Compile

```bash
latexmk -pdf -outdir=build book.tex
```

Replace `book.tex` with another file name to compile a different document. The PDF and all helper files go to `build/`, which is not tracked by git.

To delete the helper files and keep the PDF:

```bash
latexmk -c -outdir=build book.tex
```

## Custom headings in `book.tex`

### `\topic`

A heading that looks like `\subsection`, with its own counter. The counter runs through the whole book and does not reset in a new chapter. The text and the table of contents show "Experiment 1", "Experiment 2" and so on.

```latex
\topic{A First Topic}        % Experiment 1
\skiptopic                   % jump one number
\topic{Another Topic}        % Experiment 3
\topicnumber{10}             % the next topic gets the number 10
\topicnumber*{10b}           % the next topic gets the text "10b", the count does not change
```

The word before the number is in `\topicname`.

### `\protocol` and `\protocolsummary`

Two headings on the `\subsubsection` level that share one counter. The title is optional. The number and the title are separated by a normal space, with no number box.

```latex
\protocol                          % PROTOCOL 1
\protocolsummary[The Title]        % PROTOCOL SUMMARY 2 The Title
\skipprotocol                      % jump one number
\protocolnumber{10}                % the next heading gets the number 10
\protocolnumber*{10a}              % the next heading gets the text "10a"
```

The words are in `\protocolname` and `\protocolsummaryname`.

All of these headings work with `\label` and `\ref`, and they appear in the table of contents and in the PDF bookmarks.

## The separator in `asterisk-break.tex`

`\myfancybreak{<sign>}` puts a centered sign between two fragments of text.

```latex
\myfancybreak{$\ast$}
```

Three parameters set the distances, from baseline to baseline, in units of `\baselineskip`:

| Parameter | Default | Meaning |
|---|---|---|
| `\fbabove` | 2.25 | last line of the text before, to the sign |
| `\fbbelow` | 1.75 | the sign, to the first line of the text after |
| `\fbflex` | 0.25 | stretch and shrink, as a part of the distance |

Known limits:

- The macro patches two LaTeX kernel commands (`\@addtocurcol` and `\end@float`) with `etoolbox`. A future LaTeX release can break the patches. The document then prints a warning.
- Floats placed with `[H]` from the `float` package are not handled.
- There is no page break between the sign and the text after it. This is by design.
- The shrink is a hard limit. The stretch is not: TeX can stretch the space more on a page with little other stretchable space.
