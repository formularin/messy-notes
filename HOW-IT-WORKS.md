# How it works (the brief version)

There are four files:

| file | what it is |
|---|---|
| `messynotes.sty` | the template itself — a *package*, i.e. a pile of macro definitions |
| `notes-template.tex` | a blank starter file. Copy it, rename it, type in it. |
| `example.tex` / `example.pdf` | a worked demo: the Cook–Levin notes, plus a page showing every macro |
| `USAGE.md` | the command reference |

To use it, put `messynotes.sty` in the same folder as your `.tex` file and write:

```latex
\documentclass[11pt]{article}
\usepackage[dotgrid]{messynotes}
\begin{document}
...
\end{document}
```

then run `pdflatex yourfile.tex`. That's the whole setup — nothing to install,
assuming you have a normal TeX Live / MacTeX.

## The three ideas the package is built on

**1. A note is a stack of short lines, not paragraphs.**
Normal LaTeX wants to flow your text into justified blocks with indented first
lines. The package turns all of that off: `\parindent` is zero, text is ragged
right, and `\parskip` (the gap between paragraphs) is set to about 0.4 of a
line. The practical consequence is that **a blank line in your source = a new
line of notes**, with a small gap. You never type `\\`.

**2. Indentation is `\leftskip`, so it nests for free.**
`\leftskip` is a TeX parameter meaning "push every line of this paragraph in by
this much". The `ind` environment just adds one step (1.5em) to it inside a
group, so when the group ends the indent automatically pops back. Nesting
`ind` inside `ind` therefore works with no bookkeeping. Every other macro
that indents — `\pt`, `\eq`, and all the bullets — does the same thing.

**3. A bullet is a marker hung in the left gutter.**
`\bul{text}` indents the line by one *gutter width*, then uses `\llap` ("left
overlap") to drop the `•` back out into the gutter it just created. That's
why wrapped lines line up under the text instead of under the bullet, and why
you can put a bullet anywhere without opening an `itemize` environment. The
gutter widens automatically if you use a fat marker.

Everything else — the headings, the coloured panels, the columns — is a thin
wrapper over standard packages: `tcolorbox` for the panels, `ulem` for the
underlines, `minipage` for the columns, `tikz` for the `↳` arrow and the
dot-grid paper.

## Two things worth knowing

- **Fonts.** Text is Fira Sans. Math is `newtxsf` (a sans math font) under
  `pdflatex`, and Lete Sans Math under `lualatex`/`xelatex`. The package
  detects the engine and picks for you, so either works. `pdflatex` is the one
  to use unless you have a reason not to.
- **Colours** are defined in one block near the top of `messynotes.sty`
  (`mnblue`, `mnteal`, `mnred`, `mnamber`, `mnviolet`, `mngray`). Change the
  hex values there and the whole document follows, because every heading and
  panel refers to them by name.
