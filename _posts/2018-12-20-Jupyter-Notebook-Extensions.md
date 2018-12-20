---
layout: post
title: Jupyter Notebook Extensions
data: 2018-12-20
excerpt: "Jupyter Notebook Extensions"
tag:
- Jupyter
- Notebook
- Extensions
comments: false
---

# Jupyter Notebook Extensions

$$Jupyter notebook$$ 虽然很方便，但是其功能却捉襟见肘，通过安装一些插件，能让你的工作效率提高不少哦！

## 极简版教程

在命令行中运行如下命令：

```shell
pip install jupyter_contrib_nbextensions && jupyter contrib nbextension install 
```

启动 $$jupyter \ notebook$$ , 并导航到新的 $$Nbextensions$$ 栏.

![jupyter notebook extensions](http://pjnlug6p5.bkt.clouddn.com/Jupyter%20Notebook%20Extensions.png)

选择你自己喜欢的插件, 当你点击你所选择的插件, 下面将会显示该插件的使用说明Demo.

(如果你没有看见扩展选项, 打开 $$notebook$$ , 单击 $$edit$$, 然后点 $$nbextensions \ config$$)

在 $$notebook$$ 的工具栏里可以看到扩展插件:

![Extensions](http://pjnlug6p5.bkt.clouddn.com/Extensions%20toolbar.png)

## 详细版教程

如果上面的教程不能满足你, 下面是一些关于$$Jupyter \ notebook$$ 插件的详细教程, 在此我列出了我最喜欢的5个插件.

### 什么是 $$notebook$$ 扩展插件?

$$Jupyter Notebook$$ 扩展插件是扩展 $$notebook$$ 环境基本功能的简单插件。它们用 $$JavaScript$$ 语言编写，会自动套用代码格式或者在单元格完成后发送浏览器通知。扩展插件目前仅支持 $$Jupyter Notebook$$ (不支持 $$Jupyter Lab$$).

为什么要使用扩展插件？$$Jupyter Notebook$$ 是一个很好用的工具，可用于教学、学习、原型设计、探索和尝试新方法（甚至可用于 $$Netflix$$ 的生产过程中）。但是，原版 $$notebook$$ 功能有限，有时令人挫败。虽然 $$Jupyter Notebook$$ 扩展插件没有完全解决这个问题，但它们确实能让你的工作变得更轻松。

### 该用哪些扩展插件?

以下是我最常用的五种 $$Jupyter \ notebook$$ 扩展插件:

* **Table of Contents: 便捷的导航栏**

  如果你在一个 $$Jupyter Notebook$$ 中同时开启了十几个单元格，那你想跟踪所有单元格就会有些困难。Table of Contents 通过添加 TOC 链接解决了这个问题，通过 TOC 链接你可以定位到页面中的任何位置。

  ![Table of Contents1](http://pjnlug6p5.bkt.clouddn.com/Table%20of%20Contests1.png)

  你还可以使用该扩展插件在 $$notebook$$ 的顶部添加一个链接目录。这样会显示你选择了哪一个目录以及哪一个正在运行：

  ![Table of Contents2](http://pjnlug6p5.bkt.clouddn.com/Table%20of%20Contents2.png)

* **Autopep8：轻轻一击就能获得简洁代码**

  我们都应该编写符合 $$pep8$$ 标准的代码，但有时你会陷入分析，难以坚持这种标准。所以当你写完代码后，只要单击这个选项，就可以让代码变得简洁漂亮。

  ![Autopep8](http://pjnlug6p5.bkt.clouddn.com/Autopep8.gif)

  这个插件可以称得上是最好的插件了，仅需点击一下，就能完成一项耗时又乏味的工作，让你专注于思考。

* **variable inspector：跟踪你的工作空间**

  variable inspector 会显示你在 $$notebook$$ 中创建的所有变量的名称，以及它们的类型、大小、形状和值。

  ![variable inspector](http://pjnlug6p5.bkt.clouddn.com/Variable.jpeg)

  这个工具对于从 RStudio 迁移过来的数据科学家来说是无价之宝。如果你不想继续打印 $$df.shape$$ 或无法重新调用 $$x$$ 的 $$type$$ ，这个工具对你来说也同样重要。

* **ExecuteTime：显示单元格的运行时间和耗时**

  我经常不知道某个单元格需要运行多久或者最后一次运行一个打开好几天的 $$notebook$$ 是什么时候。ExecuteTime 完美解决这个问题，它会显示单元格的运行完成时间和所耗时长。

  ![ExectueTime](http://pjnlug6p5.bkt.clouddn.com/ExecuteTimes.jpeg)

  的确有更好的计时方法，如 $$%%timeit$$，但 ExecuteTime 易于实现，且可以覆盖 $$notebook$$ 中的所有单元格。

* **Hide Code input：隐藏过程，展示结果**

  虽然有些人喜欢看到某项艰苦工作的具体分析，但有些人却只想看到结果。隐藏所有输入的插件让你能够立即隐藏 notebook 中的所有代码，只保留结果。

  ![Hide Code input](http://pjnlug6p5.bkt.clouddn.com/Hide%20Code%20input.gif)

  下一次如果有人说他们只想看结果，你只要单击一下就可以了。（不过你还是得经常检查代码。）

以上是我最常用的五种扩展插件，你还可以尝试其他扩展插件。先安装扩展插件，然后打开 $$notebook$$ 探索吧！

### 结论

安装 $$Jupyter Notebook$$ 扩展插件，花点时间弄清楚哪些有用，然后提高自己的工作效率。虽然这些功能不至于改变你的人生，但它们带来的益处也是值得的。而且累积起来为你节约了很多宝贵的开发时间.

如果你正在写生产代码，你可能会更愿意花些时间学习 $$IDE$$（我喜欢 $$VS Code$$），但 $$Jupyter Notebook$$ 仍然是数据科学工作流程中不可或缺的一部分.如果你要使用这个环境，就最好把它用到极致，以从中获得最大的收益.

原文链接: [Jupyter Notebook Extensions](https://towardsdatascience.com/jupyter-notebook-extensions-517fa69d2231)

