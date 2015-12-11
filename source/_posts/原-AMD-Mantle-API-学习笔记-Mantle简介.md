title: '[原]AMD Mantle API 学习笔记 -- Mantle简介'
tags: []
date: 2015-09-27 10:30:56
---

最近一段时间准备学习新的下一代graphics API，DX12和Vulkan。发现目前公开的资料不多，特别是Vulkan，kronos的速度也真慢，正式的Spec还没有出来。倒是LunarG出了个SDK和其在intel平台上实现的Vulkan driver，只是笔者一直没找到公开的代码。 

   其实，无论DX12还是Vulkan都是来自AMD的Mantle，而AMD也已经放出了公开的[Mantle API Refernce Doc](http://www.amd.com/Documents/Mantle-Programming-Guide-and-API-Reference.pdf "Mantle API"). 这份文档写的很不错，条理清晰很有价值；笔者建议有兴趣的朋友一定要拜读一下。于是笔者决定先从新标准的鼻祖mantle入手学习，结合笔者多年图形学工作的经验，希望能对下一代API能有个叫深刻的认识。在此也把自己学习体会和大家分享一下，当做读书笔记吧：）

## - Mantle 简介

mantle设计的理念和以往的API（D3D、OpenGL）完全不同；它是一个对GPU的更低级别的抽象，暴露了很多低级接口。比如mantle可以让game开发者直接操作Command Buffer；可以自己管理video memory。这中更低级的编程模型可以降低CPU的负担，从而得到更好的性能；这使得PC上的game编程更像game console了。 

Mantle大大降低了驱动的复杂度，让很多原本在驱动中做的工作移到了应用程序里。这样的好处是应用程序可以根据自身特性，利用Mantle暴露的低级别的系统接口函数，实现符合自己的效率更高的优化。相对于D3D，OpenGL，Mantle对CPU的利用效率更高；在对CPU要求高的游戏里，Performance一般大大好于D3D和OpenGL。

## Mantel 系统架构

![Mantle软件架构图](http://img.blog.csdn.net/20150909173352344) 

整个架构主要由两个模块组成：Loader和ICD驱动。

1.  Loader有点像OpenGL里面的LibGL或者OpenGL ES里的LibEGL；其主要任务是枚举系统中所有的GPU并加载该GPU对应的Vendor提供的ICD驱动。Loader应该是完全硬件无关的，它的实现是依赖于具体操作系统的。
2.  ICD驱动是硬件供应商提供的一个动态库；它可以作用于一组GPUs（比如系统中所有AMD的GPU应该共享一个ICD；而Intel的GPU会用另一个）。
3.  Shader编译库。不是必须的；其主要功能是把高级Shader语言（类C++），编译成一个Mantle可以识别的中间语言（ASM）。

这里ICD又可以大致分为三个主要模块：

*   Validation Layer（VL）。这是一个用于调试的一层，主要是用于验证应用程序API跳用的合法性。Mantle设计理念是简单快速；所以它默认只包含很少的错误检测。在Game开发阶段，我们可以打开VL来帮助调试程序。
*   窗口系统。这个和OpenGL一样。OGL标准只是定义了一组图形API；你可以用它来做离屏渲染。为了要将结果显示在屏幕上，我们就必须调用一组窗口系统函数（比如SwapBuffer）。这个窗口系统的借口在不同OS上有不同标准：Windows是wgl，LNX X-Window是GLX，Android上是EGL。Manle里面Core部分也是不知道具体窗口系统的；必须通过extension（扩展）的方式来显示渲染结果。
*   ICD的Core部分主要是将应用程序发送来的Cmd Buffers翻译成硬件相关的Cmds，并且和OS协作来把这些Cmds送给GPU去执行。

## 运行模型（EXECUTION MODEL）

Mantle的模型和以往D3D或OpenGL完全不同，见下图。这里有几个关键概念：GPU的Engine，cmd buffer和cmd buffer queues。

*   Engine。现在的GPU上其实有很多不同功能的可以执行的engine；它们都是并行的，也就是说不同engines之间可能需要同步。目前主要有Graphics queue（图形渲染），DMA（video memory之间复制数据）和Compute（通用计算）。
*   Cmd Buffer。是一段Video memory，其内容是GPU可以执行的命令（如设置寄存器或Draw，等等）。Mantle里应用程序可以直接操作cmd buffer。
*   Cmd Buffer queue。是一个队列（FIFO或者Ring，和具体实现有关）。队列中的每一项都指向一个Cmd Buffer。Queue和Engine有关。
    ![EXECUTION MODEL](http://img.blog.csdn.net/20150909182025091)
    **所以Mantle的基本编程模型是Apps主动直接地创建并填写Cmd Buffers（可以是多线程的）；在适当时候将Cmd Buffer submit到对应的Queue中去。当然在组建cmd buffer时会用到各种State Objects，许多resources（VB/IB，textures）以及resource descriptors。**
            <div>
                作者：qwertyu1234 发表于2015/9/27 10:30:56 [原文链接](http://blog.csdn.net/qwertyu1234/article/details/48765431)
            </div>
            <div>
            阅读：25 评论：0 [查看评论](http://blog.csdn.net/qwertyu1234/article/details/48765431#comments)
            </div>