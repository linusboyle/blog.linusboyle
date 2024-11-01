---
title: 任意数制补码系统
date: 2019-11-15T09:12:58.000Z
lastmod: 2020-09-06T11:30:06.000Z
tags:
- 编码
license: cc-by-nc-nd
toc: true
description: 补码系统在任意数制下的推广
---

计算机的底层数字表示方式是二进制补码(`two's complement`)，我了解它也有很长一段时间了。最近因为要实现网络层协议，涉及到IP头校验和的计算，而IP头校验和是用二进制反码(`ones' complement`)进行的。这期间为了端序、反码加法写回的问题花了不少精力。其实补码系统相当简洁，掌握了一般规律，处理二进制的特殊情况就很简单了，只不过一直一来没有关注其理论。这里简要记录一下：

假设数制为**b**，位数是**k**，那么b进制数x的**补码**的数值是$b^k  - x$

不过，使用这个式子是无法计算的，因为第一项已经发生了溢出。我们可以定义**缩小补码**为：$\text{（}       b^k - 1 \text{）} - x$

缩小补码的第一项也就是数字b-1重复k次，而它减去x就相当于对x逐位求反。因此x的补码就可以由x**逐位取反再加一**算得。

现在考虑补码系统的最主要目的：使用加法操作进行减法运算。假设我们要计算x-y，方法是把y的补码加到x上：$b^k       - y + x = (x - y) + b^k$

如果x大于等于y，那么结果就是x-y，否则发生溢出。

以上就是任意数值的补码系统，当然最广泛使用的还是二进制补码。这里多说一句，二进制反码其实就是二进制缩小补码，其英文名是*ones' complement*而不是*one's complement*。一般来讲，s在引号前代表的是数制为相应数字加一时的缩小补码，在引号后代表的是数制为相应数字的补码。

