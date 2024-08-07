---
title: 计算机语言的接受性与确定性
layout: post
date: 2019-08-09
tags: 
- 形式语义学
- 计算机语言
license: cc-by-nc-nd
toc: true
description: 接受性:Receptive/确定性:Determinate
---

在语言学里有Receptive/Expressive Language的说法，大概是说语言本身偏向于理解还是表达。比如，当语言写在书本里，主要供人阅读，那就是接受性（感受性）的；如果是用来和别人交流，那就是表达性的。

计算机语言的性质之中也有Receptive，但是意义和上述不同。由于我没有找到关于这方面的中文资料，姑且按照语言学的译法，叫它“接受性”。而确定性则是另一个常见的语言性质，这里简单说明一下其意义。

语言的小步操作语义，是建立在一个转移关系“$\implies$”上的。比如，状态之间的转移，或者某种环境的转移。一次转移可以是单纯的计算（包括形式上的调整，比如**继续语义**），也可以是非纯的I/O操作（包括外部可见的虚拟内存读写）。

可以用标记来表示转移的类别，比如p（纯）, $i_x$（输入） 和 $o_x$（输出）。下标x代表的是输入/输出的值。我们可以定义标记上的一个等价关系=为：

$$
\begin{align}
&p = p \\
&\forall x \forall y, i_x = i_y \\
&\forall x, o_x = o_x \\
\end{align}
$$

也就是说，如果两者同为输入标记，则不必考虑输入值;而输出不同，必是输出相同值时才认为相等；纯转移的标记自然是相等的。

如果一门程序设计语言（的语义）是确定的，当且仅当:

$$
\begin{align}
&a \implies^{l1} b \quad and \quad a \implies^{l2} c => l1 = l2 \\
&a \implies^{l} b \quad and \quad a \implies^{l} c => b = c \\
\end{align}
$$

即：只有输入的不同会导致不确定性。

语言是接受的，如果：

$$
a \implies^{l1} b \quad and \quad l1 = l2 => \exists c, a\implies^{l2}c
$$

即：若能转移，则可转移性不依赖于输入。
