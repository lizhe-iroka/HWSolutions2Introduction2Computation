# 第一阶段编程练习3

本次作业需要对整个C/C++语言灵活运用并整合理解，主要都是面向过程编程的题目。因此需要先对题目的文本进行理解，之后将其抽象为算法，通过写程序将算法实现。

需要特别注意一下，抽象为算法这一步及其重要——想要让程序完成题目要求，一定要能做到自己拿纸一步一步算也能算出来。这里的计算的方法就是算法。如果自己没想清楚，没法直接用纸笔完成计算，那一定没办法写一个程序让它来帮我们完成计算。所以写程序时，先想清楚再开始敲键盘。

从这次作业开始，将不再在README里逐个题目详细分析代码，而是介绍每个题目的想法和怎么计算（算法）。

除此之外，如何进行debug也很重要。debug的基本思路如下。

```
1. 确保自己的算法没有出错
    1.1 如果发现算法出错了，那么就需要更正算法
2. 将代码分割为若干个片段，每个片段都完成算法中的一个比较完整的功能，之后逐个检查各个片段
    2.1 每个片段都需要检查程序是否按算法预期产生中间结果，预期的中间结果需要手工计算
    2.2 如果发现代码出错，寻找片段中可能导致这种错误的位置，进行改正
    2.3 很有可能检查不出错误，大部分情况是因为输入不够特殊，无法触发一些极端的边界条件错误或者运行时错误
    2.4 构造边界条件输入，重新检查
    2.5 构造可能触发运行时错误（内存泄漏、爆栈、除零等）的输入，重新检查
```

在debug时，需要灵活查看各个变量的状态是否符合预期，因此需要打印各个变量来进行检查。熟练的同学可以使用断点来进行调试。

## 埃博拉来袭

每一天都可能发生如下的事情：a) 有人新被传染得病，b) 病人还存活着，和c) 病人死亡，只要我们能正确得模拟这三件事就可以完成这道题目。假设我们记录了每天有多少新病人和一共有多少病人还存活，则可以有如下的算法。

```
第i天时 (i > 1)
1. 若没有人死亡 (i < Y)
    1.1 前一天存活的病人都每个人都会感染X个人，记录这些人为第i天的新病人
    1.2 新病人和前一天存活的病人都是第i天存活的病人
2. 否则有人死亡，第i-Y+1天的新病人死亡
    2.1 前一天存活的病人中去除掉死亡的病人，剩下的每个人都会感染X个人，记录这些人为第i天的新病人
    2.2 新病人和前一天存活的病人，除去死亡的病人，是滴i天存活的病人
```

按上述算法，若有初始条件，即可不断递推得到结果。初始条件显而易见，第1天有N个新病人，并且第1天有N个病人存活。

提供的代码中定义了debug函数，可以帮助在代码中打印整个数组的情况从而辅助debug。

## 求e

一步一步求取级数的各项并求和即可完成题目。每一步都由上一步的e(i-1)和当前的项f(i)计算出当前的e(i)，算法如下。

```
第i步 (i > 0)
1. 首先计算f(i)的值，f(i) = i * f(i-1)
2. 计算e(i)的值，e(i) = e(i-1) + 1 / f(i)
```

同样的，这个算法由初始条件不断递推，就能得到结果。初始条件是e(0) = 1，f(0) = 1。

## 生日蛋糕

判断蛋糕能否被均匀切成三块，且每块上都有草莓的条件有三个。

```
1. 没有草莓在圆心上
2. 没有草莓在圆外面
3. 各个草莓与圆心连线的夹角不会都小于120°
```

## 统计满足条件的四位数个数

取出每个数的个十百千位即可完成题目，取的方法如下。

```
给定四位数num，且num为整型
1. 个位 = num % 10
2. 十位 = (num / 10) % 10
3. 百位 = (num / 100) % 10
4. 千位 = num / 1000
```

## 鸡尾酒疗法

简单题目，不需要讲解。