## 序言

![图片](img/common.jpg)

为什么要学习计算机编程？编程能够促进创造力、推理能力和问题解决能力。程序员有机会从无到有地创造东西，运用逻辑将编程结构转化为计算机可以执行的形式，当事情不像预期那样顺利时，使用问题解决技能找出出错的地方。编程是一项有趣的、有时具有挑战性（并且偶尔令人沮丧）的活动，从中获得的技能在学校和工作中都能派上用场——即使你的职业与计算机无关。而且，如果什么都没有，编程也是一个度过阴雨下午的好方式。

### 为什么选择 Python

Python 是一种易于学习的编程语言，对于初学者来说，它具有一些非常有用的功能。与其他编程语言相比，Python 的代码非常容易阅读，而且它提供了一个交互式 shell，你可以在其中输入程序并查看程序运行结果。

除了简单的语言结构和可以进行实验的交互式 shell，Python 还具有一些功能，这些功能极大地增强了学习过程，并允许你为自己创建简单的游戏动画。其中之一是 turtle 模块，灵感来自于 1960 年代 Logo 编程语言使用的 Turtle 图形，旨在用于教育用途。另一个是 tkinter 模块，它是 Tk 图形用户界面（GUI）工具包的接口，提供了一种创建稍微复杂一些的图形和动画程序的简单方法。

### 如何学习编程

就像任何第一次尝试的事情一样，最好的方法是从基础开始，所以从第一章开始，抵制跳到后面章节的冲动。没有人能在第一次拿起乐器时就演奏一首交响乐。学员飞行员在理解基本操控之前是不能飞飞机的。体操运动员通常不会在第一次尝试时就做后空翻。如果你过于急于跳跃，你不仅会发现基础概念没有记住，而且后面章节的内容会显得比实际复杂。

在你学习本书的过程中，尝试所有示例代码，以便你能够了解它们是如何工作的。大多数章节还包括一些编程难题，让你尝试，这将有助于提高你的编程技能。记住，你越是理解基础，后来理解更复杂的概念就会越容易。当你遇到困难或感到挑战时，这里有一些我觉得有用的方法：

1.  将一个问题分解成更小的部分。尝试理解一小段代码在做什么，或者只考虑一个难点的某个小部分（集中注意力在一小段代码上，而不是一次性试图理解全部内容）。

1.  如果这还不能解决问题，最好先放一放，休息一会儿，等第二天再来看看。这是解决许多问题的好方法，对于计算机程序员尤为有用。

### 谁应该阅读本书

本书适合任何对计算机编程感兴趣的人，无论是第一次接触编程的孩子还是成年人。如果你想学会如何编写自己的软件，而不是仅仅使用别人开发的程序，*Python for Kids* 是一个很好的起点。

在接下来的章节中，你将找到帮助你安装 Python、启动 Python Shell 进行基本计算、在屏幕上打印文本、创建列表并使用 if 语句和 for 循环进行简单控制流操作的信息（你还将学习什么是 if 语句和 for 循环！）。你将学习如何通过函数重用代码、类和对象的基础知识，并了解一些 Python 内置函数和模块。

你还会看到关于简单和高级海龟图形的章节，以及使用 tkinter 模块在计算机屏幕上绘图的内容。许多章节结尾处有不同复杂度的编程难题，帮助你通过自己动手写小程序来巩固新学的知识。掌握了基本的编程知识后，你将学习如何编写自己的游戏。你将开发两个图形化游戏，并了解基本的碰撞检测、事件处理和不同的动画技巧。

本书中的大多数示例使用 Python 的 IDLE（集成开发环境）Shell。IDLE 提供了语法高亮、复制粘贴功能（类似于其他应用程序的操作）以及一个编辑窗口，可以让你保存代码以供日后使用。这意味着 IDLE 既是一个交互式的实验环境，也像一个文本编辑器。虽然这些示例在标准控制台和普通文本编辑器中也能正常工作，但 IDLE 的语法高亮和稍微更人性化的环境有助于理解，因此第一章会教你如何进行设置。

### 本书内容

下面是你将在每一章中找到的内容概览。

**第一章** 是编程入门，提供了首次安装 Python 的指导。

**第二章** 介绍了基本的计算和变量，**第三章** 描述了一些基本的 Python 数据类型，如字符串、列表和元组。

**第四章** 是对海龟模块的初步了解。我们将从基础编程跳跃到让海龟（形状像箭头）在屏幕上移动。

**第五章** 介绍了条件语句和 if 语句的变化，**第六章** 则讲解了 for 循环和 while 循环。

**第七章** 是我们开始使用和创建函数的地方，接着在 **第八章** 介绍类和对象。我们涵盖了足够的基本概念，以支持本书后面游戏开发章节中的一些编程技巧。到此为止，内容开始变得有些复杂。

**第九章** 将回到 turtle 模块，帮助你尝试更复杂的图形。**第十章** 则讲解如何使用 tkinter 模块来创建更高级的图形。

在 **第十一章** 和 **第十二章** 中，我们将创建第一个游戏 *Bounce!*，它基于前面章节所学的知识，而在 **第十三章** 到 **第十六章** 中，我们将制作另一个游戏 *Mr. Stick Man Races for the Exit*。游戏开发章节是最容易出错的地方。如果遇到问题，可以从配套网站下载代码 (*[`python-for-kids.com`](http://python-for-kids.com)*)，并与这些可运行的示例代码进行比较。

最后，在 **后记** 中，我们简要介绍了如何使用 Python 包管理工具（pip）安装 PyGame 模块，并给出了一个简短的 PyGame 示例，之后还会展示其他编程语言的一些示例。

在 **附录 A** 中，你可以找到 Python 关键字的列表，在 **附录 B** 中列出了部分有用的内置函数（你将在书中的后续部分了解 *关键字* 和 *函数* 的概念）。**附录 C** 提供了常见问题的故障排除信息。

### Python for Kids 网站

如果在阅读过程中遇到困难，可以访问 *[`python-for-kids.com`](http://python-for-kids.com)* 寻求帮助。在网站上，你可以找到本书所有示例的下载链接，并提供进一步的信息，包括如何下载本书中使用的源代码。

### 祝你玩得开心！

在你学习本书的过程中，记住编程是有趣的。不要把它当作工作来看待。把编程当作一种创建有趣游戏或应用程序的方式，这些你可以与朋友或他人分享。

学习编程是一个极好的脑力锻炼过程，成果也可以非常有价值。但最重要的是，无论你做什么，享受其中的乐趣！
