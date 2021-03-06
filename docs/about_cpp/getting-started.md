# 如何入门C++？

## 定义入门

如果只是用`C++`写个`hello world`，那么大约需要`5`分钟。

如果要能出去找个`C++`开发的工作称之为入门，那么大概需要半年至一年的时间。

## C++包含内容

`C++`语言博大精深，很难有人精通`C++`的所有内容。当我们谈论`C++`的时候，其实包含了以下内容：

* 面向过程的编程语言`C++`
* 面向对象的编程语言`C++`
* `C++ STL`标准库
* `C++`元（模板）编程

## C++是系统编程语言

`C++`是一门系统编程语言。

`C++`与`Java`、`Python`最大的不同在于，`C++`能够”直接“操控硬件，拥有更高的性能，在对性能、实时性要求较高的领域拥有无可替代的影响力。

`C++`与它的前身`C`语言相比，它引入了面向对象的编程范式，降低了使用`C`语言构建大型项目的难度，引入`STL`标准库，解决了手动造轮子的效率问题，引入了元编程概念，使得在程序在编译器拥有更多优化空间，进一步提升`C++`性能。

## C++学习阶段

`C++`拥有比`C`语言更加复杂的语法，学习曲线更加陡峭。

如果你之前有过系统编程语言学习或开发经验，如`C`语言，那么恭喜你，`C++`的学习难度主要体现在语法上。

如果你是零基础学习`C++`，那么你不仅仅要学习`C++`灵活、强大但繁琐的语法，还要对计算机操作系统中的**内存**、CPU、进程（线程）有所涉猎。

如果你想成为`C++`开发高手，那么不仅仅要熟练掌握操作系统中各种概念和逻辑，熟练运用`Linux/Windows`编程接口，还要了解计算机组成原理，对常用的数据结构和算法熟烂于心。

如果想在某个领域有所造诣，那可能还要学习这个领域中的知识，如网络、音视频、计算机视觉、神经网络等。

如果你还想进阶，成为`C++`专家，那么你可能会从某些领域入手，如编译原理、元编程、`STL`算法优化等。

## C++学习路径

每个人的心智与学习能力不尽相同，下面是我给的学习路径：

1. C++基础学习。先快速学习一边`C++`语法基础，这个阶段不需要你把所有内容都掌握，看不懂的地方也不必深究，只需要有个印象即可。这一遍的主要作用是对`C++`有个基础的了解。
2. 操作系统学习。C++核心是内存。很多晦涩难懂的C++语法和知识，如果没有操作系统的知识做铺垫，死记硬背是无法完全掌握的。特别是操作系统对内存的管理，只有理解了操作系统如何管理内存，才能深入理解C++中指针、引用、左值、右值、容器、智能指针、动态内存、拷贝控制等内容的设计精髓。
3. 深入学习C++。这个阶段要死磕C++语法，这个阶段最好能记笔记。对于不明白的地方一定要手写代码编译运行，务必掌握`C++`的语法、面向对象的设计思想、`STL`中的容器。
4. 提升`C++`编写技巧。这个阶段可以看一些工具书，如`Effective C++`系列，可以让你在使用`C++`的时候少走弯路，少踩坑。

经过这四个阶段，可以说入门`C++`，也基本也可以胜任`C++`初级开发工程师的工作。

## C++学习课程与书籍

对于第一阶段基础学习阶段，推荐`B站`上 黑马程序员 的《C++从0到1入门编程》系列课程，学习完成课程可以基本上对C++有个大概的认知，

课程：《C++从0到1入门编程》

地址：[https://www.bilibili.com/video/BV1et411b73Z?share_source=copy_web](https://www.bilibili.com/video/BV1et411b73Z?share_source=copy_web)

第二阶段的操作系统学习阶段，推荐`B站`上 **哈工大李治军老师** 的《操作系统》，这门课以`Linux 0.11`为蓝本，详细讲解了操作系统中的 CPU调度、进程（线程）管理、内存管理、IO、信号量与锁等操作系统中的重要概念。初学时有些难度，可以反复听，佐以笔记更佳。

课程：操作系统

地址：[https://www.bilibili.com/video/BV1d4411v7u7?share_source=copy_web](https://www.bilibili.com/video/BV1d4411v7u7?share_source=copy_web)

第三阶段是深入学习C++阶段，这阶段推荐大名鼎鼎的《C++ Primer 中文版 第5版》，这本书在业内的地位不用赘述。第五版是最新版，包含`C++11`标准，虽然`C++20`标准已发布，但它仍是系统学习`C++`的首先工具书。后续我们将会发布`C++ Primer` 学习视频。

书籍：《C++ Primer 中文版 第5版》

地址：[https://www.aliyundrive.com/s/pcsfnZ58vkU](https://www.aliyundrive.com/s/pcsfnZ58vkU)，密码：`38xi`

第四阶段就是提升`C++`编程技巧阶段。此阶段推荐两本书，一本是《Effective C++》，而另一本是《More Effective C++》，两本都是`Scott Meyers`的力作，在业内也是有口皆碑。两者都是`C++98/03`的标准，有些建议按照`C++11`的标准已经过时，但仍是最值得一读的`C++`书籍之二。

书籍：《Effective C++》

地址：[https://www.aliyundrive.com/s/rE1fdjDZ3x4](https://www.aliyundrive.com/s/rE1fdjDZ3x4) 密码：`9e2x`

书籍：《More Effective C++》

地址：[https://www.aliyundrive.com/s/fhv9shgMnak](https://www.aliyundrive.com/s/fhv9shgMnak) 密码：`q3o5`

还有一些其他很好的视频书籍，但是以上推荐的视频和书籍对于C++入门已经够用了。在后续的视频和文章中，我们会推荐更多的`C++`学习视频和书籍给大家，请大家拭目以待，共同学习。

学习一门语言最好的时间是`10`年前，其次是现在。万丈高楼平地起，让我们开始攀登`C++`这座高峰吧！

