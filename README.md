# MyPre Beamer Template

## Compile

Use **XeLaTeX** (recommended):

```bash
xelatex mypre_beamer_template.tex
xelatex mypre_beamer_template.tex
```

Two passes are needed so the table of contents is fully populated.

## Fonts

- English: **Times New Roman**. If unavailable, the theme falls back to TeX Gyre Termes.
- Chinese: **KaiTi / 楷体**. If unavailable, the theme falls back to `AR PL KaitiM GB`.

## Title hierarchy

Use a section for the first-level title and a subsection for the second-level title. Use the same subsection text as the frame title:

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

The contents page shows both levels. Section numbers in the contents are Roman numerals.

## Contents page

Keep the `[t]` option so the directory is vertically top-aligned rather than centered:

```latex
\begin{frame}[t]{Contents}
  \tableofcontents
\end{frame}
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

## 手动对齐

每页正文区域可以手动选择 **垂直上对齐 / 垂直居中**，以及 **水平左对齐 / 水平居中**。为了让垂直位置可控，建议 frame 一律写成 `[t]`，然后用 `MyPreContent` 包住该页主要内容：

```latex
\begin{frame}[t]{Your Subtitle}
  \begin{MyPreContent}[top][left]
    % 上对齐 + 左对齐
  \end{MyPreContent}
\end{frame}
```

四种组合分别是：

```latex
\begin{MyPreContent}[top][left]       ... \end{MyPreContent}
\begin{MyPreContent}[top][center]     ... \end{MyPreContent}
\begin{MyPreContent}[center][left]    ... \end{MyPreContent}
\begin{MyPreContent}[center][center]  ... \end{MyPreContent}
```

目录页已经作为示例设置为 `MyPreContent[top][left]`，即 **垂直上对齐、水平左对齐**。目录一级编号继续使用罗马数字 I、II、III，二级标题保留。

> 注意：目录条目的水平对齐由主题里的 `section in toc` / `subsection in toc` 模板决定，**不在**这两个模板里写 `\centering`，因此目录会跟随 frame 的对齐设置（默认左对齐）。


## Alignment controls
Use `MyPreContent` inside a `[t]` frame to choose alignment for each major content group:

```latex
\begin{MyPreContent}[top][left] ... \end{MyPreContent}
\begin{MyPreContent}[top][center] ... \end{MyPreContent}
\begin{MyPreContent}[center][left] ... \end{MyPreContent}
\begin{MyPreContent}[center][center] ... \end{MyPreContent}
```

The sample Contents slide uses `[top][left]`: vertically top-aligned and horizontally left-aligned.
