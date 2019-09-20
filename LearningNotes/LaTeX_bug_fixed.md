# `LaTeX` Tips

## bibliography

```latex
\usepackage{natbib}

i love my book \citet{rjp97} do you love it
\bibliographystyle{plainnat}
\bibliography{test.bib}
```

Here the `.bib` is

```latex
@article{rjp97,
AUTHOR = {rjp},
title = {rjpbook},
edition = {1st},
publisher = {rjppub},
address = {tj},
year = {2019}
}
```



## Using Chinese

```latex
\usepackage[BoldFont]{xecjk}
\setCJKmainfont[BoldFont=SimHei]{SimSun}
\setCJKmonofont{SimSum}
```

