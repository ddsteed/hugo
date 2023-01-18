+++
title = "众人拾材火焰高"
author = ["RDS"]
date = 2022-04-09T10:14:00+08:00
tags = ["misc"]
categories = ["感悟"]
draft = false
+++

程序尽量要开源，这样才能更快进步。很多科研工作者好容易写了一个计算程序，掖着藏着，生怕别人偷去了。最后的结果是，无论工作多好，都无法正确继续下去。

想起一个切身体会的例子。目前用变分法处理电子与分子散射的方法和程序主要有三种：

1.  V. McKoy 等人创建的多通道 Schwinger 法；
2.  P. Burke 等人创建的 _R_-matrix (_R_ 矩阵)法；
3.  我的导师在美国的导师 C. McCurdy 等人创建的 Complex Kohn (复科恩)法。

其实，1979年国际学术年会的时候， _R_-matrix法结果还远远比不上另外两个，但到了今天，我看 _R_-matrix法快一统江湖了。虽然原因很多，但我觉得其中主要的一个原因是：Burke等人把自己开发的 _R_-matrix 程序包公开在网上，所有人都可以自由索取，并继续开发。 **众人拾材火焰高** ， _R_-matirx 法发展得越来越好。而另外两个方法，程序包只在自己的课题组内部流传，外人就算搞懂了理论，也由于没有程序可以实践，完全无法实用。

到现在，Complex Kohn 法除了 McCurdy 自己和学生在用外，其他人几乎没有用这个方法做出好的工作；多通道 Schwinger 法大概传到了巴西，Bettega 课题组大量用这个方法发文章。不过我写信去请教他们，没有任何回音。而反观 _R_-matrix 法，我从网上下载了所有的源代码，再仔细研读 Burke、Tennyson 等人的文章，竟然也可以用 _R_-matrix 方法计算实用的体系，并发表了 SCI 论文。

所以啊，一定不要“小心眼”，觉得程序公开了，别人就会“剽窃”自己的成果。其实，没有了大家的合作，不管再好的方法，最后都会束之高阁。想起当年读研究生时，看到过其他课题组有些教授不知道从哪拷了程序，连学生都不给看源码。现在，也只是尘世间的一粒灰尘而已。