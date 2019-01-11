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

single dash `-` produces a hyphen, two dashes `--` produce a longer dash, three dashes `---` produce the longest dash

## 2. The Document





