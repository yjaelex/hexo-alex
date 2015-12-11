title: '[转][转] Redefining the shading languages ecosystem with SPIR-V'
tags: []
date: 2015-11-08 16:33:01
---

SPIR-V，全称**<span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px">Standard Portable Intermediate Representation</span><span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px">&nbsp;(</span><span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px">SPIR</span>**<span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px">**)**&nbsp;是一种用在GPU通用计算和图形学上的中间语言（</span>[intermediate
 language](https://en.wikipedia.org/wiki/Intermediate_language "Intermediate language")，类&#20284;汇编）；由[Khronos](https://en.wikipedia.org/wiki/Khronos_Group "Khronos Group")开发<span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px">,
 最初是为</span>[OpenCL](https://en.wikipedia.org/wiki/OpenCL "OpenCL")准备的。<span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px">&nbsp;目前的版本是
 SPIR-V，和下一代图形标准Vulkan差不多同时提出。前面版本的SPIR其实基于LLVM IR；而最新的<span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px">SPIR-V则是重新定义了一套；当然还是和LLVM IR有些类&#20284;。</span></span>

<span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px">SPIR的目的是为Shader或OpenCL kernel，提供一个平台设备无关的类汇编语言的标准。这样应用程序发布时可以使用二进制的类汇编&#26684;式Shader Binary；而不是像以前OpenGL那样直接在线编译GLSL。以前的方式需要提供Shader的源代码，这不安全。Vulkan以后会大量的走类&#20284;D3D的方式，只提供一个Shader二进制&#26684;式。</span>

<span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px">

</span>

<span style="color:rgb(37,37,37); font-family:sans-serif; font-size:14px; line-height:22.4px"></span>

At Siggraph 2014, the Khronos Group announced the development of&nbsp;[GLnext and a shader
 IL](https://www.khronos.org/assets/uploads/developers/library/2014-siggraph-bof/OpenGL-Ecosystem-BOF_Aug14.pdf). They have a name now:&nbsp;[Vulkan](https://www.khronos.org/vulkan)&nbsp;and&nbsp;[SPIR V](https://www.khronos.org/spir).

First, let's clear out something: SPIR-V has nothing to do with SPIR. It's built from scratch with marketing thinking it was a good idea to name two products with different version values. Ohhh, I can't wait to battle against the confusions this idea will lead
 to. SPIR-V is not tied to LLVM, it's a fully specified and self-contained specification. It can represent both graphics code and compute code for any API, including Vulcan, OpenCL, OpenGL, OpenGL ES, WebGL, etc.

If we trust the industry is interested in solving actual developers' problems, it will eventually be used outside the Khronos Group, for Direct3D, Metal, consoles and beyond!

The Khronos Group has published some great documents for SPIR-V:&nbsp;[a white paper](https://www.khronos.org/registry/spir-v/papers/WhitePaper.pdf),&nbsp;[a
 provisial portable core specification](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.pdf),&nbsp;[an provisial extended instructions for graphics specification](https://www.khronos.org/registry/spir-v/specs/1.0/GLSL.std.450.pdf),&nbsp;[an
 provisial extended instructions for compute specification](https://www.khronos.org/registry/spir-v/specs/1.0/OpenCL.std.21.pdf). A work in progress&nbsp;[GLSL to SPIR-V compiler](https://www.khronos.org/opengles/sdk/tools/Reference-Compiler/)&nbsp;is also available.

<div class="list" style="padding:8px 0px; font-family:verdana; font-size:14px"><span class="list" style="margin:0px">SPIR-V in a nutshell:</span>

*   Better portability
*   Better runtime performance
*   Source languages independent
*   Fully specified
*   Simple binary
*   Extendable
</div>

#### 
A current shader cross-compilation pipeline

Engines will typically use a meta-HLSL or meta shading languages for their shader code. Historically, HLSL on desktop and consoles but GLSL on mobile and CAD/DCC/professional stuff because of markets differences and reality. An engine that wants to address
 multiple markets is quickly confronted to the requirement of cross-compiling shaders from HLSL to GLSL and/or GLSL to HLSL for example with game engines. Alternatively, in the off-line rendering ecosystem, we could also imagine the issue happenning between&nbsp;[Open
 Shading Language](http://www.openshading.com/)&nbsp;and[RenderMan](https://renderman.pixar.com/resources/current/RenderMan/shadingLanguage.html).

To match the reality of the ecosystem, we have to cross compile the shading languages but this is always&nbsp;[a painful process](http://aras-p.info/blog/2014/03/28/cross-platform-shaders-in-2014/)&nbsp;as
 illustrated in figure 1.

![](http://img.blog.csdn.net/20151108173215688?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

<div class="post-image-white" style="width:816px; padding-top:0px; padding-bottom:0px; text-align:center; margin-left:auto; margin-right:auto; font-family:verdana; font-size:14px">

</div>
<div class="post-image-title" style="font-style:italic; text-align:center; padding-bottom:32px; font-family:verdana; font-size:14px">
Figure 1: Shader compilation pipeline in Unity 5&#43;</div>

Is this crazy and insane? Yes. However, it addresses the reality of the market which is complex and fragmented with a lot of legacy hardware and software. If there are reasons to use an engine, this is one. This pipeline is particularly complex but still GLSL
 remains a second class shading language in the sense that this pipeline doesn't allow to run GLSL code on Direct3D for example.

#### 
Issues with HLSL11 IL and D3D11 compiler

<div class="list" style="padding:8px 0px; font-family:verdana; font-size:14px"><span class="list" style="margin:0px">In this pipeline HLSL and HLSL11 IL have a central place but HLSL11 IL is not specified and it's produced by a closed source Windows only compiler.
 Additionally, the D3D11 compiler should be a deprecated software because:</span>

*   1 It was designed for legacy vec4 GPUs which implies wasting 50% of registers on vec2 and %25% on vec3\. Considering that most GPUs hide memory access latencies by launching as many wavefronts as possible, GPUs can only launch new wavefronts when there is still
 space available in the register file. Thus, D3D11 compiler cost a great deal of performance by wasting registers.
*   2 HLSL11 doesn't expose many hardware features ([gl_DrawID](https://www.opengl.org/registry/specs/ARB/shader_draw_parameters.txt),&nbsp;[pixel
 local storage](https://www.khronos.org/registry/gles/extensions/EXT/EXT_shader_pixel_local_storage.txt),&nbsp;[framebuffer fetch](https://www.khronos.org/registry/gles/extensions/EXT/EXT_shader_framebuffer_fetch.txt)). How to express these features in HLSL when we want to use them
 in GLSL where they are available? Well, it's hard and at best possible through ugly hacks!
*   3 D3D11 compiler performs destructive &quot;optimizations&quot; on the input shaders. Maybe, these &quot;optimizations&quot; where ok at some points in history but they prevent GPU vendors to properly optimize the shaders because source information gets lost. From where comes
 from these massive performance gains from hotfix drivers on new AAA released games? At least, part of it from shaders replacements. If HLSL11 IL input is A, use this totally different binary. This process is probably ok for hardware vendors that can afford
 it but this process won't scale beyond some flagship AAA games. Hence, the rest of us, the 99%, are paying the price of a poor compiler.
</div>

#### 
Easier and more robust cross compilation with SPIR-V

SPIR-V is a fully specified, cross APIs, binary intermediate language which is easy to read, to extend or to ignore unknown instructions without destructing the source in a tool chain.

<div class="post-image-white" style="width:816px; padding-top:0px; padding-bottom:0px; text-align:center; margin-left:auto; margin-right:auto; font-family:verdana; font-size:14px">
![](http://img.blog.csdn.net/20151108173315148?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

</div>
<div class="post-image-title" style="font-style:italic; text-align:center; padding-bottom:32px; font-family:verdana; font-size:14px">
Figure 2: A more desiable shader compilation pipeline to match the market reality</div>

In figure 2 shows how SPIR-V could be used as a center piece of the compilation pipeline allowing multiple front-ends / languages to produce the SPIR-V IL. On platforms supporting SPIR-V, we could directly feed APIs with SPIR-V. The reality is that it will
 take a lot of time to transform the ecosystem and we need a solution for others platforms (eg: shipped mobiles which will never get new drivers). Hence, in a market real shader compilation pipeline, SPIR-V would need to be converted to HLSL9 IL, HLSL11 IL,
 GLSL, Metal, etc.

This is a lot of work but SPIR-V provides a simpler and more robust approach to cross compile to others ILs and languages.&nbsp;[HLSLcc](https://github.com/James-Jones/HLSLCrossCompiler)&nbsp;is a great
 tool that demonstrated that cross compilation at IL level is a good direction. Another example is&nbsp;[IL2CPP](http://blogs.unity3d.com/2014/05/20/the-future-of-scripting-in-unity/)&nbsp;used by Unity
 to cross compile C# to C&#43;&#43;. However, HLSL11 IL has many issues, as expressed previously. With SPIR-V, the source IL is fully specified and extendable making the translation from SPIR-V to HLSL11 IL easier (or even possible) than HLSL11 IL to SPIR-V for example.

I am glade to see that some frameworks are already&nbsp;[investigating about SPIR-V](https://jogamp.org/bugzilla/show_bug.cgi?id=1140#c2):&nbsp;[Jogamp](https://jogamp.org/)&nbsp;is
 a Java binding for multiple APIs including OpenGL and OpenCL. SPIR-V will allow the framework users to target independently OpenGL compute of OpenCL depending framework users needs for example.

<div class="post-image-white" style="width:816px; padding-top:0px; padding-bottom:0px; text-align:center; margin-left:auto; margin-right:auto; font-family:verdana; font-size:14px">
![](http://img.blog.csdn.net/20151108173349740?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

</div>
<div class="post-image-title" style="font-style:italic; text-align:center; padding-bottom:32px; font-family:verdana; font-size:14px">
Figure 3: Dreaming of a large market adoption, wish: that's the plan 5 years down this line</div>

In figure 3, we show what would be the ultimate goal for SPIR-V: A massive adoption of the shading intermediate language so that we only have focus on innovations in the source language world. Great time to start a PHD on shading language!

In the current ecosystem, we have to generate N shaders per source because Direct3D 9, Direct3D 11, OpenGL and OpenGL ES accept different syntaxes. In many cases, only the syntax sugar is different but the functionality is exactly the same from API to API.
 The multiplication of generated shaders has a production cost in iteration time and latency until we get the results that SPIR-V could ultimately avoid. One SPIR-V =&gt; N platforms. Ideally, only hardware feature levels would condition the generation of multiple
 shader outputs.

#### 
Building bridges between ecosystems

Supporting multiple source languages as first class citizen is valuable for many reasons: The user case choice the shading language he like best but also to build bridges between ecosystem: off-line rendering and real-time rendering; Mobile and desktop; Simulation
 and rendering,; etc!

Let's take a common real-time use case: the case of skinning. For some games it might be valuable to do the skinning using a computer shader so that this work will be done on the GPU. However, some games might be GPU bound so to get better performance, it's
 probably a good idea to rely on the CPU. Let's imagine that&nbsp;[ISPC](https://github.com/ispc/ispc)&nbsp;decides to support SPIR-V just like it supports&nbsp;[NVIDIA
 PTX](https://github.com/ispc/ispc/blob/master/ptxtools/ptx.ll)&nbsp;for example. In such case an engine could create a single parallel friendly and optimized code and a game could choose either a GPU or CPU target for skinning depending on its needs.

Futhermore, SPIR-V allows to decouple the tools from the shipping software. There is no reason to ship a SPIR-V compiler such as ISPC or LLVM in a final game. It would increase export time and take a lot of space which is not desiable on mobile or WebGL platforms
 where memory is critical.

#### 
Better matching of the engine reality: custom shading languages!

If we consider engine shading languages, most are actually meta-HLSL using custom syntax to expose built-in shaders, engine specific functionality, fallback systems, making easier/possible cross compilation and platform targeting or offering a more natural
 way to expose shaders to technical shader artists.&nbsp;[Unity shading language](http://docs.unity3d.com/Manual/SL-Reference.html)&nbsp;is a good example of such meta-HLSL approach.

SPIR-V is offering us the opportunity to officialize these languages and even build the custom engine functionalities in their hearts through extended instruction sets. SPIR-V is built around a model where unrecognized blocks are just ignored and preserved,
 allowing interactions with SPIR-V tools unaware of these extensions.

Going crazier, we could imagine in the future storing OpenGL states in SPIR-V or describing rendering passes with off-line tools capable to filter redundant states changes. Many opportunities to run our imagination wild!

On the opposity side, we could imagine innovations to design a shading language friendly for technical artists, exposing&nbsp;[PBR](http://www.frostbite.com/2014/11/moving-frostbite-to-pbr/)&nbsp;parametrization
 for example or decoupling&nbsp;[surface shader, lighting model and light shader](http://aras-p.info/blog/2009/05/07/shaders-must-die-part-2/)&nbsp;natively in the language.

#### 
NOT resolved by SPIR V: &quot;Ohoho, on line shader compilation time is too long.&quot;

Some game developers would say that we need an IL for better online compilation performance. I think this is misunderstanding the cause of this issue. The slow part of the GLSL compilation isn't the translation from GLSL to IHVs IL (eg&nbsp;[NVIDIA
 PTX](http://docs.nvidia.com/cuda/parallel-thread-execution/#abstract)&nbsp;or&nbsp;[AMD IL](http://amd-dev.wpengine.netdna-cdn.com/wordpress/media/2012/10/AMD_Intermediate_Language_%28IL%29_Specification_v2.pdf)) but the optimizations path on the IHVs IL, particularly
 register allocation and scheduling transformation. These operations are fully dependent of GPU architectures hence should be performed by a targeted platform hardware vendor compiler.

Parsing GLSL / HLSL and performing non destruction optimizations have a cost and moving these off-line will give us some online performance gains. However, following the&nbsp;[80-20
 rule](http://en.wikipedia.org/wiki/Pareto_principle), we should expect only 20% performance gain. Compilation is a finer art than just parsing languages.

#### 
NOT fully resolved by SPIR V: &quot;Ohoho, IHVs shader compilers are rubish.&quot;

A lot of issues with current GLSL compilers is that all of them support invalid syntaxes but hardware vendors don't want to fix them because it could break some applications. Removing the language syntax avoid potential errors but if tools generate invalid
 SPIR-V code, chances are that drivers will still do whatever it takes to garantee the IL works on their implementations. From an IHVs point of view, the worse is an application that doesn't work on their platforms by run fine else where.

By design, SPIR-V doesn't prevent IHVs to run invalid code so something else would need to be done to resolve this issue. Conformance tests? This said, I think that the simplicity of SPIR-V brings us to a much better state but the larger potential SPIR-V ecosystem
 than GLSL makes it required to ensure SPIR-V code quality.

The other issue is that some shader compilers are terrible at performance trivial optimizations hence we will probably need to have a shader pipeline path to do this job.

#### 
Better integration of the shading language in modern graphics APIs:

<div class="post-image-white" style="width:816px; padding-top:0px; padding-bottom:0px; text-align:center; margin-left:auto; margin-right:auto; font-family:verdana; font-size:14px">
![](http://img.blog.csdn.net/20151108173431099?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

</div>
<div class="post-image-white" style="width:816px; padding-top:0px; padding-bottom:0px; text-align:center; margin-left:auto; margin-right:auto; font-family:verdana; font-size:14px">
<span style="font-style:italic">Figure 4: Integration of SPIR-V in an explict graphics API such as Vulkan</span></div>

Figure 4 shows a potential integration of SPIR-V in Vulkan. Basically, with OpenGL and GLSL, all the cross compilation can be done off-line but the rest, including the actual shader compilations, needs to happen in the rendering thread. With Vulkan, we can
 decouple the tasks and guarantee that the rendering loop never compile shaders because of shader patching, play with shader cache or validate states. Additionally, we have a full control other the threaded shader compilations. With OpenGL, if we don't query
 the compilation results right after requesting a compilation, the drivers may thread and hide the compilation time but this behavior is not specified: Would the drivers actually thread the compilation? How many compilations can happen in parallel? Vulkan is
 an explict API, hence we have control for each step, when, where and how these steps should be performed.

#### 
Time for celebrations before hard work for an ecosystem revolution!

I am absolutely convinced that SPIR-V is a game changer for the industry just like WebGL before it. However, there is no magic. To become really successful, it will take a lot of efforts to ensure adoption by hardware and platform vendors but also to adapt
 SPIR-V to the reality of the market and build (open source!) cross compilation tools placing SPIR-V at their centers.

The head lines might be all about iPhone6 and Samsung S6 but the reality of the market is that we are seeing a race to the bottom with lot of old ES2 GPU IP shipping everyday. Furthermore, there is already a lot of devices already shipped that will never see
 the color of a drivers update during their entire life time. Finally, Vulkan might be great and all but realistically it will take years for the ecosystem to complete this transition. Meanwhile, there is a lot of OpenGL/ES, WebGL, Direct3D, console APIs out
 there. SPIR-V is solution for all of them!

I am looking forward the shading language revolution that SPIR-V will lead to, one step at a time!

Enjoy!

I cannot express how awesome this is. :-P<span class="quote-author" style="font-style:normal; text-align:right; display:block">[John W
 Kloetzli, Jr](https://twitter.com/JJcoolkl/status/572791033551036417)</span>

*   [SPIR-V: whitepaper](https://www.khronos.org/registry/spir-v/papers/WhitePaper.pdf)
*   [SPIR-V 0.99, provisiontal specification](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.pdf)
*   [SPIR-V Extended Instructions for GLSL, provisiontal specification](https://www.khronos.org/registry/spir-v/specs/1.0/GLSL.std.450.pdf)
*   [SPIR-V OpenCL 2.1 Extended Instruction Set, provisiontal specification](https://www.khronos.org/registry/spir-v/specs/1.0/OpenCL.std.21.pdf)
*   [SPIR-V front page](https://www.khronos.org/spir)
*   [Vulkan front page](https://www.khronos.org/vulkan)

            <div>
                作者：qwertyu1234 发表于2015/11/8 16:33:01 [原文链接](http://blog.csdn.net/qwertyu1234/article/details/49719399)
            </div>
            <div>
            阅读：31 评论：0 [查看评论](http://blog.csdn.net/qwertyu1234/article/details/49719399#comments)
            </div>