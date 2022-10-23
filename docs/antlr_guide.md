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

之后在 `antlr-playground` 中会生成 `test.ps`。一般来说，电脑上的 PDF 阅读器都可以成功打开 `.ps` 文件（双击打开即可），如果无法打开寻找将 PS 转化为 PNG 的在线转化器，上传 `test.ps`，即可得到相应的 `.png` 文件。

### 配置至 .bashrc 或 .zshrc

将：

```bash
export CLASSPATH=".:[antlr-complete-jar]:$CLASSPATH"
alias grun='java org.antlr.v4.gui.TestRig'
```

其中 `[antlr-complete-jar]` 表示的是 `antlr-4.11.1-complete.jar` 的绝对地址。在将上述命令输入至 `.bashrc` 或 `.zshrc` 之后，在命令行中输入：

```bash
source .bashrc
source .zshrc
```

有 `.bashrc` 选择前者，有 `.zshrc` 选择后者。即可在重新打开 WSL 的时候无需重新配置。

## ANTLR 是什么

ANTLR 可以将输入的代码转化成与之对应的**树形结构**，即语法树，以便后续程序操作。语法树是什么，进入 `antlr-playground` 按照 [配置](#antlr-配置) 操作，即可得到一份 `Python` 代码对应的语法树。
