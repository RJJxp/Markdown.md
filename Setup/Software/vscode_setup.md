# VS Code 设置

保存一下 `.json` 设置文件和自己的扩展有哪些

有时候年代久了, 容易忘



## 1.扩展

-  `LaTex Workshop` : 用来写 `.tex` 文档
-  `Markdown All In One` 和 `Markdown Preview Enhanced` 组合起来, 前者和 `typora` 一样, 后者预览效果比 `VsCode` 自带的效果要好看



## 2. `.json` 设置文件

2019年1月19日15:02:15 更新

之前的配置只是单纯的用炜哥给的设置

在嘉定那边疯狂配置环境, 现在来看这些配置居然都能看懂什么意思了

发现自己之前的配置问题很大, 于是重新写一遍

```json
{
    // common setting
    "files.autoSave": "onFocusChange",
    "editor.wordWrap": "on",
    // git setting
    "git.ignoreMissingGitWarning": true,
    // latex-workshop part setting
    // first tool is default for compilation
    "latex-workshop.latex.tools": [
        {
            // use the xelatex instead of default latexmk
            // xelatex can work for chinese
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
    // first recipes is default for compilation
    "latex-workshop.latex.recipes": [
        {
            "name": "pdf",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "xe",
            "tools": [
                "xelatex"
            ]
        },
        {
            "name": "xe->xe",
            "tools": [
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdf->pdf",
            "tools": [
                "pdflatex",
                "pdflatex"
            ]
        },
        {
            "name": "xe->bib->xe->xe",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdf->bib->pdf->pdf",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        }
    ],
    // this makes sure alt+tab+v can show the pdf
    "latex-workshop.view.pdf.viewer": "tab",
    // when save .tex, don't build
    "latex-workshop.latex.autoBuild.onSave.enabled": false,
    // when error happens, don't clear build files like .aux .... 
    "latex-workshop.latex.autoBuild.cleanAndRetry.enabled": false
}
```



