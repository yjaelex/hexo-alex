title: '[转][转]The Mali GPU: An Abstract Machine, Part 1 - Frame Pipelining'
tags: []
date: 2015-12-03 09:55:35
---

转三篇ARM一个牛人写的怎样优化OpenGL ES应用程序的文章。该系列三篇文章，深入浅出介绍了OpenGL ES API的背后实现和

ARM GPU硬件架构。其中第一篇是所有图形API都通用的概念；后面介绍了Tile-based rendering和ARM自己Shader Core。

这是第一篇，有中英文对照的。![微笑](http://static.blog.csdn.net/xheditor/xheditor_emot/default/smile.gif)

## 1\. 英文原文

Optimization of graphics workloads is often essential to many modern mobile applications, as almost all rendering is now handled directly or indirectly by an OpenGL ES based rendering back-end. One of my colleagues,&nbsp;[Michael
 McGeagh](https://community.arm.com/people/mcgeagh)<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:inherit; vertical-align:baseline">&nbsp;, recently posted a work guide [</span>[http://community.arm.com/docs/DOC-8055](https://community.arm.com/docs/DOC-8055)<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:inherit; vertical-align:baseline">]
 on getting the ARM® DS-5™ Streamline™ profiling tools working with the Google Nexus 10 for the purposes of profiling and optimizing graphical applications using the Mali™-T604 GPU. Streamline is a powerful tool giving high resolution visibility of the entire
 system’s behavior, but it requires the engineer driving it to interpret the data, identify the problem area, and subsequently propose a fix.</span>

&nbsp;

For developers who are new to graphics optimization it is fair to say that there is a little bit of a learning curve when first starting out, so this new series of blogs is all about giving content developers the essential knowledge they need to successfully
 optimize for Mali GPUs. Over the course of the series, I will explore the fundamental macro-scale architectural structures and behaviors developers have to worry about, how this translates into possible problems which can be triggered by content, and finally
 how to spot them in Streamline.

&nbsp;

## 
Abstract Rendering Machine

&nbsp;

The most essential piece of knowledge which is needed to successfully analyze the graphics performance of an application is a mental model of how the system beneath the OpenGL ES API functions, enabling an engineer to reason about the behavior they observe.

&nbsp;

To avoid swamping developers in implementation details of the driver software and hardware subsystem, which they have no control over and which is therefore of limited value, it is useful to define a simplified abstract machine which can be used as the basis
 for explanations of the behaviors observed. There are three useful parts to this machine, and they are mostly orthogonal so I will cover each in turn over the first few blogs in this series, but just so you know what to look forward to the three parts of the
 model are:

&nbsp;

*   The CPU-GPU rendering pipeline
*   Tile-based rendering
*   Shader core architecture

&nbsp;

In this blog we will look at the first of these, the CPU-GPU rendering pipeline.

&nbsp;

## 
Synchronous API, Asynchronous Execution

&nbsp;

The most fundamental piece of knowledge which is important to understand is the temporal relationship between the application’s function calls at the OpenGL ES API and the execution of the rendering operations those API calls require. The OpenGL ES API is specified
 as a synchronous API from the application perspective. The application makes a series of function calls to set up the state needed by its next drawing task, and then calls a&nbsp;<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:'courier new',courier; vertical-align:baseline">glDraw</span><sup><small style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:0.92em; font-family:inherit; vertical-align:baseline">[1]</small></sup>&nbsp;function
 — commonly called a draw call — to trigger the actual drawing operation. As the API is synchronous all subsequent API behavior after the draw call has been made is specified to behave as if that rendering operation has already happened, but on nearly all hardware-accelerated
 OpenGL ES implementations this is an elaborate illusion maintained by the driver stack.

&nbsp;

In a similar fashion to the draw calls, the second illusion that is maintained by the driver is the end-of-frame buffer flip. Most developers first writing an OpenGL ES application will tell you that calling&nbsp;<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:'courier new',courier; vertical-align:baseline">eglSwapBuffers</span>&nbsp;swaps
 the front and back-buffer for their application. While this is logically true, the driver again maintains the illusion of synchronicity; on nearly all platforms the physical buffer swap may happen a long time later.

&nbsp;

### 
Pipelining

&nbsp;

The reason for needing to create this illusion at all is, as you might expect, performance. If we forced the rendering operations to actually happen synchronously you would end up with the GPU idle when the CPU was busy creating the state for the next draw
 operation, and the CPU idle while the GPU was rendering. For a performance critical accelerator all of this idle time is obviously not an acceptable state of affairs.

[![gles-sync.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-2755-6948/gles-sync.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-2755-6948/gles-sync.png)

&nbsp;

To remove this idle time we use the OpenGL ES driver to maintain the illusion of synchronous rendering behavior, while actually processing rendering and frame swaps asynchronously under the hood. By running asynchronously we can build a small backlog of work,
 allowing a pipeline to be created where the GPU is processing older workloads from one end of the pipeline, while the CPU is busy pushing new work into the other. The advantage of this approach is that, provided we keep the pipeline full, there is always work
 available to run on the GPU giving the best performance.

&nbsp;

[![gles-async.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-2755-6949/gles-async.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-2755-6949/gles-async.png)

&nbsp;

The units of work in the Mali GPU pipeline are scheduled on a per render-target basis, where a render target may be a window surface or an off-screen render buffer. A single render target is processed in a two step process. First, the GPU processes the vertex
 shading<sup><small style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:0.92em; font-family:inherit; vertical-align:baseline">[2]</small></sup>&nbsp;for all draw calls in the render target, and second, the fragment shading<sup><small style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:0.92em; font-family:inherit; vertical-align:baseline">[3]</small></sup>&nbsp;for
 the entire render target is processed. The logical rendering pipeline for Mali is therefore a three-stage pipeline of: CPU processing, geometry processing, and fragment processing stages.

&nbsp;

[![gles-mali.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-2755-6950/gles-mali.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-2755-6950/gles-mali.png)

&nbsp;

### 
Pipeline Throttling

&nbsp;

An observant reader may have noticed that the fragment work in the figure above is the slowest of the three operations, lagging further and further behind the CPU and geometry processing stages. This situation is not uncommon; most content will have far more
 fragments to shade than vertices, so fragment shading is usually the dominant processing operation.

&nbsp;

In reality it is desirable to minimize the amount of latency from the CPU work completing to the frame being rendered – nothing is more frustrating to an end user than interacting with a touch screen device where their touch event input and the data on-screen
 are out of sync by a few 100 milliseconds – so we don’t want the backlog of work waiting for the fragment processing stage to grow too large. In short we need some mechanism to slow down the CPU thread periodically, stopping it queuing up work when the pipeline
 is already full-enough to keep the performance up.

&nbsp;

This throttling mechanism is normally provided by the host windowing system, rather than by the graphics driver itself. On Android for example we cannot process any draw operations in a frame until we know the buffer orientation, because the user may have rotated
 their device, changing the frame size. SurfaceFlinger — the Android window surface manager – can control the pipeline depth simply by refusing to return a buffer to an application’s graphics stack if it already has more than N buffers queued for rendering.

&nbsp;

If this situation occurs you would expect to see the CPU going idle once per frame as soon as “N” is reached, blocking inside an EGL or OpenGL ES API function until the display consumes a pending buffer, freeing up one for new rendering operations.

&nbsp;

[![gles-mali-throttle.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-2755-6951/gles-mali-throttle.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-2755-6951/gles-mali-throttle.png)

&nbsp;

This same scheme also limits the pipeline buffering if the graphics stack is running faster than the display refresh rate; in this scenario content is &quot;vsync limited&quot; waiting for the vertical blank (vsync) signal which tells the display controller it can switch
 to the next front-buffer. If the GPU is producing frames faster than the display can show them then SurfaceFlinger will accumulate a number of buffers which have completed rendering but which still need showing on the screen; even though these buffers are
 no longer part of the Mali pipeline, they count towards the N frame limit for the application process.

&nbsp;

[![gles-mali-vsync.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-2755-6952/gles-mali-vsync.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-2755-6952/gles-mali-vsync.png)

As you can see in the pipeline diagram above, if content is vsync limited it is common to have periods where both the CPU and GPU are totally idle. Platform dynamic voltage and frequency scaling (DVFS) will typically try to reduce the current operating frequency
 in these scenarios, allowing reduced voltage and energy consumption, but as DVFS frequency choices are often relatively coarse some amount of idle time is to be expected.

&nbsp;

### 
Summary

&nbsp;

In this blog we have looked at synchronous illusion provided by the OpenGL ES API, and the reasons for actually running an asynchronous rendering pipeline beneath the API. Tune in&nbsp;[next
 time](https://community.arm.com/groups/arm-mali-graphics/blog/2014/02/20/the-mali-gpu-an-abstract-machine-part-2), and I’ll continue to develop the abstract machine further, looking at the Mali GPU’s tile-based rendering approach.

&nbsp;

Comments and questions welcomed,

Pete

&nbsp;

### 
Footnotes

&nbsp;

*   [1] There are many OpenGL ES draw functions which draw things, it doesn't really matter which one you pick for this example.
*   [2]&nbsp;[Vertex
 (computer graphics) - Wikipedia, the free encyclopedia](https://community.arm.com/external-link.jspa?url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FVertex_%28computer_graphics%29)
*   [3]&nbsp;[Fragment
 (computer graphics) - Wikipedia, the free encyclopedia](https://community.arm.com/external-link.jspa?url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FFragment_%28computer_graphics%29)

## 2\. 中文翻译

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">图形工作负载的优化对于许多现代移动应用程序而言往往必不可少，因为几乎所有渲染现在都直接或间接地由基于</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;OpenGL

ES&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">的渲染后端负责处理。我的同事</span>&nbsp;<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:Arial,sans-serif; vertical-align:baseline">[<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; color:rgb(4,129,165)">Michael
 McGeagh</span>](https://community.arm.com/people/mcgeagh)</span>&nbsp;<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">最近发表了一篇工作指南</span>&nbsp;<span style="margin:0px; padding:0in; border:1pt windowtext; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline">[</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:Arial,sans-serif; vertical-align:baseline">[<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; color:rgb(4,129,165)">http://community.arm.com/docs/DOC-8055</span>](https://community.arm.com/docs/DOC-8055)</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">]</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">，介绍如何将</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;ARM®DS-5™
 Streamline™&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">性能分析工具用于</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;Google
 Nexus 10</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">，对利用</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">Mali™-T604
 GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">的图形应用程序进行性能分析和优化。</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">Streamline&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">是一款强大的工具，能够深入细致地洞悉整个系统的行为，但也需要驾驭它的工程师能够解读相关数据，识别问题区域，进而提出修复建议。</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">对于初涉图形优化的开发人员而言，起步阶段总会遇到一些困难，所以我写了新的系列博文，给开发人员提供必要的知识，以便他们能够成功地针对</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;Mali
 GPU<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline">进行优化。在整个系列博文中，我将阐述开发人员必须要考虑的基本宏观体系结构和行为、这些因素如何转化为能被内容触发的潜在问题，以及最终如何在</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; vertical-align:baseline">

Streamline&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline">中找出这些问题。</span></span>

&nbsp;

## 
<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:15pt; font-family:SimSun; vertical-align:baseline; color:rgb(78,85,132)">抽象渲染机器</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">要想成功分析应用程序的图形性能，必须先掌握一个最基本的知识，也就是对</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;OpenGL
 ES API&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">底下系统运作方式建立一个心智模型，让工程师能够推断他们观察到的行为。</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">为避免让开发人员陷于驱动程序软件和硬件子系统的实施细节的沼泽之中（这些他们无法控制，因而价&#20540;有限），有必要定义一个简化的抽象机器，用作解读所观察到的行为的基础。这一机器包含三个有用部分，它们大体上是独立不相干的，所以我将在本系列博文的开头几篇中逐一介绍。不过，为了让你对它们有个初步印象，下面列出该模型的三个部分：</span>

*   <span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Symbol; vertical-align:baseline; color:black"><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline">CPU-GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline">渲染管线</span></span>
*   <span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Symbol; vertical-align:baseline; color:black"><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline">基于区块的渲染</span></span>
*   <span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Symbol; vertical-align:baseline; color:black"><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline"></span></span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Symbol; vertical-align:baseline; color:black"><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline">着色器核心架构</span></span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">在本篇博文中，我们将探讨第一个部分，即</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;CPU-GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">渲染管线。</span>

&nbsp;

## 
<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:15pt; font-family:SimSun; vertical-align:baseline; color:rgb(78,85,132)">同步</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:15pt; font-family:Arial,sans-serif; vertical-align:baseline; color:rgb(78,85,132)">API</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:15pt; font-family:SimSun; vertical-align:baseline; color:rgb(78,85,132)">，异步执行</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">务必要了解的一个基本知识是，</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">OpenGL
 ES API&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">上应用程序函数调用和这些</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;API&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">调用所需渲染运算的执行之间的临时关系。从应用程序的角度而言，</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">OpenGL
 ES API&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">被指定为同步</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;API</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">。应用程序进行一系列的函数调用来设置其下一绘制任务所需的状态，然后调用</span>&nbsp;<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">glDraw</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:9pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">[1]</span>&nbsp;<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">函数（通常称为绘制调用）触发实际的绘制运算。由于</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;API&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">是同步的，执行绘制调用后的所有</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;API&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">行为都被指定为要像渲染运算已经发生一样进行，但在几乎所有硬件加速的</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;OpenGL
 ES&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">实现上，这只是一种由驱动程序堆栈维持的美妙假象。</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">与绘制调用相&#20284;，驱动程序维持的第二个假象是帧末缓冲翻转。大多数头一次编写</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;OpenGL
 ES&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">应用程序的开发人员会告诉你，调用</span>&nbsp;<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">eglSwapBuffers<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline">将交换其应用程序的前缓冲和后缓冲。虽然这在逻辑上是对的，但驱动程序再一次维持了同步性的假象；在几乎所有平台上，实际的缓冲交换可能会在很久之后才会发生。</span></span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:12pt; font-family:SimSun; vertical-align:baseline; color:rgb(78,85,132)"></span>&nbsp;

### 
<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:12pt; font-family:SimSun; vertical-align:baseline; color:rgb(78,85,132)">管线化</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">正如你所想到的，需要创造这一假象的原因在于性能。如果我们强制渲染运算真正同步发生，你就会面临这样的尴尬：</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">CPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">忙于创建下一绘制运算的状态时，</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">会闲置；</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">执行渲染时，</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">CPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">会闲置。对于以性能为重的加速器而言，所有这些闲置时间都是绝然不可接受的。</span>

<div style="margin:0px; padding:0px; border:0px; font-size:14px; font-family:'Gill Sans Alt One WGL W01 Lt'; vertical-align:baseline; color:rgb(86,91,91); line-height:25px; text-align:justify">
<span style="margin:0px; padding:0in; border:1pt windowtext; font-weight:inherit; font-style:inherit; font-family:Arial,sans-serif; vertical-align:baseline">[![gles-sync.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-3304-8413/gles-sync.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-3304-8413/gles-sync.png)[<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; color:rgb(4,129,165)">

</span>](https://community.arm.com/servlet/JiveServlet/downloadImage/38-2755-6948/gles-sync.png)</span></div>
<div style="margin:0px; padding:0px; border:0px; font-size:14px; font-family:'Gill Sans Alt One WGL W01 Lt'; vertical-align:baseline; color:rgb(86,91,91); line-height:25px; text-align:justify">
<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">为了去除这一闲置时间，我们使用</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;OpenGL
 ES&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">驱动程序来维持同步渲染行为的假象，而在面纱之后实际是以异步执行的方式处理渲染和帧交换。通过异步运行，我们可以建立一个小小的工作储备以允许创建一个管线，</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">从管线的一端处理较旧的工作负载，而</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;CPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">则负责将新的工作推入另一端。这一方式的优势在于，只要管线装满，就始终有工作在</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">上运行，提供最佳的性能。</span></div>

[![gles-async.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-3304-8420/gles-async.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-3304-8420/gles-async.png)

&nbsp;

<div style="margin:0px; padding:0px; border:0px; font-size:14px; font-family:'Gill Sans Alt One WGL W01 Lt'; vertical-align:baseline; color:rgb(86,91,91); line-height:25px; text-align:justify">
<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">Mali GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">管线中的工作单元是以渲染目标为单位进行计划的，其中渲染目标可能是屏幕缓存或离屏缓存。单个渲染目标通过两步处理。首先，</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">为渲染目标中的所有绘制调用处理顶点着色</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">[2]</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:SimSun; vertical-align:baseline">。</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">然后，为整个渲染目标处理片段着色</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">[3]</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:SimSun; vertical-align:baseline">。</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">因此，</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">Mali&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">的逻辑渲染管线包含三个阶段：</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">CPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">处理阶段、几何处理阶段，以及片段处理阶段。</span></div>

&nbsp;

[![gles-mali.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-3304-8421/gles-mali.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-3304-8421/gles-mali.png)

&nbsp;

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:12pt; font-family:SimSun; vertical-align:baseline; color:rgb(78,85,132)">管线节流</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">观察力敏锐的读者可能已注意到，上图中片段部分的工作是三个运算中最慢的，被</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;CPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">和几何处理阶段甩得越来越远。这种情形并不少见；大多数内容中要着色的片段远多于顶点，因此片段着色通常是占主导地位的处理运算。</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">在现实中，最好要尽可能缩短从</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;CPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">工作结束到帧被渲染之间的延时</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;–&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">对最终用户而言，最让人烦躁的莫过于在操作触控屏设备时，其触控事件输入和屏幕中数据显示之间出现数百毫秒的不同步</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;–&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">所以，我们不希望等待片段处理阶段的工作储备变得过大。简而言之，我们需要某种机制来定期减慢</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;CPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">线程，当管线足够满、能够维持良好性能时停止把工作放入队列。</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">这种节流机制通常由主机窗口系统提供，而不是图形驱动程序本身。例如，在</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;Android&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">上，我们只有在知道缓冲方向时才能处理任何绘制运算，因为用户可能会旋转其设备，造成帧大小出现变化。</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">SurfaceFlinger—
 Android&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">窗口表面管理器</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;–&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">可以通过一个简单方式控制管线深度：当管线中排队等待渲染的缓冲数量超过</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;N&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">个时，拒绝将缓冲返回到应用程序的图形堆栈。</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">如果出现这种情形，你就会看到：一旦每一帧达到</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">“N”</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">时</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;CPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">就会进入闲置状态，在内部阻止</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;EGL&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">或</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;OpenGL

ES API&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">函数，直到显示屏消耗完一个待处理缓存，为新的渲染运算空出一个位置。</span>

&nbsp;

&nbsp;&nbsp;&nbsp;[![gles-mali-throttle.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-3304-8422/gles-mali-throttle.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-3304-8422/gles-mali-throttle.png)

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">如果图形堆栈的运行快于显示刷新率，同样的方案也可限制管线缓冲；在这一情形下，内容受到</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">VSYNC</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">限制</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">”</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">并等待垂直空白（</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">VSYNC</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">同步）信号，该信号告诉显示控制器它可以切换到下一缓冲。如果</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">产生帧的速度快于显示屏显示帧的速度，那么</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">

SurfaceFlinger&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">将积累一定数量已经完成渲染但依然需要显示在屏幕上的缓冲；即使这些缓冲不再是</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;Mali&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">管线的一个部分，它们依然算在应用程序进程的</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;N&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">帧限制内。</span>

[![gles-mali-vsync.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-3304-8423/gles-mali-vsync.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-3304-8423/gles-mali-vsync.png)

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">正如上面的管线示意图所示，如果内容受到</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">VSYNC</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">同步限制，那么会经常出现</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;CPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">和</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;GPU&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">都完全闲置的时段。平台动态电压和频率调节</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;(DVFS)&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">通常会在此类情形中尝试降低当前的工作频率，以降低电压和功耗，但由于</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;DVFS&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">频率选择通常相对粗糙，所以可能会出现一定数量的闲置时间。</span>

&nbsp;

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:inherit; vertical-align:baseline">小结</span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">本篇博文中，我们探讨了</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;OpenGL
 ES API&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">提供的同步假象，以及</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;API&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">下实际运行异步渲染管线的原因。敬请<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:13.3333px; vertical-align:baseline">期待</span></span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:Arial,sans-serif; vertical-align:baseline; color:black">[<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">下一篇</span>](https://community.arm.com/groups/arm-mali-graphics/blog/2014/02/20/the-mali-gpu-an-abstract-machine-part-2)</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">博文，我将继续往下开发这一台抽象机器，探讨</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">&nbsp;Mali
 GPU<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline">基于区块的渲染做法。</span></span>

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:black">欢迎大家提出意见和问题。</span>

<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">Pete</span>

&nbsp;

&nbsp;

<div style="margin:0px; padding:0px; border:0px; font-size:14px; font-family:'Gill Sans Alt One WGL W01 Lt'; vertical-align:baseline; color:rgb(86,91,91); line-height:25px; text-align:justify">

<span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:12pt; font-family:SimSun; vertical-align:baseline; color:rgb(78,85,132)">脚注</span></div>

<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Symbol; vertical-align:baseline; color:black"><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline">[1]&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline">执行绘制的</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline">&nbsp;OpenGL
 ES&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline">绘制函数有许多，本例中选用哪一个其实没什么关系。</span></span>

<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Symbol; vertical-align:baseline"><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Arial,sans-serif; vertical-align:baseline; color:black">[2]&nbsp;</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:13.3333px; font-family:Arial,sans-serif; vertical-align:baseline">[<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; color:rgb(4,129,165)">Vertex
 (computer graphics) –&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:rgb(4,129,165)">（顶点（计算机图形））</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; color:rgb(4,129,165)">&nbsp;-&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:rgb(4,129,165)">维基百科，自由的百科全书</span>&nbsp;&nbsp;](https://community.arm.com/external-link.jspa?url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FVertex_%28computer_graphics%29)</span></span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Symbol; vertical-align:baseline"><span class="pasted-list-info" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:13.3333px; font-family:inherit; vertical-align:baseline"></span></span>

<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:Symbol; vertical-align:baseline"><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:13.3333px; font-family:Arial,sans-serif; vertical-align:baseline">[3]</span>&nbsp;<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:13.3333px; font-family:Arial,sans-serif; vertical-align:baseline">[<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; color:rgb(4,129,165)">Fragment
 (computer graphics) -&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:rgb(4,129,165)">（片段（计算机图形））</span><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; color:rgb(4,129,165)">&nbsp;-&nbsp;</span><span lang="ZH-CN" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:SimSun; vertical-align:baseline; color:rgb(4,129,165)">维基百科，自由的百科全书</span>&nbsp;&nbsp;](https://community.arm.com/external-link.jspa?url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FFragment_%28computer_graphics%29)</span></span>

            <div>
                作者：qwertyu1234 发表于2015/12/3 9:55:35 [原文链接](http://blog.csdn.net/qwertyu1234/article/details/50156667)
            </div>
            <div>
            阅读：7 评论：0 [查看评论](http://blog.csdn.net/qwertyu1234/article/details/50156667#comments)
            </div>