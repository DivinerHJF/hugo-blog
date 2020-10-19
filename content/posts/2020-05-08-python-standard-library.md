---
title: "Python 标准库之我见"
date: 2020-05-18T00:20:31+08:00
categories: 技术
tags:
    - python
    - Python 标准库
mathjax: false
codes: [python, ]
comments: true
vertical: false
---

「Python 标准库」中包含了几种不同类型的组件。

首先，它包含 **数据类型** —— 任何一门编程语言的「核心部分」，例如数字和列表。对于这些数据类型，Python 语言内核定义了简单的形式，并对其语法作了一些约束，但不是完全定义它们的语法（另一方面，语言内核的确定义了一些语法特性，如操作符的拼写和优先级）。

其次，这个库也包含了 **内置函数和异常** —— 不需要 import 语句另外导入就可以在所有 Python 代码中使用的对象。其中一些被核心语言定义，但大部分对核心语法而言并不是必须的，只是在这里介绍一下。

最重要的，Python 标准库主体是由一系列的 **模块** 组成的。这些模块集可以不同方式分类。有些模块是用 C 编写并内置于 Python 解释器中；另一些模块则是用 Python 编写并以源码形式导入。有些模块提供专用于 Python 的接口，例如打印栈追踪信息；有些模块提供专用于特定操作系统的接口，例如操作特定的硬件；另一些模块则提供针对特定应用领域的接口，例如万维网。有些模块在所有更新和移植版本的 Python 中可用；另一些模块仅在底层系统支持或要求时可用；还有些模块则仅当编译和安装 Python 时选择了特定配置选项时才可用。

Python 标准库可按「从内到外」的顺序进行认知：首先描述内置函数、数据类型和异常，最后是根据相关性进行分组的各种模块。

本文仅对内置函数、数据类型和异常中的一些常用组件进行总结，若想浏览全部内容请参见 [官方文档](https://docs.python.org/zh-cn/3/library/index.html)。

## [内置函数](https://docs.python.org/zh-cn/3/library/functions.html)

![20200519154654](https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200519154654.png)

## [内置类型](https://docs.python.org/zh-cn/3/library/stdtypes.html)

![20200519154527](https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200519154527.png)

## [内置异常](https://docs.python.org/zh-cn/3/library/exceptions.html)

```python
BaseException
 +-- SystemExit
 +-- KeyboardInterrupt
 +-- GeneratorExit
 +-- Exception
      +-- StopIteration
      +-- StopAsyncIteration
      +-- ArithmeticError
      |    +-- FloatingPointError
      |    +-- OverflowError
      |    +-- ZeroDivisionError
      +-- AssertionError
      +-- AttributeError
      +-- BufferError
      +-- EOFError
      +-- ImportError
      |    +-- ModuleNotFoundError
      +-- LookupError
      |    +-- IndexError
      |    +-- KeyError
      +-- MemoryError
      +-- NameError
      |    +-- UnboundLocalError
      +-- OSError
      |    +-- BlockingIOError
      |    +-- ChildProcessError
      |    +-- ConnectionError
      |    |    +-- BrokenPipeError
      |    |    +-- ConnectionAbortedError
      |    |    +-- ConnectionRefusedError
      |    |    +-- ConnectionResetError
      |    +-- FileExistsError
      |    +-- FileNotFoundError
      |    +-- InterruptedError
      |    +-- IsADirectoryError
      |    +-- NotADirectoryError
      |    +-- PermissionError
      |    +-- ProcessLookupError
      |    +-- TimeoutError
      +-- ReferenceError
      +-- RuntimeError
      |    +-- NotImplementedError
      |    +-- RecursionError
      +-- SyntaxError
      |    +-- IndentationError
      |         +-- TabError
      +-- SystemError
      +-- TypeError
      +-- ValueError
      |    +-- UnicodeError
      |         +-- UnicodeDecodeError
      |         +-- UnicodeEncodeError
      |         +-- UnicodeTranslateError
      +-- Warning
           +-- DeprecationWarning
           +-- PendingDeprecationWarning
           +-- RuntimeWarning
           +-- SyntaxWarning
           +-- UserWarning
           +-- FutureWarning
           +-- ImportWarning
           +-- UnicodeWarning
           +-- BytesWarning
           +-- ResourceWarning
```
