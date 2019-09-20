# `LaTeX` Learning Notes

20190430 13:08:44

I naked myself in front of `LaTex` .

Finally, I make up my mind to finish this book also with the learning notes 

during almost the 2-years' long run.

This unfinished-book has become a nightmare for me.

And this time, I want to get rid of it in this short holiday.

Let's do it.



20190911 2203

Alright I didn't keep my promise.



[TOC]



## 1. The Basics

Here is a simple example of `LaTeX` file:

```latex
% \ : backslash
% / : slash
\documentclass{article}
\begin{document}
This is my \emph{first} document prepared in \LaTeX.
\end{document}
```

### 1.1 What is and Why is `LaTeX` ?

- `LaTeX` is a **typesetting program**.

  Focuses on how the input text is formatted into lines, paragraphs and pages.

  An extension of the original program `TeX` written by **Donald Knuth**.

- Why not simply use a word processor ?

  In a word processor, the operations including *text is entered into the computer*, *input text is formatted into lines, paragraphs and pages*, *output text is displayed on the computer screen*, *the final output is printed* are integrated into a single application package.

  **Donald Knuth** says that his aim in creating `TeX` is to 

  beautifully typeset technical documents especially those including a lot of Mathematics.

  So if you want your document to look really beautiful then `LaTeX` is the natural choice.

### 1.2 Simple Typesetting

The end of a paragraph is specified by a *blank line* in the input. 

The first line of each paragraph starts with an *indentation* from the left margin of the text.

You can use command `\noindent` to remove the *indentation*.

- **Space**

  In traditional typesetting, a little extra is added to periods which end sentences.

   `TeX` also follows this custom. It assumes that every period *not following an upper case letter ends a sentence*. So here are two exceptions:

  a *period* is the blank space from the end of a previous sentence to the start of the next sentence.

  - a sentence ends in an upper case letter

    ```latex
    % use command \@ before the period
    Carrots are good for your eyes, since they contain Vitamin A\@. Have you ever seen a rabbit wearing glasses?
    Carrots are good for your eyes, since they contain Vitamin A.\@ Have you ever seen a rabbit wearing glasses?
    ```

  - a period following a lowercase letter does not end a sentence

    ```latex
    % use command \ (a backslash and a space) before the period
    The number 1, 2, 3 etc.\ are called natural numbers. According to Kronecker, they were make by God; all else being the works of Man.
    ```

  There are other situations use the command `\ ` (backslash and space), for example:

  ```latex
  % try it and you will see the difference
  I think \LaTeX is fun.
  I think \LaTeX\ is fun.
  ```

- **Quote**

  2 ways to produce left singe quote and right single quotes:

  - keyboard *`* representing left single quotes, and *'* representing right single quotes
  - command `\lq` for left single quotes and command `\rq` for right single quotes

  To produce double quotes, input twice single quotes.

- **Dash**

  Single dash produces hyphen, two dashes produce s a longer dash and three dashes produces longest dash in the output.

  ```latex
  X-rays are discussed in pages 221--225 of Volume 3--- the volume on electromagnetic waves.
  ```

- **Special symbols**

  There are 10 special symbols in `LaTeX` .

  ```latex
  \textasciitilde 	% ~
  \textbackslash		% \
  \textasciicircum	% ^
  \#	\$	\%	\&	\_ 	\{	\}
  ```

  The reasons for particularity of these symbols need to be explained.

   `%` is for comments. `{}` is for scope of *declaration*. `\_` and `$` for mathematics.

   `\~` and `\^` are used for producing accents (which part I skip). 

  Also, `\\` is used for break lines.

  After `\\` , we give an optional argument to **increase** the vertical distance

  ```latex
  This is first line.\\[10pt]
  This is second line.
  ```

- **Text positioning**

  Use a `LaTeX` construct called an *environment* 

  ```latex
  \begin{center}		\end{center}
  \begin{flushright}	\end{flashright}
  \begin{flushleft}	\end{flushleft}
  ```

### 1.3 Types

The actual letters and symbols (collectively called *type*) that `LaTeX` (or any other typesetting system) produces are characterized by their *style* and *size*. A set of types of a particular style and size is called a *font*.

- **Type Style**

  A type style is specified by *family*, *series*, *shape*.

  <table>
      <!-- first block -->
  	<tr>
          <th>   </th>
          <th> TYPE </th>
          <th> COMMAND </th>
          <th> DECLARATION </th>
      </tr>
      <tr>
          <td rowspan="3"> FAMILY </td>
          <td> roman </td>
          <td> \textrm{roman} </td>
          <td> {\rmfamily roman} </td>
      </tr>
      <tr>
          <td> sans serif </td>
          <td> \textsf{sans serif} </td>
          <td> {\sffamily sans serif} </td>
      </tr>
      <tr>
          <td> typewriter </td>
          <td> \texttt{typewriter} </td>
          <td> {\ttfamily typewriter} </td>
      </tr>
      <!-- second block -->
      <tr>
          <td rowspan="2"> SERIES </td>
          <td> medium </td>
          <td> \textmd{medium} </td>
          <td> {\mdseries medium} </td>
      </tr>
      <tr>
          <td> boldface </td>
          <td> \textbf{boldface} </td>
          <td> {\bfseries boldface} </td>
      </tr>
      <!-- third block -->
      <tr>
          <td rowspan="4"> SHAPE </td>
          <td> upright </td>
          <td> \textup{upright} </td>
          <td> {\upshape upright} </td>
      </tr>
      <tr>
          <td> italic </td>
          <td> \textit{italic} </td>
          <td> {\itshape italic} </td>
      </tr>
      <tr>
          <td> slanted </td>
          <td> \textsl{slanted} </td>
          <td> {\slshape slanted} </td>
      </tr>
      <tr>
          <td> SMALL CAP </td>
          <td> \textsc{smalll cap} </td>
          <td> {\scshape small cap} </td>
      </tr>
  </table>

  As shown above, each of these type style changing commands has an alternate form as a *declaration*. Try the source code and see the difference:

  ```latex
  By a \bfseries{triangle}, we mean a polygon of three sides.
  By a {\bfseries triangle}, we mean a polygon of three sides.
  ```

  These declaration names can also be used as environment names for typesetting a long passage. For example

  ```latex
  \begin{sffamily}
  ...
  \end{sffamily}
  ```

  About `\emph` command

  When we are in the middle of normal (upright) text, it produces italic shape.

  But if current type shape is slanted or italic, then it switches to upright shape.

  ```latex
  % test source code
  \textit{A polygon of three sides is called a \emph{triangle} and a 	polygon of four sides is called a \emph{quadrilateral}}.
  ```

- **Type Size**

  Type size is measured in points. The default size is of 10 pt size.

  We use *declaration* to control the size of type.

  ```latex
  {\tiny size}				{\large size}
  {\scriptsize size}			{\Large size}
  {\footnotesize size}		{\LARGE size}
  {\small size}				{\huge size}
  {\normalsize size}			{\Huge size}
  ```

  Unlike the style changing commands, there are no *command-with-one-argument* forms for these declarations.


Finally, try to typeset this source code as the end of this chapter

```latex
\begin{center}
	{\bfseries\huge The \TeX nical Institute}\\[1cm]
	{\scshape\LARGE Certificate}
\end{center}

\noindent This is to certify that Mr.N.O. Vice has undergone a course at this institute and is qualified to be a \TeX nical Expert.

\begin{flashright}
	{\sffamily The Director\\
	The \TeX nical Institute}
\end{flashright}
```



## 2. The Document

In this chapter, I focus on the command of each sub-chapter.

The command is the core of each sub-chapter.

The scope of each command should be concrete. 

How large does the commands function?

What are the alternative options? What are the mandatory arguments?

Especially, various conditions of the command should be clear.

### 2.1 Document Class

 `LaTeX` files should begin by specifying the kind of document to be produced.

**Use the command** `\documentclass[options]{class}` **to decide the overall appearance of the document.**

Note that *options* are given in *square brackets* and not braces.

Mandatory arguments are given within *braces* after *options* in *square brackets*.

The *document classes* available in `LaTeX` are *article*, *book*, *report*, *letter*.

The *options* includes *font size*, *paper size*, *page format* 3 aspects.

- **Font Size**

  3 choices: *10pt*, *11pt*, *12pt*

  The default is 10pt if no font-size option is specified. 

- **Paper Size**

   `LaTeX` has its own method of breaking lines to make paragraphs.

​       It also has methods to make vertical breaks to produce different pages of output.

​       For these breaks to work properly, it must know the width and height of the paper used.

​       6 choices: *letterpaper*, *legalpaper*, *executivepaper*, *a4paper*, *a5paper*, *b5paper*

​       The default is *letterpaper*.

- **Page Format**

  - **column** :  *onecolumn*, *twocolumn*

    The default is *onecolumn*.

  - **side** : *oneside*, *twoside*

    The default is *oneside* for *article*, *report* and *letter* and *twoside* for *book*.

    In the *twoside*, page numbers are printed on the right on odd-numbered pages and on the left on even numbered pages.

    So that the numbers are always on the outside for visibility.

  - **chapter** : *openany*, *openright*

    A provision to specify the different chapters for *report* and *book* class in `LaTeX`.

    The default if openany for *report* and *openright* for *book*.

    Chapters always begin on a new page, leaving blank space in the previous page.

    With *book* class, there is an additional restriction that chapters begin only on odd-numbered pages, leaving an entire page blank if need be.

    Straightaway, in *report* class, chapters begin on "any" new page and in *book* class, chapters begin on new right, that is, odd-numbered pages.

  - **title** : *notitlepage*, *titlepage*

    A provision to format the "title" of a document with special typographic consideration.

    The default is *notitlepage* for *article* and *titlepage* for *report* and *book*.

    In the *article* class, this part is printed along with the text following on the first page.

    For *report* and *book* class, a separate title page is printed.

### 2.2 Page Style

The `\documentclass[options]{class}` decides on the overall appearance of the document through various options. Meanwhile we can set the individual pages.

What is *page* in `LaTeX` ? 

In `LaTeX` parlance, each page has a *head*, a *foot* containing such information as the current page number or the current chapter and *body* for necessary.

- **Style**

    `\pagestyle{style}` 

    style: *plain*, *empty*, *headings*, *myheadings*

    *plain* : *head* is empty, *foot* contains just the page number which is default for *article* class

    *empty* : both *head* and *foot* are empty. No page number printed.

    *headings* : *foot* is empty, *head* contains page number and chapter section subsection which is default for *book* class. 

    *myheadings* : same as *headings*, except that the "section" part is not predetermined. Use the command below to customize.
    `\markboth{left_head}{right_head}` for *twoside* 

    `\markright{right_head}` for *oneside*

    Customize the current page by command  `\thispagestyle{style}` 

​       The page number will be suppressed for the current page alone `\thispagestyle{empty}` 

​       Only the printing is suppressed. The next page will be numbered with the next number.

- **Numbering**

    `\pagenumbering{args}` : *arabic*, *roman*, *Roman*, *alph*, *Alph*

    The default value is *arabic*. This command resets the page counter.

     `\setcounter{page}{12}` The next page will be numbered 13 and so on.

- **Length**

   Use *geometry* package : `\setlength{\textwidth}{15cm}` 


### 2.3 Parts of Document

Now we turn our attention to the contents of the document itself.

- **Title** 

  ```latex
  \title{MyTitle}
  \author{Author_01\thanks{here is a footnote test}\\
  		Address line11\\
  		Address line12\\
  		Address line13
  		\and
  		Author_02\\
  		Address line21\\
  		Address line22\\
  		Address line23}
  % \date{} prints no date
  % \date will print the current date
  \date{Month Data, Year}
  ```

   `\thanks{footnote_text}` can be given at any point within the *\\title*, *\\author*, *\\date*. It put a marker at the point and places the *footnote_text* as a footnote.

- **Abstract** 

  ```latex
  \begin{abstract}
  \end{abstarct}
  ```

- **Chapter, Section, Paragraph** 

  ```latex
  % the highest hierarchy
  % not affect numbering of lesser sectioning command
  \part{part_name}
  
  % article class does not have chapter
  \chapter{chapter_name} 
  
  % a starred section does not produce numbers
  \section{section_name}
  \section*{section_name}
  
  \subsection{subsection_name}
  \sub...subsection{name}
  
  \paragraph{paragraph_name}
  \subparagraph{name}
  ```



## 3. Bibliography

## 4. Table of contents, Index and Glossary

## 5. Displayed Text

## 6. Rows and Columns

