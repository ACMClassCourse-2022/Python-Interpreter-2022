# ANTLR in Python Interpreter

## ANTLR 配置

首先需要安装 `Java`。

在 WSL 中进入 `antlr-playground`，输入：

```bash
export CLASSPATH=".:antlr-4.11.1-complete.jar:$CLASSPATH"
```

接着配置 `grun`:

```bash
alias grun='java org.antlr.v4.gui.TestRig'
```

之后，在命令行中输入：

```bash
grun Python3 file_input -gui
```

然后输入相应的 `Python` 代码，以 `EOF` 结束（Command-D 或 Ctrl-D），便会弹出一个弹窗，展示代码对应的树形结构。如果没有弹窗，参考[](#使用--ps)

`-gui` 可替换为 `-tree` 将树以 `LISP` 形式输出。

### 使用 -ps

在命令行中输入：

```bash
grun Python3 file_input -ps test.ps
```

之后在 `antlr-playground` 中会生成 `test.ps`，之后寻找将 PS 转化为 PNG 的在线转化器，上传 `test.ps`，即可得到相应的 `png` 文件。

## ANTLR 是什么

ANTLR 可以将输入的代码转化成与之对应的**树形结构**，即语法树，以便后续程序操作。语法树是什么，进入 `antlr-playground` 按照 [配置](#antlr-配置) 操作，即可得到一份 `Python` 代码对应的语法树。
