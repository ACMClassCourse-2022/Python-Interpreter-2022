# ANTLR in Python Interpreter

## 使用 generate

如果你不想配置以下内容，可以使用助教提供的脚本，有两种方法使用：

### 使用 bash

在 WSL 中进入 `antlr-playground`，输入：

```bash
bash generate
```

之后在命令行中输入 Python 代码即可。

### 使用 chmod

在 WSL 中进入 `antlr-playground`，输入：

```bash
chmod +x generate
```

以上为初次配置，之后使用时：

```bash
./generate
```

之后在命令行中输入 Python 代码即可。

## 配置教程

### 安装 Java

在 WSL 中输入：

```bash
sudo apt-get update
sudo apt-get install openjdk
```

若输入：

```bash
java --verion
```

能够显示其版本，则安装成功。

### 配置 ANTLR

在 WSL 中进入 `antlr-playground`，输入以配置 `Java`：

```bash
export CLASSPATH=".:antlr-4.11.1-complete.jar:$CLASSPATH"
```

接着配置 `antlr4`：

```bash
alias antlr4='java -jar antlr-4.11.1-complete.jar'
```

接着配置 `grun`：

```bash
alias grun='java org.antlr.v4.gui.TestRig'
```

之后，在命令行中输入：

```bash
antlr4 Python3Lexer.g4
antlr4 Python3Parser.g4
javac *.java
grun Python3 file_input -gui
```

前两条命令只在初始时需要，第三条命令在每次生成新的语法树是都需要。然后终端会等待你输入相应的 `Python` 代码，以 `EOF` 结束（Ctrl-D），便会弹出一个弹窗，展示代码对应的树形结构。如果没有弹窗，参考[](#使用--ps)

`-gui` 可替换为 `-tree` 将树以 `LISP` 形式输出。

#### 使用 -ps

在命令行中输入：

```bash
grun Python3 file_input -ps test.ps
```

之后在 `antlr-playground` 中会生成 `test.ps`。一般来说，电脑上的 PDF 阅读器都可以成功打开 `.ps` 文件（双击打开即可），如果无法打开寻找将 PS 转化为 PNG 的在线转化器，上传 `test.ps`，即可得到相应的 `.png` 文件。

### 配置至 .bashrc 或 .zshrc

将：

```bash
export CLASSPATH=".:[antlr-complete-jar]:$CLASSPATH"
alias antlr4='java -jar [antlr-complete-jar]'
alias grun='java org.antlr.v4.gui.TestRig'
```

其中 `[antlr-complete-jar]` 表示的是 `antlr-4.11.1-complete.jar` 的绝对地址。在将上述命令输入至 `~/.bashrc` 或 `~/.zshrc` 之后，在命令行中输入：

```bash
source ~/.bashrc
source ~/.zshrc
```

注意，有 `.bashrc` 选择前者，有 `.zshrc` 选择后者。即可在重新打开 WSL 的时候无需重新配置。

### 使用 Clion 插件

由于本次 `.g4` 文件的特性，目前 ANTLR 插件只能支持不带 `INDENT` 和 `DEDENT` 规则的解释。

首先在插件市场中找到插件：

[](docs/plugin-market.png)

安装后，右键 `.g4` 中的 `return_stmt` 或任何不包含 `INDENT` 和 `DEDENT` 的规则，点击 `test rule`：

[](docs/right-click.png)

之后在屏幕下方的 `antlr-preview` 中，左侧是待测试的代码，右侧是依据代码生成的语法树结构图。

## ANTLR 是什么

ANTLR（全名：ANother Tool for Language Recognition）是基于 LL(\*)算法实现的语法解析器生成器（parser generator），用 Java 语言编写，使用自上而下（top-down）的递归下降 LL 剖析器方法。

ANTLR 可以将输入的代码转化成与之对应的**树形结构**，即语法树，以便后续程序操作。语法树是什么，进入 `antlr-playground` 按照 [配置](#antlr-配置) 操作，即可得到一份 `Python` 代码对应的语法树。
