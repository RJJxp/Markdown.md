# VS Code 设置

保存一下 `.json` 设置文件和自己的扩展有哪些

有时候年代久了, 容易忘



## 1.扩展

-  `LaTex Workshop` : 用来写 `.tex` 文档
-  `Markdown All In One` 和 `Markdown Preview Enhanced` 组合起来, 前者和 `typora` 一样, 后者预览效果比 `VsCode` 自带的效果要好看



## 2. `.json` 设置文件

```json
{
    "latex-workshop.view.pdf.viewer": "tab",
    "workbench.colorTheme": "Monokai",
    "files.autoSave": "onFocusChange",
    "latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOC%"
            ]
        },
        {
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ]
        }, {
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }, {
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ]
        }, {
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ]
        }
    ],
    "editor.wordWrap": "on",
    "latex-workshop.message.update.show": false,
    "latex-workshop.latex.autoBuild.onSave.enabled": false,
    "git.ignoreMissingGitWarning": true
}
```

