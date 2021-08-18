# LaTeX

- Markup for generating professional-looking documents
- Extension: `.tex`

## Installation

For Arch:

```bash
sudo pacman -S texlive-most
```

For Mac:

http://www.tug.org/mactex/ and reboot.

## Minimal Working Example

paper.tex:

```tex
\documentclass{article}
\title{Hello World}
\author{Jane Doe}
\date{September 1994}
\begin{document}
    \maketitle
    Oh hi there!
\end{document}
```

Compile:

```bash
latex paper  # You can omit the .tex
# paper.pdf will be created
```

## Document classes

```
article for articles in scientific journals, presentations, short reports, program documentation, invitations, ...
proc a class for proceedings based on the article class.
minimal is as small as it can get. It only sets a page size and a base font. It is mainly used for debugging purposes.
report for longer reports containing several chapters, small books, thesis, ...
book for real books
slides for slides. The class uses big sans serif letters.
memoir for changing sensibly the output of the document. It is based on the book class, but you can create any kind of document with it (1)
letter For writing letters.
beamer For writing presentations.
```


