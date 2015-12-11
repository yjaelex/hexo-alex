title: '[转][转] 8 reasons why SPIR-V makes a big difference'
tags: []
date: 2015-11-08 16:56:07
---

转另一篇SPIR-V的文章。

<span style="font-size:24px">8 reasons why SPIR-V makes a big difference</span>

From all the news that came out of GDC, I’m most eager to talk about SPIR-V. This intermediate language[![spir-v](http://i0.wp.com/streamcomputing.eu/wp-content/uploads/2015/03/spir-v.jpeg?resize=300%2C148)](http://i0.wp.com/streamcomputing.eu/wp-content/uploads/2015/03/spir-v.jpeg)will
 make a big difference for the compute-industry. In this article I’d like to explain why. If you need a technical explanation of what SPIR-V is, I suggest you first read&nbsp;[gtruc’s
 article on SPIR-V](http://www.g-truc.net/post-0714.html)&nbsp;and then return here to get an overview of the advantages.

Currently there are several shader and c ompute languages, which SPIR-V tries to replace/support. We have GLSL, HLSL for graphics shaders, SPIR (without the V), OpenCL, CUDA and many others for compute shaders.

If you have questions after reading this article, feel free to ask them in a comment or to us&nbsp;[directly](http://streamcomputing.eu/about-us/contact/).<span id="more-9206" style="margin:0px; padding:0px; border:0px; vertical-align:baseline"></span>

# 
1\. It’s used by Vulkan

Yes, this is the number one reason. This will make SPIR-V a big standard for compute, probably bigger than OpenCL (in its current form). That’s a reason we will go big on this here at StreamComputing.

Vulkan is the stateless version of OpenGL – not even fully released, but now already the future de-facto cross-platform standard for modern graphics. Seems support for it is very complete. As it uses SPIR-V as shader language, this will greatly push SPIR-V.
 You have to understand, this is quite aggressive pushing. It unfortunately is needed with all those big corporations having their me-too languages everywhere. Once SPIR-V is everywhere, all languages that compile to SPIR-V are also supported – that probably
 will include HLSL, GLSL and various OpenCL languages.

# 
2.It supports OpenCL C, C&#43;&#43; and more

In theory also OpenGL could accept SPIR-V next to GLSL on modern graphics cards – this would avoid the need to have GLSL and SPIR-V next to each other. Same for DirectX and HLSL. There is always demand for writing one single code-base and have support for many
 platforms, so expect many languages and tools that can export to and import from SPIR-V.

<div id="attachment_9230" class="wp-caption alignnone" style="margin:5px 20px 20px 0px; padding:5px 3px 10px; border:1px solid rgb(240,240,240); vertical-align:baseline; max-width:96%; text-align:center; color:rgb(85,85,85); font-family:Merriweather,Arial,Helvetica,sans-serif; line-height:21px; width:560px">
[![spir-v_in_out](http://i0.wp.com/streamcomputing.eu/wp-content/uploads/2015/03/spir-v_in_out.png?resize=550%2C368)](http://i0.wp.com/streamcomputing.eu/wp-content/uploads/2015/03/spir-v_in_out.png)

Lots of input possibilities from GLSL and OpenCL to complete new languages. Currently only two main driver targets. “Other languages” where SPIR-V can export to, includes LLVM.

</div>

Even CUDA-kernels or HLSL could get compiled to SPIR. This means that porting can even go faster, if the host-code isn’t too complex.

> SPIR-V is the base, not for only&nbsp;<span class="share-body" style="margin:0px; padding:0px; border:0px; vertical-align:baseline">OpenCL-C, but also OpenCL-C&#43;&#43;, OpenCL-Python, OpenCL-C#, OpenCL-Julia,&nbsp;OpenCL-Java and many more. Creating a single source language> 
>  is a simple next step, by making splitting a part of the compile-phase or even using JIT.</span>

See what is already&nbsp;[coming
 around on Github](https://github.com/search?q=%22spir-v%22). I already see Python, dotNET/Linq and GLSL! No HLSL or CUDA yet –&nbsp;[ask
 us](http://streamcomputing.eu/about-us/contact/)&nbsp;for more info…

# 
3\. It’s better than the LLVM-based SPIR

Flexibility is key. LLVM has it’s own agenda, which seemed to be too different from what Khronos wanted with SPIR. A few years I wrote “[SPIR
 by example](http://streamcomputing.eu/blog/2013-12-27/opencl-spir-by-example/)” where you already see the limits of LLVM for the purposes of SPIR – important data was put in comments. SPIR-V is SPIR without limits.

[![2015-spir-v-page6](http://i2.wp.com/streamcomputing.eu/wp-content/uploads/2015/05/2015-spir-v-page6.png?resize=550%2C244)](http://i2.wp.com/streamcomputing.eu/wp-content/uploads/2015/05/2015-spir-v-page6.png)

# 
4\. Tools can do (static) analysis on SPIR-code

Many, many languages a problem? Not at all – tools just have to focus on SPIR, and the frontend-compilers need to make sure the mapping to the original source is fluent. This makes it possible to have tool-support for new kernel-languages for free.

# 
5\. It integrates very well with the existing LLVM too-chain

SPIR-V is a clean language to have as a frontend-language for LLVM. SPIR-V is expected to get an official LLVM frontend soon. For now there is&nbsp;[LunaGLASS’s
 project](https://www.phoronix.com/scan.php?page=news_item&amp;px=LunarGLASS-SPIR-V).

# 
6\. It is the same on mobile processors

As Vulkan has the same API for embedded processors, SPIR-V is also the same for mobile and desktop. There is only the checking of the GPU-capabilities, as we know from OpenCL (memories, compute capabilities), which is simpler to code.

# 
7\. It is handled by the OpenCL Runtime

[![khronos-SPIR-V-flowchart](http://i0.wp.com/streamcomputing.eu/wp-content/uploads/2015/05/khronos-SPIR-V-flowchart.png?resize=550%2C323)](http://i0.wp.com/streamcomputing.eu/wp-content/uploads/2015/05/khronos-SPIR-V-flowchart.png)

There are two options for handling SPIR-V. First is using an adapted version of the GLSL-compiler, but that won’t be able to use the full spectrum of V’s capabilities without a lot of fixing. The better solution is to adapt the OpenCL-compiler built for that
 platform – and that’s what Khronos also had in mind. Above is an image of the flow – as you see OpenCL 2.1 is the minimum version needed.

[![2015-spir-v-page8](http://i2.wp.com/streamcomputing.eu/wp-content/uploads/2015/05/2015-spir-v-page8.png?resize=550%2C99)](http://i2.wp.com/streamcomputing.eu/wp-content/uploads/2015/05/2015-spir-v-page8.png)

# 
8\. Better IP protection

Where OpenCL kernels could “accidentally” be read, when hidden in the code, SPIR-V gives better legal protection. SPIR-V has to be decoded and falls under the same laws that protect decoding of JAVA or dotNET code.

            <div>
                作者：qwertyu1234 发表于2015/11/8 16:56:07 [原文链接](http://blog.csdn.net/qwertyu1234/article/details/49719641)
            </div>
            <div>
            阅读：22 评论：0 [查看评论](http://blog.csdn.net/qwertyu1234/article/details/49719641#comments)
            </div>