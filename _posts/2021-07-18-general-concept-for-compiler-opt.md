---
title: 编译器优化相关文章阅读注解
author: zjh
date: 2021-07-18
last_modified_at: 2021-07-18
categories: [compiler optimisation]
tags: [note]
---

## 引言
搜索了一些关于编译器优化的一些文章，对其中一些观点进行了摘抄和注解
 

## [XL Fortran for Blue Gene/Q 14.1.0](https://www.ibm.com/docs/en/xffbg/121.141?topic=guide-optimizing-your-applications)

### [Distinguishing between optimization and tuning](https://www.ibm.com/docs/en/xffbg/121.141?topic=applications-distinguishing-between-optimization-tuning)

这里主要介绍了优化和调优这两个概念的区别

针对优化，文中提及：
> Optimization is a compiler driven process that searches for opportunities to restructure your source code and give your application better overall performance at run time, without significantly impacting development time.

> Each basic optimization technique can result in a performance benefit, although not all optimizations can benefit all applications.

针对调优，文中提及

> While optimization applies general transformations designed to improve the performance of any application in any supported environment, **tuning** offers you opportunities to adjust specific characteristics or target execution environments of your application to improve its performance. Even at low optimization levels, tuning for your application and target architecture can have a positive impact on performance. With proper tuning the compiler can:
> * Select more efficient machine instructions.
> * Generate instruction sequences that are more relevant to your application.
> * Select from more focussed optimizations to improve your code.

### [Steps in the optimization process](https://www.ibm.com/docs/en/xffbg/121.141?topic=applications-steps-in-optimization-process)
> As you begin the optimization process, consider that not all optimization techniques suit all applications. Trade-offs sometimes occur between an increase in compile time, a reduction in debugging capability, and the improvements that optimization can provide.

这里提及到优化要在很多因素中作出权衡，包括编译时间增长、debug能力削弱、与优化能带来的提升等

#### Basic Optimisation
接着介绍了X1 Fortran 的一些基础的优化
##### Optimizing at level 0
> Benefits at level 0
> * Minimal performance improvement, with minimal impact on machine resources.
> * Exposes some source code problems, helping in the debugging process.

> This level performs basic analytical optimization by **removing obviously redundant code**, and can result in better compile time. It also ensures your code is **algorithmically correct** so you can move forward to more complex optimizations. -O0 also includes some **redundant instruction elimination and constant folding**. The option -qfloat=nofold can be used to suppress folding floating-point operations. Optimizing at this level accurately preserves **all debugging information** and can expose problems in existing code, such as uninitialized variables.
> Additionally, specifying -qarch at this level targets your application for a particular machine and can significantly improve performance by ensuring your application takes advantage of all applicable architectural benefits.

##### Optimizing at level 2
> Benefits at level 2
> * Eliminates redundant code
> * Basic loop optimization
> * Can structure code to take advantage of -qarch and -qtune settings

> After successfully compiling, executing, and debugging your application using -O0, recompiling at -O2 opens your application to **a set of comprehensive low-level transformations that apply to subprogram or compilation unit scopes and can include some inlining**. Optimizations at -O2 are a relative balance between increasing performance while limiting the impact on compilation time and system resources. You can increase the memory available to some of the optimizations in the -O2 portfolio by providing a larger value for the -qmaxmem option. Specifying -qmaxmem=-1 allows the optimizer to use memory as needed without checking for limits but does not change the transformations the optimizer applies to your application at -O2.

> See the Tuning XL compiler applications topics for details on the -qarch and -qtune options.
>The -O2 option can perform a number of additional optimizations, including:
> * Common subexpression elimination: Eliminates redundant instructions.
> * Constant propagation: Evaluates constant expressions at compile-time.
> * Dead code elimination: Eliminates instructions that a particular control flow does not reach, or that generate an unused result.
> * Dead store elimination: Eliminates unnecessary variable assignments.
> * Graph coloring register allocation: Globally assigns user variables to registers.
> * Value numbering: Simplifies algebraic expressions, by eliminating redundant computations.
> * Instruction scheduling for the target machine.
> * Loop unrolling and software pipelining.
> * Moving invariant code out of loops.
> * Simplifying control flow.
> * Strength reduction and effective use of addressing modes.

上面列举了XL编译器在O2级别包含的其中一些常见的优化

> Even with -O2 optimizations, some useful information about your source code is made available to the debugger if you specify -g. Using a higher -g level increases the information provided to the debugger, but reduces the optimization that can be done. Conversely, higher optimization levels can transform code to an extent to which debugging information is no longer accurate. Use that information with discretion. 

优化会导致debug信息变得不准确。在XL编译器中，你可以了通过-g保留一些debug信息，但是保留的越多，相应的能做的优化就会减少。
我们从XL编译器上看到编译器所具有公共特征，优化和debug能力之间是具有一定的tradeoff在里面的，此消彼长。
#### Advanced Optimisation
接着介绍了X1 Fortran 的一些高级的优化
##### Optimizing at level 3

> Benefits at level 3
> * Automatic generation of SIMD instructions (-qhot=level=0)
> * In-depth Aliasing analysis
> * Better loop scheduling
> * High-order loop analysis and transformations (-qhot=level=0)
> * Inlining of small procedures within a compilation unit by default
> * Eliminating implicit compile-time memory usage limits
> * Widening, which merges adjacent load/stores and other operations
> * Pointer aliasing improvements to enhance other optimizations

> Specifying -O3 initiates more intense low-level transformations that **remove many of the limitations present at -O2**. For instance, the optimizer no longer checks for memory limits, by defaulting to -qmaxmem=-1. Additionally, optimizations encompass larger program regions and attempt more in-depth analysis. While not all applications contain opportunities for the optimizer to provide a measurable increase in performance, most applications can benefit from this type of analysis.
> Potential trade-offs at level 3
> With the in-depth analysis of -O3 comes a trade-off in terms of compilation time and memory resources. Also, since -O3 implies -qnostrict, the optimizer can alter certain floating-point semantics in your application to gain execution speed. This typically involves precision trade-offs as follows:
> * Reordering of floating-point computations.
> * Reordering or elimination of possible exceptions, such as division by zero or overflow.
> * Using alternative calculations that might give slightly less precise results or not handle infinities or NaNs in the same way.

> you can still gain most of the -O3 benefits while preserving precise floating-point semantics by specifying **-qstrict**. Compiling with -qstrict is necessary if you require the same absolute precision in floating-point computational accuracy as you get with -O0, -O2, or -qnoopt results. The option -qstrict=ieeefp also ensures adherence to all IEEE semantics for floating-point operations. If your application is sensitive to floating-point exceptions or the order of evaluation for floating-point arithmetic, compiling with -qstrict, -qstrict=exceptions, or -qstrict=order helps to ensure accurate results. You should also consider the impact of the -qstrict=precision suboption group on floating-point computational accuracy. The precision suboption group includes the individual suboptions: subnormals, operationprecision, association, reductionorder, and library (described in the -qstrict option in the XL Fortran Compiler Reference).

> Without -qstrict, the difference in computation for any one source-level operation is very small in comparison to Basic optimization. Although a small difference can be compounded if the operation is in a loop structure where the difference becomes additive, most applications are not sensitive to the changes that can occur in floating-point semantics.

##### An intermediate step: adding -qhot suboptions at level 3

> At -O3, the optimization includes minimal -qhot loop transformations at level=0 to increase performance. You can further increase your performance benefit by increasing the level and therefore the aggressiveness of -qhot. Try specifying -qhot without any suboptions, or -qhot=level=1.
> The following -qhot suboptions can also provide additional performance benefits, depending on the characteristics of your application:
> * -qhot=vector to enable long vectorization
> * -qhot=arraypad to enable array padding
> * -qhot=fastmath to enable the replacement of math routines with those from the XLOPT library

> For more information on -qhot, see High-order transformation (HOT).

> Conversely, if the application does not use loops processing arrays (which -qhot improves), you can improve compile speed with minimal performance loss by using -qnohot after -O3.

##### Optimizing at level 4

> Benefits at level 4
> * Propagation of global and argument values between compilation units
> * Inlining code from one compilation unit to another
> * Reorganization or elimination of global data structures
> * An increase in the precision of aliasing analysis

> Optimizing at -O4 builds on -O3 by triggering -qipa=level=1 which performs **interprocedural analysis (IPA), optimizing your entire application as a unit**. This option is particularly pertinent to applications that contain a large number of frequently used routines.

> To make full use of IPA optimizations, you must specify -O4 on the compilation and link steps of your application build as interprocedural analysis occurs in stages at **both compile and link time**.
> 
> Potential trade-offs at level 4
> In addition to the trade-offs already mentioned for -O3, specifying -qipa can **significantly increase compilation time**, especially at the link step.

> The IPA process
> * At compile time optimizations occur on a file-by-file basis, as well as preparation for the link stage. IPA writes analysis information directly into the object files the compiler produces.
> * At the link stage, IPA reads the information from the object files and analyzes the entire application.
> * This analysis guides the optimizer on how to rewrite and restructure your application and apply appropriate -O3 level optimizations.

> The Interprocedural analysis (IPA) section contains more information on IPA including details on IPA suboptions.
Beyond -qipa, -O4 enables other optimization options:

    -qhot

    Enables more aggressive HOT transformations to optimize loop constructs and array language.
    -qhot=vector

    Optimizes array data to run mathematical operations in parallel where applicable.
    -qarch=auto and -qtune=auto

    Optimizes your application to execute on a hardware architecture identical to your build machine. If the architecture of your build machine is incompatible with your application's execution environment, you must specify a different -qarch suboption after the -O4 option. This overrides -qarch=auto.
    -qcache=auto

    Optimizes your cache configuration for execution on specific hardware architecture. The auto suboption assumes that the cache configuration of your build machine is identical to the configuration of your execution architecture. Specifying a cache configuration can increase program performance, particularly loop operations by blocking them to process only the amount of data that can fit into the data cache.

    If you want to execute your application on a different machine, specify correct cache values.

##### Optimizing at level 5
> Benefits at level 5
> * Most aggressive optimizations available
> * Makes full use of loop optimizations and Interprocedural analysis (IPA)

> As the highest optimization level, -O5 includes all -O4 optimizations and deepens whole program analysis by increasing the -qipa level to 2. Compiling with -O5 also increases how aggressively the optimizer pursues aliasing improvements. Additionally, if your application contains a mix of C/C++ and Fortran code that you compile using the XL compilers, you can increase performance by compiling and linking your code with the -O5 option.
> 
> Potential trade-offs at level 5
> Compiling at -O5 **requires more compile time and machine resources** than any other optimization levels, particularly if you include -O5 on the IPA link step. Compile at -O5 as the final phase in your optimization process after successfully compiling and executing your application at -O4.

#### [Specialized optimization techniques](https://www.ibm.com/docs/en/xffbg/121.141?topic=applications-specialized-optimization-techniques)

##### High-order transformation (HOT)

> As part of the XL compiler optimization suite, the HOT transformations focus specifically on **loops which typically account for the majority of the execution time for most applications**. HOT transformations perform in-depth loop analysis to minimize their execution time.

> Loop optimization analysis includes:
> * Interchange
> * Fusion
> * Unrolling loop nests
> * Reducing the use of temporary arrays

> The goals of these optimizations include:
> * Reducing memory access costs through effective cache use and translation look-aside buffers (TLBs). Increasing memory locality reduces cache and TLB misses.
> * Overlapping computation and memory access through effective utilization of the hardware data prefetching capabilities.
> * Improving processor resource utilization by reordering and balancing the use of instructions with complementary resource requirements. Loop computation balance typically involves creating an equitable relationship between load/store operations and floating-point computations.

HOT short vectorization
> When you are targeting a PowerPC® processor that supports Quad Processing Extension (QPX) vectors, the default -qsimd=auto option enables the compiler to transform code into QPX vector instructions. **These machine instructions can execute up to sixteen operations in parallel. This transformation mostly applies to the loops that iterate over contiguous array data and perform calculations on each element**. You can use the NOSIMD directive to prevent the transformation of a particular loop.

HOT long vectorization

> When you specify any of the following:
> * -O4 and higher
> * -qhot with -qnostrict

> you enable -qhot=vector by default. Specifying -qnostrict with optimizations other than -O4 and -O5 ensures that the compiler looks for long vectorization opportunities. This can optimize loops in source code for operations on array data by ensuring that operations run in parallel where applicable. The compiler uses standard machine registers for these transformations and does not restrict vector data size; supporting both single- and double-precision floating-point vectorization. Often, HOT vectorization involves transformations of loop calculations into calls to specialized mathematical routines supplied with the compiler such as the Mathematical Acceleration Subsystem (MASS) libraries. These mathematical routines use algorithms that calculate results more efficiently than executing the original loop code.


HOT array size adjustment

> An array dimension that is a power of two can lead to a decrease in cache utilization. The -qhot=arraypad suboption allows the compiler to increase the dimensions of arrays where doing so could improve the efficiency of array-processing loops. Using this suboption can reduce cache misses and page faults that slow your array processing programs. The HOT transformations will not necessarily pad all arrays, and can pad different arrays by different amounts in order to gain performance. You can specify a padding factor to apply to all arrays. This value is typically a multiple of the largest array element size.

> Use -qhot=arraypad with discretion as array padding uses more memory and the performance trade-off does not benefit all applications. Also, these HOT transformations do not include checks for array data overlay, as with Fortran EQUIVALENCE, or array shaping operations.

HOT fast scalar math routines
> The XLOPT library contains faster versions of certain math functions that are normally provided by the operating system or in the default runtime. With -qhot=fastmath, the compiler replaces calls to the math functions with their faster counterparts in XLOPT library. This option requires -qstrict=nolibrary in effect.


##### Interprocedural analysis (IPA)
> Interprocedural Analysis (IPA) can analyze and optimize your application as a whole, rather than on a file-by-file basis.
> Run during the link step of **an application build, the entire application, including linked libraries,** is available for interprocedural analysis. This whole program analysis opens your application to a powerful set of transformations available only when more than one file or compilation unit is accessible. IPA optimizations are also effective on **mixed** language applications.

The following are some of the link-time transformations that IPA can use to restructure and optimize your application:

> * Inlining between compilation units.
> * Complex data flow analyses across subprogram calls to eliminate parameters or propagate constants directly into called subprograms.
> * Improving parameter usage analysis, or replacing external subprogram calls to system libraries with more efficient inline code.
> * Restructuring data structures to maximize access locality.

> In order to maximize IPA link-time optimization, you **must** use IPA at **both the compile and link step**. Objects you do not compile with IPA can only provide minimal information to the optimizer, and receive minimal benefit. However when IPA is active on the compile step, the resulting object file contains program information that IPA can read during the link step. The program information is invisible to the system linker, and you can still use the object file and link without invoking IPA. The IPA optimizations use hidden information to reconstruct the original compilation and can completely analyze the subprograms the object contains in the context of their actual usage in your application.

> During the link step, IPA restructures your application, partitioning it into distinct logical code units. After IPA optimizations are complete, IPA applies the same low-level compilation-unit transformations as the -O2 and -O3 base optimizations levels. Following those transformations, the compiler creates one or more object files and linking occurs with the necessary libraries through the system linker.

> It is important that you specify a set of compilation options as consistent as possible when compiling and linking your application. This includes all compiler options, not just -qipa suboptions. When possible, specify identical options on all compilations and repeat the same options on the IPA link step. Incompatible or conflicting options that you specify to create object files, or link-time options in conflict with compile-time options can reduce the effectiveness of IPA optimizations.
文中还详细列举了下面三个细节条目，这里就不再赘述了
* Using IPA on the compile step only
* IPA Levels and other IPA suboptions
* Using IPA across the XL compiler family

##### Using compiler reports to diagnose optimization opportunities

## LLVM简介
下面将介绍下当下最优秀的编译器之一LLVM

LLVM诞生于University of Illinois at Urbana–Champaign，距今已有近20年的历史[<sup>1</sup>](#refer-anchor-1)[<sup>2</sup>](#refer-anchor-2)。LLVM


## References
<div id="refer-anchor-1"></div>
[1]. [Compiler](https://en.wikipedia.org/wiki/Compiler)
<div id="refer-anchor-2"></div>
[2]. [LLVM WIKI](https://en.wikipedia.org/wiki/LLVM)
<div id="refer-anchor-3"></div>
[3]. [LLVM](https://llvm.org/)
