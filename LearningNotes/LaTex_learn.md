# LaTeX Learning Notes

by rjp

2019_01_11 started



[TOC]

## 0. The Preface

同样的时间花在word和LaTeX上, 可能两者排版出的文档一样漂亮

但是我更喜欢LaTeX, 它排版格式可以通过代码控制

语句, 公式, 引用, 图片皆有迹可循



自己前前后后, 也零散地接触过 LaTeX

甚至还用 LaTeX 写了很多文档, 大地测量实习, 老张的两门课等等

炜哥给过我两本 LaTeX 教材, 一本中文, 一本英文

恕我直言, 中文书就是屎一样, 根本没有逻辑框架, 单纯告诉你语法

英文书深入浅出, 循循善诱, 读起来非常享受

当时在 wjx 家旁边的书店里把英文教程看了很多

时过境迁, 长时间不用 LaTeX, 忘得差不多了

重新看, 系统地学习一遍 LaTeX

 

所参考教材为

 [LaTeX Tutorials A PRIMER](https://www.tug.org.in/tutorials.html) 

作者: Indian TEX User Group

地点: Trivandrum, India

时间: 2003 September

由于教材为英文, 所以学习笔记也使用英文



## 1. The Basics

### 1.1 What is LaTeX?

LaTeX is a typesetting program originating from TEX written by Donald Knuth.

Hers are four steps in preparation of documents using computers.

1. the text entered into computer.
2. input text formatted into lines, paragraphs and pages.
3. output text displayed on computer screen.
4. final output is printed.

Typesetting program focus on the 2nd step.

- A small example

  ```latex
  \documentclass{article}
  \begin{document}
  This is my \emph{first} document prepared in \LaTeX
  \end{document}
  ```

   `\` character is called *backslash*.

- Why LaTeX?

  why not simplified the word processor?

  LaTeX is aimed at creating **beautifully** typeset technical documents especially those containing a lot of Mathematics.

### 1.2 Simple Typesetting

The end of a paragraph is specified by a blank line in the input.

If you do not want the indent, it's good to add `\noindent` in the start of each paragraph

 #### 1.2-1 Spaces

To make sure the length of output lines is equal, TEX will adjust the space between the words.

A little extra space is added to periods which end sentences. 

**period: in one line, the blank space between 2 sentences** by rjp.

How does TEX know that it's the end of a sentence? 

It assumes that every period **not following an upper case letter **ends a sentence.

What if a sentence ends up with a upper case letter? For example: 

 `Carrots are good for your eyes, since they contain Vitamin A. Have you ever seen a rabbit wearing glasses?` 

Add the command `\@` before the period.

Tried this on VsCode, the result is:

![difference between having \@ and not](pic/LaTeXLearning/chapter01/01.jpg)

And what if there is a low case letter, but not end the sentence, like:

 `The numeber 1, 2, 3, etc. are called natural numbers. According to Kronecker, they were made by God; all else being the work of Man.` 

Add the command `\` before the period.

Tried on VsCode, the result is:

![difference between having \ or not](pic/LaTeXLearning/chapter01/02.jpg)

#### 1.2-2 Quotes

Try `Note the difference in right and left quotes in 'single quotes' and "double quotes".` in LaTeX, you will find something like:

![quotes](pic/LaTexLearning/chapter01/03.jpg)

So recommend to use command `\lq` representing left single quotes, and  `\rq` representing right sinlge quotes. `\lq\lq` will represent left double quotes, and `\rq\rp` means right double quotes. 

#### 1.2-3 Dashes

Single dash `-` produces a hyphen, two dashes `--` produce a longer dash, three dashes `---` produce the longest dash.

#### 1.2-4 Accents(skip)

#### 1.2-5 Special symbols

Use symbol `\` to indicate the program that what follows should not be typeset,

but an instruction to be carried out.

If you want to type `\` , use `\textbackslash` 

Use `%` symbol to make comment, which means what behind `%` will not be typeset.

If you want to type `%` , use `\%` .

Here are 10 symbols like `%` or `\` , if you want type any of them, preceding them with a `\` ,

 `~` , `#` , `%` , `^` , `&` , `_` , `\` , `{` , `}` 

with 3 exception below.

 `~` : `\textasciitilde` 

 `^` : `\textasciicircum` 

 `\` : `\textbackslash`

Because `\\` used to break lines, also we can give a optional parameter to increase the distance between the lines. For example:

 `This is the first line.\\[10pt] This is the second line.` 

#### 1.2-6 Text positioning

suppose you wanna type something like:

![pic04](pic/LaTexLearning/chapter01/04.png)

This is produced by 

```latex
% text between `begin` and `end` is aligned in the middle of page
\begin{center} 
The \TeX nical Institute\\[0.75cm]
Certificate
\end{center}
% no indent
\noindent This is to certify that Mr.N.O. Vice has undergone a course at this institute and is qualified to be a \TeX nicain.
% typeset text flush with the right margin
\begin{flushright}
The Director\\
The \TeX nical Institute
\end{flushright}
```

These examples are an illustration of a LaTeX construct called **environment**, which is of the form like: `\begin{env_name} ... \end{env_name}` 

### 1.3 Fonts

The actual letters and symbols (collectively called **type**) that LaTeX produces are characterized by their **style** and **size**. A set of types of a particular style and size is called a **font**.

#### 1.3-1 Type style

In LaTeX, a type style is specified by **family**, **series** and **shape**. Any type in the output is a combination of these three characteristics.

Here is the table:

<table>
    <!-- first block -->
	<tr>
        <th>   </th>
        <th> TYPE </th>
        <th> COMMAND </th>
    </tr>
    <tr>
        <td rowspan="3"> FAMILY </td>
        <td> roman </td>
        <td> \textrm{roman} </td>
    </tr>
    <tr>
        <td> sans serif </td>
        <td> \textsf{sans serif} </td>
    </tr>
    <tr>
        <td> typewriter </td>
        <td> \texttt{typewriter} </td>
    </tr>
    <!-- second block -->
    <tr>
        <td rowspan="2"> SERIES </td>
        <td> medium </td>
        <td> \textmd{medium} </td>
    </tr>
    <tr>
        <td> boldface </td>
        <td> \textbf{boldface} </td>
    </tr>
    <!-- third block -->
    <tr>
        <td rowspan="4"> SHAPE </td>
        <td> upright </td>
        <td> \textup{upright} </td>
    </tr>
    <tr>
        <td> italic </td>
        <td> \textit{italic} </td>
    </tr>
    <tr>
        <td> slanted </td>
        <td> \textsl{slanted} </td>
    </tr>
    <tr>
        <td> SMALL CAP </td>
        <td> \textsc{smalll cap} </td>
    </tr>
</table>


When command `\emph` in the middle of normal text, it produces italic shape.

When the current type is slanted or italic, then `\emph` switches to upright type.

And each of these type command has an alternate form as a **declaration**.

For example, instead of `\textbf{boldface}` , you can type `{\bfseries boldface}` 

#### 1.3-2 Type size

The default type TEX produces is of 10 pt size. There are some **decarations** provided in LaTeX for changing the type size. Have a try in LaTeX.

 `{\tiny size}` 

 `{\scriptesize size}` 

 `{\footnotesize size}` 

 `{\small size}` 

 `{\normalsize size}` 

 `{\large size}` 

 `{\Large size}` 

 `{\LARGE size}` 

 `{\huge size}` 

 `{\Huge size}` 

Unlike the style changing commands, there are no **command-with-one-argument** forms for these declarations.

Now, retry the certificate:

```latex
\begin{center}
	{\bfseries\huge The \TeX nical Institute}\\[1cm]
	{\scshape\LARGE Certificate}
\end{center}

\noindent This is to certify that Mr.N.O. Vice has undergone a course at this institute and is qualified to be a \TeX nical Expert.

\begin{flushright}
{\sffamily The Director\\
The \TeX nical Institute}
\end{flushright}
```



## 2. The Document

### 2.1 Document Class

We now describe how an entire document with chapters and sections and other embellish-ments can be produced with LaTeX. All LaTeX files should begin by specifying the kind of document to be produced, using the command `\documentclass{}` .

For a short article, we use `\documentclass{article}` .

For books, we use `\documentclass{book}` . Also there are other document class as `report` and `letter` .

We can also specify some options which modify the default format. Thus the actual syntax of the document class is `\documentclass[options]{class}` . Note the **options** are given in **square brackets** not braces.

You can describe the options in three aspects: **font size**, **paper size** and **page format**

- font size: **10pt**, **11pt**, **12pt**

- paper size: **letterpaper**, **legalpaper**, **executivepaper**, **a4paper**, **a5paper**, **b5paper**

- page format

  - **onecolumn**, **twocolumn**

  - **oneside**, **twoside**

    oneside is default for article, report and letter; **twoside is default for book**. The page numbers is always on the outside when it's twoside.

  - **openany**, **openright**

    In the report and book class, there is a provision to specify the different chapters. Chapters always begin on a new page, leaving blank space in the previous page, if necessary. With the book class there is the additional restriction that chapters begin only on odd-numbered pages, leaving an entire page blank, if need be.

    The default is openany for report class and openright is default for book class.

  - **notitlepage**, **titlepage**

    There is also a provision in LATEX for formatting the title (the name of the document,
    author(s) and so on) of a document with special typographic consideration. In the
    article class, this part of the document is printed along with the text following on the
    first page, while for report and book, a separate title page is printed.

    The default is notitlepage for article and titlepage for report and book.

### 2.2 Page Style

Having decided on the overall appearance of the document through the `\documentclass` 
command with its various options, we next see how we can set the style for the individual
pages. In LATEX parlance, each page has a “head” and “foot” usually containing such
information as the current page number or the current chapter or section. 

Just what goes where is set by the command `\pagestyle{...}` 

and mandatory argument can be any one of these following styles:

`plain` , `empty` , `headings` , `myheadings` 

The behavior pertaining to each of these is given below:

- **plain**: 

  The page head is empty and the foot contains just the page number, centered with respect to the width of the text. This is the default for the article class if no `\pagestyle` is specified in the preamble.

- **empty**:

  Both the head and foot are empty. In particular, no page numbers are printed.

- **headings**:

  This is the default for the book class. The foot is empty and the head contains the page number and names of the chapter section or subsection, depending on the document class and its options as given below:

  <table>
      <tr>
      	<th> CLASS </th>
      	<th> OPTION </th>
      	<th> LEFT PAGE </th>
      	<th> RIGHT PAGE </th>
      </tr>
      <tr>
      	<td rowspan="2"> book, report </td>
          <td> one-sided </td>
          <td> --- </td>
          <td> chapter </td>
      </tr>
      <tr>
      	<td> two-sided </td>
          <td> chapter </td>
          <td> section </td>
      </tr>
      <tr>
      	<td rowspan="2"> article </td>
          <td> one-sided </td>
          <td> --- </td>
          <td> section </td>
      </tr>
      <tr>
      	<td> two-sided </td>
          <td> section </td>
          <td> subsection </td>
      </tr>
  </table>

- **myheadings**:

  The same as headings, except that the section information in the head are not predeter-mined, but to be given explicitly using the commands `\markright` or `\markboth ` .

   `\markboth{left_head}{right_head}` 

   `\markright{right_head}` 

  Customize what is on the head of page

  The `\markboth` command is used with the **twoside** option with even numbered pages
  considered to be on the left and odd numbered pages on the right. And `\markright` command is used for the **oneside** option.

We can customize the current page by the command `\thispagestyle{style}` .

### 2.3 Page Numbering

 `\pagenumbering{...}` command, the argument is shown below:

| Argument |           Style           |
| :------: | :-----------------------: |
|  arabic  |   Indo-Arabic numberals   |
|  roman   | lowercase Roman numberals |
|  Roman   | uppercase Roman numberals |
|   alph   | lowercase English letters |
|   Alph   | uppercase English letters |

Command `\pagenumbering{...}` will reset the page counter.

We can make the page start with any number we want by command:

 `\setcounter{page}{number}` 

this command will also reset the page counter, and the following page will continue the set page number.

### 2.4 Formatting lengths

Each page LaTeX produces consists not only of a **head** and **foot**, but also a **body**, which contains the actual text. You can use command `\setlength{\textwidth}{15cm}` makes the width of text 15cm. The package **geometry** gives easier way to customize page format.

### 2.5 Parts of Document

We now turn our attention to the document itself. Documents are divided into **chapters**, **sections** and so on. There may be a little part like **title**, **abstract**.

#### 2.5-1 Title

```latex
\title{document_names}
\author{author_names}
\date{date_text}

\maketitle % make sure the title part to be typeset
```

If the title is too long, use command `\\` to break it at appropriate place; If there are several authors and their names are separated by `\and` command

The command `\thanks{footnote_text}` can be given at any point within `\title` , `\author` , `\date` 

#### 2.5-1 Abstract

In the document classes **article** and **report**, you can produce abstract by the command:

```latex
\begin{abstract}
Abstract Text
\end{abstract}
```

### 2.6 Dividing the document

LaTeX provides the following hierarchy of sectioning commands in the **book** and **report** class:

 `\chapter` , `\section` , `\subsection` , `subsubsection` , `paragraph` , `subparagraph` 

Except for `\chapter` , all these are available in **article** class. For example:

```latex
\section{1111}
    \subsection{1}
\section{2222}
    \subsection{1}
    \subsection{2}
```

The result is like:

![pic](pic/LaTexLearning/chapter02/01.png)

So you do not have to set up the number of every section by yourself, LaTeX will do it for you.

- book and report

  In the book and the report classes, the `\chapter` command shifts to the beginning of a new page and prints the word “Chapter” and a number and beneath it, the name we have
  given in the argument of the command. The \section command produces two numbers
  (separated by a dot) indicating the chapter number and the section number followed by the name we have given. It does not produce any text like “Section”. Subsections have three numbers indicating the chapter, section and subsection. Subsubsections and commands below it in the hierarchy do not have any numbers.

- article

  In the article class, `\section` is highest in the hierarchy and produces single number like `\chapter` in book. (It does not produce any text like “Section”, though.) 

  In this case, subsubsections also have numbers, but none below have numbers.

Each sectioning command also has a “starred” version which does not produce numbers. Thus `\section*{name}` has the same effect as `\section{name}` , but produces no number for this section.

Some books and longish documents are divided into parts also. LATEX also has a `\part` command for such documents. In such cases, \part is the highest in the hierarchy, but it does not affect the numbering of the lesser sectioning commands.

You may have noted that LATEX has a specific format for typesetting the section headings, such as the font used, the positioning, the vertical space before and after the heading and so on. <font color=red>All these can be customized, but it requires some TEXpertise and cannot be addressed at this point.</font> However, the package **sectsty** provided some easy interfaces for tweaking some of these settings.

### 2.7 What's next?

The task of learning to create a document in LATEX is far from over. There are other things to do such as **producing a bibliography** and a method to refer to it and also at the end of it all to **produce a table of contents** and perhaps an index. 

All these can be done efficiently (and painlessly) in LATEX, but they are matters for other chapters



## 3. Bibliography

### 3.1 Introduction

Bibliography is the environment which helps the author to cross-reference one publication from the list of sources at the end of the document. LaTeX helps authors to write a well structured bibliography. Package **harvard** and **natbib** are widely used for generating bibliography.

To produce bibliography, we have the environment **thebibliography**, which is similar to the **enumerate** environment. Here we use the command `\bibitem` to separate the entries in the bibliography and use `\cite` to refer to a specific entry from this list in the document.

```latex
% widest-label indicates the width of the widest label in the bibliography
% the key is a mnemonic string used got cite the publication within the 
% document text. Key can be any sequence of letters, digits and punctuation 
% characters, except that it may not contain a comma (maximum 256 characters)
\begin{thebibliography}{widest-label}
	\bibitem{key1}
	\bibitem{key2}
\end{thibibliography}
```

Example:

```latex
It is hard to write unstructured and disorganised documents using \LaTeX˜\cite{les85}.It is interesting to typeset one equation˜\cite[Sec 3.3]{les85} rather than setting ten pages of running matter˜\cite{don89, rondon89}.

\begin{thebibliography}{9}
    \bibitem{les85}Leslie Lamport, 1985. \emph{\LaTeX---A Document Preparation System---User’s Guide and Reference Manual}, Addision-Wesley, Reading.
    \bibitem{don89}Donald E. Knuth, 1989. \emph{Typesetting Concrete Mathematics}, TUGBoat, 10(1):31-36.
    \bibitem{rondon89}Ronald L. Graham, Donald E. Knuth, and Ore Patashnik, 1989. \emph{Concrete Mathematics: A Foundation for Computer Science}, Addison-Wesley, Reading.
\end{thebibliography}
```

The result is : 

![pic](pic/LaTexLearning/chapter03/01.png)

### 3.2 Natbib

The **natbib** package is widely used for generating bibliography, because of its flexible interface for most of the available bibliographic styles. The **natbib** package is a reimplementation of the LATEX \cite command, to work with both author–year and numerical citations.

Use command `\usepackage[options]{natbib}` to load the package

