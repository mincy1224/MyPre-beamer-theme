# MyPre Beamer Template

## Requirements

**XeLaTeX is required**, not merely recommended. `pdflatex` fails at the first
`\RequirePackage{fontspec}` (`fontspec` needs XeTeX or LuaTeX), and `lualatex`
fails at `\RequirePackage{xeCJK}` (`xeCJK` needs XeTeX).

Packages beyond a minimal TeX Live:

- **`newtx`** — provides `newtxmath.sty`, used for the math fonts. A minimal
  TeX Live (and several distro packages) does not ship it:
  ```bash
  tlmgr install newtx
  ```
  On Debian/Ubuntu it is usually part of `texlive-fonts-extra`. If your system
  `tlmgr` refuses with a "Local TeX Live is older than remote repository" error,
  point it at the matching historic repository, e.g.
  ```bash
  tlmgr --usermode --repository \
    https://ftp.math.utah.edu/pub/tex/historic/systems/texlive/2021/tlnet-final \
    install newtx
  ```
- **`multicol`** — used by `\MyPreTOC` for the two-column contents list.

Tested with TeX Live 2021 (XeLaTeX) on Linux.

## Compile

```bash
xelatex mypre_beamer_template.tex
xelatex mypre_beamer_template.tex
```

Two passes are needed: the first writes the `.toc` file, and the second uses it
both to fill in the contents page and to decide its layout.

## Installing the theme for reuse

To use `\usetheme{MyPre}` from a document in another directory, put the style
file where TeX can find it rather than copying it next to every document:

```bash
mkdir -p ~/texmf/tex/latex/MyPre
cp beamerthemeMyPre.sty ~/texmf/tex/latex/MyPre/
```

The `~/texmf` tree is searched automatically; no `texhash` is needed.

## Fonts

- English: **Times New Roman**. If unavailable, the theme falls back to
  **TeX Gyre Termes**.
- Chinese: **KaiTi / 楷体**. If unavailable, the theme falls back to
  **AR PL KaitiM GB**.

Both fallbacks have to be installed too, otherwise neither branch can be used —
the fallback names are not built into TeX Live. On the fallback path you may see
the following output while compiling:

```text
kpathsea: Running mktextfm KaiTi
This is METAFONT ...
! I can't find file `KaiTi'.
! Emergency stop.
```

This is **not** an error. It is XeTeX probing for the font, finding nothing, and
taking the fallback branch; the `Emergency stop` comes from the abandoned probe
subprocess, not from your document.

## Title hierarchy

Use a section for the first-level title and a subsection for the second-level
title. Use the same subsection text as the frame title:

```latex
\section{Review}
\subsection{DPF Invariants}
\begin{frame}{DPF Invariants}
  ...
\end{frame}
```

This renders the upper-left heading as:

```text
I. Review
DPF Invariants
```

Section numbers in the contents are Roman numerals.

## Contents page

Keep the `[t]` option so the contents list is vertically top-aligned:

```latex
\begin{frame}[t]{Contents}
  \begin{MyPreContent}[top][left]
    \MyPreTOC
  \end{MyPreContent}
\end{frame}
```

`\MyPreTOC` lays the list out in **one column**, and switches to **two balanced
columns** when the list would not fit in one. The decision is made from the
number of sections and subsections in the previous run's `.toc` file, so it
settles on the second pass.

If the list is too long to fit even in two columns, the theme raises

```text
Package beamerthemeMyPre Warning: Contents list is long ...
```

and you split it explicitly over two slides using beamer's own `sections` key —
the options are passed straight through to `\tableofcontents`:

```latex
\begin{frame}[t]{Contents}
  \begin{MyPreContent}[top][left]
    \MyPreTOC[sections={1-4}]
  \end{MyPreContent}
\end{frame}

\begin{frame}[t]{Contents (cont.)}
  \begin{MyPreContent}[top][left]
    \MyPreTOC[sections={5-9}]
  \end{MyPreContent}
\end{frame}
```

Pagination is deliberately **not** automatic: where a contents list breaks is a
decision about meaning (by topic, by part), not about height, and a list that is
split in the middle of a topic is worse than one spread over two slides.

Two knobs, if you need them:

```latex
\MyPreTocSectionSkip=1.0em   % vertical gap between first-level entries
\MyPreTocDebugtrue           % print the layout decision to the log
```

## Cover

Edit these fields in the main `.tex` file:

```latex
\title{...}
\author{...}
\date{...}
```

The cover shows the author as `Presenter: ...`.

## Page number

The lower-right page number uses a prominent 12 pt bold size.

## Alignment / 对齐

每一页正文区域可以手动选择 **垂直上对齐 / 垂直居中**，以及 **水平左对齐 / 水平居中**。为了让垂直位置可控，建议 frame 一律写成 `[t]`，然后用 `MyPreContent` 包住该页主要内容：

Use `MyPreContent` inside a `[t]` frame to choose the alignment of each major
content group. The four combinations are:

```latex
\begin{MyPreContent}[top][left]       ... \end{MyPreContent}
\begin{MyPreContent}[top][center]     ... \end{MyPreContent}
\begin{MyPreContent}[center][left]    ... \end{MyPreContent}
\begin{MyPreContent}[center][center]  ... \end{MyPreContent}
```

Both arguments are optional and default to `[top][left]`.

Note that `left` means `\raggedright`, so content inside `MyPreContent` is
**not** justified, while content outside it keeps beamer's default justification.
Mixing the two on one slide therefore gives inconsistent alignment.

> 目录条目的水平对齐由主题里的 `section in toc` / `subsection in toc` 模板决定，**不在**这两个模板里写 `\centering`，因此目录会跟随 frame 的对齐设置（默认左对齐）。
