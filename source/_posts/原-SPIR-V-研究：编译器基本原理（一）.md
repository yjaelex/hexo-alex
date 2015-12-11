title: '[原]SPIR-V 研究：编译器基本原理（一）'
tags: []
date: 2015-12-03 17:31:05
---

# SPIR-V 研究：编译器基本原理（一）

前面转过两篇关于SPIR-V 中间语言的介绍；接下来笔者准备深入学习一下SPIR-V的标准。根据标准，**SPIR-V是以一种二进制格式存在的，并且函数还是以控制流图CFG的形式存在；数据结构也保留了高级语言里的层级关系**。（[https://en.wikipedia.org/wiki/Standard_Portable_Intermediate_Representation](https://en.wikipedia.org/wiki/Standard_Portable_Intermediate_Representation)） 

这样做的目的是为了更好的在目标平台上进行优化；同时Khronos也放出了官方标准的开源编译器[Glslang](https://github.com/KhronosGroup/glslang)。 所以，为了更好的了解SPIR-V，我们有必要先温习一下编译器的基本原理，特别是前端的词法分析、语法分析、语义分析和中间语言生成。

* * *

## 编译器基本结构

下图是一个一般编译器的编译高级语言的过程。 

![这里写图片描述](http://img.blog.csdn.net/20151203172900409) 

基本上就是预处理（比如C/C++里宏汇编），编译并汇编（每个源码文件都生成对应的.o文件，这时的代码是relocatable的），链接生成可执行文件（linker，把许多.o文件以及用到的库函数.lib文件合并并作全局优化），最后OS的加载器会加载可执行文件并运行之（Windows上是PE格式，Linux上市ELF格式）。具体细节可以参考[Linkers and Loaders](http://www.amazon.com/Linkers-Kaufmann-Software-Engineering-Programming/dp/1558604960) 这本书。

下面我们重点看编译器的结构。 

![这里写图片描述](http://img.blog.csdn.net/20151203173049347) 

这是一般编译器的结构，主要分前端和后端，以中间语言生成为界限。前端主要是分析语法语义并生成代码的中间表示，这主要包括lexer 和 parser，以及语义分析；后端主要是生成目标机器代码，包括分析并优化，寄存器分配等。 

下图是编译器的主要编译过程（phase）。 

![这里写图片描述](http://img.blog.csdn.net/20151203172941801) 

**这里我们研究的目标SPIR-V就是一种标准化的中间语言表示。**所以我们更关心的是前端的研究，这也是编译器里比较成熟，相对简单的部分。以前的OpenGL里，GLSL写成的Shader是高级语言（相当于C++)，OpenGL的驱动会把它编译成显卡GPU对应的机器码。所以各家会有不同的中间表示；Vulkan会统一标准用SPIR-V了。 

![这里写图片描述](http://img.blog.csdn.net/20151203173018532)

## 计算机语言

计算机科学里的语言可以看作是一个字母表（alphabet ）上的某些有限长字符串的集合；这一般可以包含无限多个字符串，也就是无限集合（Infinite Sets）。这里有限长字符串就是“sentences”；sentences是word（或token）组成，它有一定的结构；token则是letter由一定规则组成；letter个数是有限的，它们全部取自字母表（alphabet ）。 

这是个有层次的定义。

<table>
<thead>
<tr>
  <th align="left">Layer</th>
  <th align="right">分析对象</th>
  <th align="center">组成元素</th>
</tr>
</thead>
<tbody><tr>
  <td align="left">词法（Lexical structure ）</td>
  <td align="right">token</td>
  <td align="center">letter</td>
</tr>
<tr>
  <td align="left">语法（Syntactic structure）</td>
  <td align="right">sentences</td>
  <td align="center">token</td>
</tr>
<tr>
  <td align="left">语义（Semantics）</td>
  <td align="right">语法树</td>
  <td align="center"></td>
</tr>
</tbody></table>

语言是有一定的语法规则的（grammar）。而grammar是一组有限的规则的集合；计算机语言里的grammar是不同于一般自然语言的语法，它可以构造出所有可能的sentences。所以这是一个利用有限规则来产生无限的合法语句的问题。 

而grammar又可以用正则表达式来定义；比如GLSL的语法[glslang.y](https://github.com/KhronosGroup/glslang/blob/master/glslang/MachineIndependent/glslang.y)。

            <div>
                作者：qwertyu1234 发表于2015/12/3 17:31:05 [原文链接](http://blog.csdn.net/qwertyu1234/article/details/50163847)
            </div>
            <div>
            阅读：7 评论：0 [查看评论](http://blog.csdn.net/qwertyu1234/article/details/50163847#comments)
            </div>