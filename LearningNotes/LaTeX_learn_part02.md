# LaTeX Learning Notes Part02

by rjp

2019_01_21 started

由于 Part01的容量过大, 所以新开一个 `.md` 开始写学习笔记



[TOC]



## 8.Typesetting Mathematics

Donal Knuth created TEX primarily to typeset Mathematics beautifully.

### 8.1 The Basics

A mathematical expression occurring in running text (called in-text math) is produced by enclosing it between dollar signs. There are 3 ways to do that.

 `$ax+by+c=0$` , `\(ax+by+c=0\)` , `\begin{math}ax+by+c=0\end{math}` 

Now suppose we want to display math as the equation, this can be input as following:

```latex
% way 01
The equation representing a straight line in the Cartesian plane is of the form 
$$
ax+by+c=0
$$
where $a$, $b$, $c$ are constants.

% way 02
The equation representing a straight line in the Cartesian plane is of the form 
\[
ax+by+c=0
\]
where $a$, $b$, $c$ are constants.

% way 03
The equation representing a straight line in the Cartesian plane is of the form 
\begin{displaymath}
ax+by+c=0
\end{displaymath}
where $a$, $b$, $c$ are constants.
```

#### 8.1-1 Superscripts and subscripts

```latex
In the seventeenth century, Fermat conjectured that if $n>2$, then there are no intergers $x$, $y$, $z$ for which
$$
x^n+y^n=z^n.
$$
This is proved in 1994 by Andrew Wiles.
```

