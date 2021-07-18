---
title: Dynamic Programming
author: zjh
date: 2020-06-20
last_modified_at: 2020-06-20
categories: [algorithm]
tags: [note]
---

## 引言
之前我推荐过[两本算法书](2019-12-30-computer-books.html)，现在再次翻阅回味，有了想写笔记的想法。
本篇以其中的这本书[Algorithms by Jeff Erickson](http://jeffe.cs.illinois.edu/teaching/algorithms/)的第三章为基础，
写一些笔记。

本章一开始作者从介绍音乐相关的背景知识展开叙述。所以为了能够看懂这段话，我特地补了补音乐方面的名词，可以参考这篇[博文](https://www.soundbrenner.com/blog/rhythm-basics-beat-measure-meter-time-signature-tempo/)。

### 音乐小知识

Music is made up of notes of different lengths (that’s how we have rhythm), and we measure the length of notes in beats. 
音乐由不同长度的notes组成，而note的长度以节拍(beat)为单位来衡量。

![Note VS Beat](../assets/img/2021-06-20-algorithms-dynamic-programming/note-values.png)

beat:表示节拍。
Bar or Measure (They’re the same thing!)：
Time Signature：
Tempo:表示每分钟多少拍，又叫BPM (Beats Per Minute),Tempo is the speed of the music。
Meter: Meter is a specific pattern of strong and weak pulses – or, you can think of it as the rhythmic feel of the music.



[Banburismus](https://en.wikipedia.org/wiki/Banburismus)
> Banburismus was a cryptanalytic process developed by Alan Turing at Bletchley Park in Britain during the Second World War

cryptanalytic（ 密码分析的） 



编译器是一类有着悠久历史的计算机程序。该类程序的目标是将某种编程语言写出来的计算机代码
翻译成另一种语言[<sup>1</sup>](#refer-anchor-1)。一般地，我们使用高级语言（例如C/C++、JAVA）将代码写出来，然后使
用编译器将这些代码翻译成更接近目标机器可运行的低级语言（例如汇编、字节码）。

现代编译器的工作流程一般被划分成三个阶段[<sup>1</sup>](#refer-anchor-1):

![Workflow of Modern Compiler Workflow of Modern Compiler](/assets/img/2019-10-04-llvm_getting_started/1.svg)

Workflow of Modern Compiler Workflow of Modern Compiler

前端（Front end）：我们熟悉的词法分析（lexical analysis）、语法分析（syntax analysis）和
语义分析（semantic analysis）都集中在这个阶段。目前国内大多数高校的编译原理课程还只是
停留在前端这部分。一般地，前端在验证完源码的正确性后，会将源码转换成对等的机器无关的中
间语言代码，我们称之为IR（Intermediate representation），作为中端的输入。

中端（Middle end）：中端主要是做一些机器无关的（machine-independent）优化，它的输入
是IR，输出也是IR。一般地，输出的IR比输入的IR更优。比如：常数折叠（Constant folding）、
无效代码消除（dead code elimination）等这些都常见于中端的优化中。

后端（Back end）: 后端的输入来自于中端产出的优化后的IR，后端将对这些IR进行进一步的优化
和转换，一般地，这部分的优化和转换与目标机器都是强相关的，比如：寄存器分配（register
allocation）、指令调度（instruction scheduling）。


## LLVM简介
下面将介绍下当下最优秀的编译器之一LLVM

LLVM诞生于University of Illinois at Urbana–Champaign，距今已有近20年的历史[<sup>1</sup>](#refer-anchor-1)[<sup>2</sup>](#refer-anchor-2)。LLVM
一开始的时候是Low Level Virtual Machine的缩写，后来发展发展，已经和这个全称没有太大关
系了，它现在是一个大型项目（umbrella project），包含了很多子项目，我们熟知的Clang、
LLVM Core libraries等等都在这里。

Clang可以将C/C++等高级语言写的程序转换成功能对等的LLVM的IR，它的这部分功能对应于上
图中说的前端，如果你的兴趣点在中后端的话，那么Clang可以非常省心的帮你屏蔽掉复杂的高级
语法，专心处理中后端。

LLVM Core libraries包含了很多针对LLVM IR优化的Pass，能够产生更优的功能对等的LLVM
IR，对应上图中的中端部分。值得注意的一点是，LLVM使用的IR在整个中端优化过程中都是同一
种。有些编译器，它中端优化的各个Pass之间采用的IR是特定的不一样的。这使得我们在学习和使
用LLVM IR的时候可以更关注优化算法，而不用关注我们处于何种Pass状态中。
LLVM Core libraries也包含了后端相关的代码生成模块，这块主要是将优化后的LLVM IR转换成
机器相关的代码。

## References
<div id="refer-anchor-1"></div>
[1]. [Compiler](https://en.wikipedia.org/wiki/Compiler)
<div id="refer-anchor-2"></div>
[2]. [LLVM WIKI](https://en.wikipedia.org/wiki/LLVM)
<div id="refer-anchor-3"></div>
[3]. [LLVM](https://llvm.org/)
