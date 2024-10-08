---
title: 无类型λ演算的语法与语义
date: 2019-07-01
layout: post
tags:
- λ演算
- 语义学
license: cc-by-nc-nd
toc: true
description: 这篇文章描述了无类型的λ演算系统的语法和操作语义。
---

这篇文章描述了无类型的λ演算系统的语法和操作语义。

## 语法

形式的λ演算系统的语法是：

> [**λ表达式**] = λ[**变量**].[**λ表达式**] \| [**表达式**]
> 
> [**表达式**] = [**应用**] \| [**原子**]
> 
> [**应用**] = [**表达式**] [**原子**]
> 
> [**原子**] = [**变量**] \| [**常量**] \| [**λ表达式**]

这里，应用为左结合。λ表达式λx.(e)中约束变量x的作用域是从e左括号始，右括号止。

### 自由/约束变量

称表达式e1在表达式e2中出现，如果满足递归定义：

1. e1 = e2
2. e2 = e3 e4 其中e3是表达式，e4是原子，e1在e3或e4中出现
3. e2 = (e3) 其中e3是λ表达式，e1在e3中出现
4. e2 = λy.e3 其中y是任一变量，e3是λ表达式，e1在e3中出现

如果变量x在e中出现，且按上述第四条定义出现时，满足x在e3中自由出现且x不等于y，则称x在y中**自由出现**；如果变量x在e中出现但不是自由出现，称为**约束出现**。

如果变量x在表达式Y中只有自由出现/约束出现，则称为表达式Y的自由变量/约束变量。

### 置换

置换[N/x,M]表示用表达式N替换x在M中的自由出现。需要注意的是，在替换前后不应该改变变量的自由出现和约束出现。因此，如果M有如下形式：

$$
M = \lambda y.K, x \neq y
$$

并且满足y在N中有自由出现。为了使y在N中的自由出现不会在替换后变为约束的，需进行如下换名：

$$
[N/x, M] = \lambda z.[N/x, [z/y, K]]
$$

### 良构

形式的λ演算系统中表达式称为良构的，或称为**合式公式**，如果其所有变量或者是自由的，或者是约束的，且满足如下归纳定义

1. 在表达式中自由出现的任一变量x是良构的
2. 若M和N是良构的，则（MN）也是良构的；在M、N中自由/约束出现的变量也是自由/约束的
3. 若M是良构的，且有自由变量x，则λx.M也是良构的，x的所有出现都是约束的。

## 操作语义

### 转换

λ演算以转换的形式出现。如果M是良构的，则通过这些转换后，依旧是良构的。

#### α转换（换名）

$$
\lambda x.M \to_\alpha \lambda y.[y/x, M]
$$

如果y不同于x，且不在M中自由出现
#### β转换（应用）

$$
(\lambda x.M)N \to_\beta [N/x, M]
$$

当变量名有冲突时，需先用α转换换名

#### η转换（外延）

若x不在M中自由出现，则

$$
\lambda x.Mx \to_\eta M
$$

#### ξ转换（抽象）

若M通过以上三种转换能转换为N，则称M可以ξ转换到N

#### τ转换（展开）

$$
[N/x,M] \to_\tau ((\lambda x.M)N)
$$

如果目标是良构的，且x不在N中出现。这里[N/x, M]是语法上的写法，因此不是β的逆。

以上转换中，α，β，η，δ四种转换是可以正反两个方向进行的。当按照上面说的方向进行时，称为**约简**。

#### 允许的转换

在允许不同的转换时，得到的是强弱不同的λ演算系统。如果α，β，η，δ四种演算都被允许，称为完全λ演算系统。否则，以允许的转换类型命名。

### 标准型

除了换名之外，不能再做任何转换的表达式称为标准型。表达式M通过转换变为标准型N，则称N是M的标准型。

存在这样的λ表达式，它不能转换为标准型，比如：

$$
(\lambda x.xx)(\lambda x.xx)
$$

#### 确定性

那么考虑表达式：

$$
(\lambda x.y)((\lambda x.xx)(\lambda x.xx))
$$

似乎其结果和约简的次序有关，但是λ演算系统是**确定的**，也就是说约简结果和约简的次序无关。Church-Rosser第一定理说明了这一点：

> **Chuch-Rosser第一定理**
> 
> 如果表达式A既能转换为B，又能转换为C，则一定存在表达式D，使得B能约简为D，且C能约简为D。

上述定理保证了标准型在允许换名的意义下唯一存在，但如果约简次序不当，需要无穷次约简才能达到标准型。

#### 标准约简序列

如果约简时每一次取最左侧的待约式约简，则称为标准约简序列。如此有：

> **Chuch-Rosser第二定理**
> 
> 如果λ表达式有β标准型（在β演算系统下），则一定有β转换构成的标准约简序列得到之

以及

> **Curry定理**
> 
> 如果λ表达式有β，δ标准型（在βδ演算系统下），则一定有β和δ转换构成的标准约简序列得到之

## 递归

如果表达式由递归方程

$$
\phi = \psi(\phi)
$$
确定，且存在解，则称其为**不动点**，以最小不动点作为函数的语义。

存在一个对所有递归方程都适用的通解：

$$
Y = \lambda f. (\lambda h.f(hh))(\lambda h.f(hh))
$$

可以证明，$Y\psi$是方程的解。除此之外，$Y^n\psi, n \in Z^+$都是方程的解。

## 参考

主要参考了陆汝钤老师的《计算系统的形式语义》的数学部分。
