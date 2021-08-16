

耗子哥： 不能忍 才能有 leadership。 如果你什么都能忍，那么你一定是韭菜


不加班 加班一定团队有问题

集中精力 不抓小兔子 能让你分心的事情太多了  要集中精力


rust cargo在安装东西的时候 国内镜像，在.cargo/config目录下写入如下国内镜像

[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"

replace-with = 'tuna'
[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"


在vscode里面打开terminal就可以看到

https://code.visualstudio.com/docs/editor/integrated-terminal


樊登：只要你讲话一慢，你的自信心就开始上升，你的掌控力出现，然后你的存在力开始浮现出来
樊登：只要你足够勤奋，所有的问题都能够得到解决，人生之败，非傲即惰，二者必居其一。勤则百弊皆除。
樊登：不要着急，不要老觉得为什么我改变了，为什么我努力了还是没有效果，一努力就要看到效果，这是小孩子才会做的事情，作为一个长大的成年人，要知道耐心是非常重要的，
    你需要把眼下的能够学到的东西慢慢的学好，然后等待着他们之间发生相互的作用。最后突然爆发的那一刻，你自己都会吃惊，自己怎么突然之间变得这么厉害。**人生需要沉淀积累才能变得强大**。

罗振宇：在公司主动结识大神，邀请别人定期的指点你，就好比找一个教练，英文单词叫coach，原意是一辆马车，然后你坐在马车上，目标一致，共同达到终点。

Bionic is an implementation of the standard C library, developed by Google for its Android operating system.[2] It differs from the GNU C Library (glibc) in being designed for devices with less memory and processor power than a typical Linux system.
https://en.wikipedia.org/wiki/Bionic_(software)

油管上搜到了一门Chris Kanich开设的系列课程 https://www.youtube.com/c/ChrisKanich/videos
感觉挺好的，这个人是UIC Associate Professor of Computer Science
他的tweet是https://twitter.com/kaytwo
其中有一节课[Code, Docker Desktop, and remote container based development environments](https://www.youtube.com/watch?v=jWZMEenRB8s)
是讲怎么使用vscode配置高效开发环境的，感觉非常受用，其中提到的一点是使用vscode的remote container来做开发
https://code.visualstudio.com/docs/remote/containers


.rel.txt has a list of relocatable entries which are basically little to do that compiler left for itself, saying when you link this program
 you need to go here, find this symbol and replace it at this place in this way.

.rel.txt等rel section类似于告诉linker to do list


Screenshot from 2021-08-16 07-47-48
这里老师提到，【preprocessor->compiler->assembler】基本上把事情都做完了，剩下的一点事情就是linker要负责的一些事情。
the last few things that a linker is in charge of. that has to do with merging all these different sections from different .o files into one final ELF executable.
and we are going to update the symbol references to their correct runtime addresses in that executable file.


https://www.youtube.com/watch?v=E804eTETaQs
在这个视屏中，老师通过对比main.o和a.out，向我们介绍了它们的内部结构以及区别。.o文件中，.rel.text会描述什么符号在哪个位置需要做什么修改，我们可以看到，目标
要修正的位置上的值是假的，等待着linker在生成executable的时候，依据.rel.txt中提供的信息，来对它们进行修正。我们通过a.out就看到了修正后的结果。

视屏中提到的，代码中会嵌入_start这种系统代码，在_start函数中某个地方会调用main函数，所以，如果我们ld一些.o文件去形成executable的话，如果里面没有main函数，
这个时候ld就会出错。

https://www.youtube.com/watch?v=nC1U1LJQL8o
In-depth: ELF - The Extensible & Linkable Format
这个视屏是介绍这个的，还没有看，先码后看


https://www.youtube.com/watch?v=l8oIupRzahU
What's so good about dynamic linking anyway?


 关于thread local storage 据说讲解的非常好的一个油管视频  先保存 后看
 https://www.youtube.com/watch?v=sjMe9aecW_A
