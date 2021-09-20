

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

樊登：**人和人的差别从来都不体现在工作的8个小时当中，而是工作之余的8个小时当中，在工作之余的时间安排才决定了人和人之间的差距**，这就是为什么在同样一个公司里面，人和人的成长速度会不一样的原因。
有很多人认为一天之中最重要的时间是工作的8个小时，早上元气满满的去上班，上完8个小时班之后，拖着疲惫的身体回到家，然后挥霍剩下的16个小时，16个小时留出一点时间睡觉，甚至很多人舍不得睡觉，就
一直在那耗耗耗，在那不停的刷剧，看电视剧。**实际上我们应该把除去上班的8小时剩下的16个小时当做是一天中的一天**，除了留出足够的睡眠时间，**剩下的时间，就是你和别人有区分的时候**。

盐sir职场宝典：迷茫的时候，一停二放，第一，停，马上停掉所有没有必要做的事情，所有的时间和精力都收回来，然后就做三件事，即，**早睡早起锻炼身体，坚持两个星期，则所有的精神和干劲就都回来了**。
二放：放开事业，放下包袱，去接触新的事物，去认识新的人，人之所以迷茫，是因为把自己陷入到一种可能性里面，而看不到其他的路，所以，有的时候，我们要拆掉思维里面的墙，
**世界上有成千上万种可能性，而你总是在迷茫中重复，这本质上就是在浪费机会**。而尝新就是打破迷茫的最好的机会，所以停，是还给你时间和精力，积蓄力量，让你整装待发。
而放，是让你发挥才能，获得新的方向。

罗振宇：在公司主动结识大神，邀请别人定期的指点你，就好比找一个教练，英文单词叫coach，原意是一辆马车，然后你坐在马车上，目标一致，共同达到终点。

我主管跟我说：公司里的人，大都只关注自己的那一小块领地，而没有去看整个系统有哪些地方是可以做优化的，是可以解决用户的痛点的，所以我们时不时的要跳出来，看看四周，看看整个系统，不要太局限自己的视野。

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
