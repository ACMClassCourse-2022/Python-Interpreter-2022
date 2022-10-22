## step 1. 阅读 "./generated/Python3.g4", line 110 to 145

**阅读 `.g4` 文件需要一定的正则表达式基础。如果你不会，在 [slides](slides.pdf) 中介绍了相关知识。**

我们假设有这样的语法规则（并不是我们这次作业的一部分）：

```
    plus: NUMBER '+' NUMBER;
    NUMBER:[0-9]+;
    ADD:'+';
```

## step 2. 阅读 "./generated/Python3Parser.h"

你可以看到以下代码（对应着上面的语法规则）：

```c++
//...............
    class PlusContext : public antlr4::ParserRuleContext {
    public:
        PlusContext(antlr4::ParserRuleContext *parent, size_t invokingState);
        virtual size_t getRuleIndex() const override;
        std::vector<antlr4::tree::TerminalNode *> NUMBER();
        antlr4::tree::TerminalNode* NUMBER(size_t i);
        antlr4::tree::TerminalNode* ADD();

        virtual void enterRule(antlr4::tree::ParseTreeListener *listener) override;
        virtual void exitRule(antlr4::tree::ParseTreeListener *listener) override;

        virtual antlrcpp::Any accept(antlr4::tree::ParseTreeVisitor *visitor) override;
    }
//...............
```

因为在上述的 `plus` 语法中，`NUMBER` 一定要出现至少一次，所以 `PlusContext` 有以下两个函数：

```c++
    std::vector<antlr4::tree::TerminalNode *> NUMBER();
    antlr4::tree::TerminalNode* NUMBER(size_t i);
```

第一个函数返回一个 `vector`，包含了指向所有 `NUMBER` 的指针，第二个函数返回指向第 i 个 `NUMBER` 的指针，从 0 开始。

因为 `ADD` 只在 `plus` 中出现了一次，所以它只有以下函数，返回指向唯一的 `ADD` 的指针：

```c++
    antlr4::tree::TerminalNode* ADD()
```

对于一个 `temrinal node`，有以下方法：

```c++
//...............
std::string toString()
Token* TerminalNodeImpl::getSymbol()
/*
 * for example, consider:
 * antlr4::tree::TerminalNode *it;
 * it->toString() returns the string, for example, "123456" or "a"
 * (so you need to converse (std::string)"123456" to (int)123456)
 * it->getSymbol()->getTokenIndex() returns where this word is in the whole input.
 */
//...............
```

## step 3. 完成 "./src/Evalvisitor.h"

在这一步中，所要做的就是补全相关代码：

```c++
//...............
    antlrcpp::Any visitPlus(Python3Parser::PlusContext *ctx)
    {
        /*
         * TODO
         * the pseudo-code is:
         * return visit(ctx->NUMBER(0))+visit(ctx->NUMBER(1));
         */
    }
//...............
```

当写：

```c++
    visit(ctx->NUMBER(0))
```

等价于写：

```c++
    visitAtom(ctx->NUMBER(0))
```

所以我们只需要用 `visit` 函数来访问各种结点，而不是用 `visitBalabala`，想想为什么？

## step 4. 编译程序

输入以下代码即可：

```sh
cmake .
make
```

如果你不会使用 `cmake`，你可以借助于 `Clion` 来实现，如果你不知道这部如何操作，请询问助教。

## 神奇的 Antlrcpp::Any.

关于这个类，你只需要会两个方法：`is<T>()` 和 `as<T>()`。

譬如说，如果有以下的语法规则：

```
plus: atom '+' atom;
atom: NUMBER | STRING+;
NUMBER:[0-9]+;
STRING:[A-Z]+;
ADD:'+';
```

在 Parser.h 中的 `Context` 长这样：

```c++
//...............
    class PlusContext : public antlr4::ParserRuleContext {
    public:
        PlusContext(antlr4::ParserRuleContext *parent, size_t invokingState);
        virtual size_t getRuleIndex() const override;
        std::vector<AtomContext*> atom();
        AtomContext* atom(size_t i);
        antlr4::tree::TerminalNode* ADD()

        virtual void enterRule(antlr4::tree::ParseTreeListener *listener) override;
        virtual void exitRule(antlr4::tree::ParseTreeListener *listener) override;

        virtual antlrcpp::Any accept(antlr4::tree::ParseTreeVisitor *visitor) override;
    }
    //notice that the atom is AtomContext* type rather than a TerminalNode* type.
    //an easy way to tell the difference is: capital letter-TerminalNode; xxxContext otherwise
//...............
```

那么相应的代码就长这样：

```c++
//...............
	antlrcpp::Any visitPlus(Python3Parser::PlusContext *ctx)
    {
        antlrcpp::Any ret1, ret2;
        ret1 = visit(ctx->NUMBER());
        ret2 = visit(ctx->NUMBER());
        if (ret1.is<int>() && ret2.is<int>())
            return ret1.as<int>() + ret2.as<int>();
        else if (ret1.is<std::string>() && ret2.as<std::string>())
            return ret1.as<std::string>() && ret2.as<std::string>();
        else if (ret1.is<int>() && ret2.is<std::string>())				//no need
            throw("unsupported operand type(s) for +: 'int' and 'str'");//no need
    }
//...............
```

我们保证测试文件的语法都是正确的，所以后两行实则是不需要的。

最好在使用 `as<T>()` 之前 使用 `is<T>()` 来保证转换的正确性。

在 OOP 课程中，你们将会学习构造函数和析构函数。所以你们最好是在理解 `antlrcpp::Any` 是如何构造与析构的基础上，进行编程。通过阅读[Any.h](../third_party/runtime/src/support/Any.h)和[Any.cpp](../third_party/runtime/src/support/Any.cpp)来理解。如果这对于你们来说太过困难，请求助助教。

## 理解 antlrcpp::Any

https://www.cnblogs.com/mangoyuan/p/6446046.html

https://www.cnblogs.com/xiaoshiwang/p/9590029.html

搜索 "c++11 traits" 来获得更多信息。
