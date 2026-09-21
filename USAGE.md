# Using the messy-notes template

Everything below assumes you have

```latex
\documentclass[11pt]{article}
\usepackage[dotgrid]{messynotes}
\begin{document}
...
\end{document}
```

and that you compile with `pdflatex yourfile.tex`. See `example.pdf` for what
all of this looks like on the page.

Package options (pick at most one): `dotgrid` for dot-grid paper, `linegrid`
for ruled paper, or leave it off for blank paper.

---

## 1. The basic rhythm

A **blank line** in your source starts a new line of notes. You do not need
`\\`, and you do not need `itemize`. So this:

```latex
Boolean formula in conjunctive normal form

$n$ boolean variables $x_1,\dots,x_n$
```

gives you two lines with a small gap. If you want a hard line break with *no*
gap (rare), `\\` still works.

To put air between sections, `\gap` gives you one blank line and `\gap[2]`
gives you two. `\sep` draws a faint horizontal rule.

---

## 2. Indentation

Two ways, and you'll use both.

**One line at a time** — `\pt{...}` shifts a single line in by one step:

```latex
\pt{3-COLOR $\le_p$ 4-COLOR}
\pt[2]{this one is two steps in}
\pt[0]{this one is not shifted at all}
```

**A whole block** — the `ind` environment, which nests:

```latex
\begin{ind}
  everything in here is one step in
  \begin{ind}
    and this is two steps in
  \end{ind}
\end{ind}
```

`\begin{ind}[3]` jumps three steps at once. One step is `\mnstep`, which is
1.5em; change it with `\setlength{\mnstep}{2em}` in your preamble.

---

## 3. Bullets

Each bullet macro takes the text, plus an **optional depth** in square
brackets that indents it further relative to wherever you already are:

```latex
\bul{a dot bullet}
\bul[1]{one step deeper}
\bul[2]{two steps deeper}
```

The set that ships:

| macro | marker | what it's for |
|---|---|---|
| `\bul{...}` | • | the default dot |
| `\dash{...}` | – | a quieter alternative |
| `\arr{...}` | ↳ | a follow-on from the line above |
| `\imp{...}` | ★ | this one matters |
| `\qn{...}` | ? | I didn't follow this |
| `\bang{...}` | ! | warning to future me |
| `\yes{...}` | ✓ | |
| `\no{...}` | ✗ | |

They can appear anywhere — inside `ind`, inside a panel, inside a column, in
the middle of a proof. There is no list environment to open or close.

**Making your own.** `\newpoint` takes a command name and a marker:

```latex
\newpoint{\hmm}{\textcolor{mnviolet}{$\rightsquigarrow$}}
```

and now `\hmm{...}` and `\hmm[1]{...}` work like the rest. The left gutter
widens automatically if your marker is wide.

The markers are also available on their own, to drop mid-sentence:
`\arrow` (↳), `\cmark` (✓), `\xmark` (✗), and `\tomb` (□, for ending proofs).

---

## 4. Headings

Five levels of loudness. Pick by how much you want the eye to stop.

```latex
\notehead[9/15/26]{Cook--Levin}   % page title + date, with a rule under it
\headbar{Loudest: white on a solid colour bar}
\headbox{Loud: text in an outlined box}
\headu{Normal: bold with a coloured rule under the words}
\head{Quieter: bold and coloured, no rule}
\subhead{Quietest: small, bold, green}
```

The date on `\notehead` is optional — `\notehead{Title}` alone is fine, and
`\notehead[\today]{Title}` fills in today's date. The date column is measured
rather than fixed, so the date stays on one line however long it is; the only
exception is a date so long it would crowd out the title, at which point it
wraps. That threshold is `\mndatemax` (0.4, i.e. the date may claim at most
40% of the width) — `\renewcommand{\mndatemax}{0.55}` to let it run longer.
With no date, the title gets the full width.

---

## 5. Panels (boxes)

Each is an environment with an **optional label** that appears in bold at the
top of the box. Leave the brackets off to get the default label, or pass an
empty `[]` to get no label at all.

```latex
\begin{ex}                      % label defaults to "Example"
  ...
\end{ex}

\begin{ex}[Example: 4-SAT $\le_p$ 3-SAT]   % custom label
  ...
\end{ex}

\begin{shade}[]                 % no label
  ...
\end{shade}
```

| environment | look | default label |
|---|---|---|
| `ex` | soft green fill | *Example* |
| `idea` | soft amber fill | *Key idea* |
| `shade` | soft grey fill | *(none)* |
| `defn` | violet outline | *Definition* |
| `warn` | red outline | *Careful* |
| `outline` | blue outline | *(none)* |
| `quo` | grey bar down the left, italic text | *(none)* |

Bullets, `ind`, math and further panels all work inside a panel. Panels split
across a page break if they need to.

**Making your own.** Two constructors, each taking a name, a colour and a
default label:

```latex
\newshadebox{conj}{mnviolet}{Conjecture}     % filled version
\newoutlinebox{lemma}{mnteal}{Lemma}         % outlined version
```

Put those in your preamble and `\begin{conj} ... \end{conj}` works.

---

## 6. Side by side

**Two blocks:**

```latex
\sbs{left half}{right half}
\sbs[0.65]{wider left}{narrower right}     % left gets 65% of the width
```

**Three or more equal columns:**

```latex
\begin{cols}{3}
  first column
\nextcol
  second column
\nextcol
  third column
\end{cols}
```

The number in braces is how many columns you're going to write; `\nextcol`
moves to the next one. The gap between columns is `\mncolgap` (1.2em).

---

## 7. Emphasis, colour, math

Inline:

| macro | effect |
|---|---|
| `\key{...}` | bold blue — a term being defined |
| `\alert{...}` | bold red |
| `\hl{...}` | yellow highlighter (short phrases only — it won't break across lines) |
| `\ul{...}` | underline |
| `\aside{...}` | small and grey, for margin-comment-ish remarks |
| `\lead{Thm}` | an underlined blue lead-in word plus a colon, as in **Thm:** |

Raw colours: `\blue{}`, `\teal{}`, `\red{}`, `\amber{}`, `\violet{}`,
`\gray{}`.

`\lead` is the workhorse for the `Thm: / Pf: / Corollary:` rhythm:

```latex
\lead{Thm} 3-SAT is NP-complete.

\lead{Pf} suppose not. \quad\tomb
```

**Display math** uses `\eq{...}`, which sets it left-aligned at the current
indentation rather than centred on the page:

```latex
\eq{\phi = \bigwedge_{j=1}^{m} c_j}
\eq[3]{\text{same thing, pushed three steps in}}
```

Ordinary `$...$`, `\[...\]`, `align`, etc. all still work — `amsmath` is
loaded. `\eq` is just the one that matches the house style.

---

## 8. Changing the look

All of these go in your preamble, after `\usepackage{messynotes}`.

```latex
\setlength{\mnstep}{2em}      % bigger indent steps
\setlength{\mnhang}{1.4em}    % wider bullet gutter
\setlength{\mncolgap}{2em}    % more space between columns
\renewcommand{\mndatemax}{0.55}     % let \notehead dates run wider before wrapping
\colorlet{mnaccent}{mnviolet} % make violet "the" accent colour
\definecolor{mnblue}{HTML}{1A7F4B}   % or redefine a palette colour outright
\geometry{margin=0.6in}       % wider text block
```

For deeper changes, the colour palette and all the lengths sit in clearly
marked blocks near the top of `messynotes.sty`.

---

## 9. Two gotchas

- **`\ul` and `\headu` use `ulem`**, which quietly drops *declarations* like
  `\bfseries` or `\color{red}` after the first word. Inside an underline, use
  the bracketed forms instead: `\ul{\textbf{two words}}`, not
  `\ul{\bfseries two words}`.
- **`\hl` is a coloured box**, so a highlighted phrase can't break across a
  line. Keep it to a few words.
