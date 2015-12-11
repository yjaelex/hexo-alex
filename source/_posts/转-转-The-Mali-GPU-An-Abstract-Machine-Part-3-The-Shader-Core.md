title: '[转][转]The Mali GPU: An Abstract Machine, Part 3 - The Shader Core'
tags: []
date: 2015-12-03 10:08:43
---

第三篇，介绍Shader Core。

## 1\. 英文原文

In the first two blogs of this series I introduced the frame-level pipelining [[The
 Mali GPU: An Abstract Machine, Part 1 - Frame Pipelining](https://community.arm.com/groups/arm-mali-graphics/blog/2014/02/03/the-mali-gpu-an-abstract-machine-part-1)] and tile based rendering architecture [[The
 Mali GPU: An Abstract Machine, Part 2 - Tile-based Rendering](https://community.arm.com/groups/arm-mali-graphics/blog/2014/02/20/the-mali-gpu-an-abstract-machine-part-2)] used by the Mali GPUs, aiming to develop a mental model which developers can use to explain the behavior of the graphics stack when optimizing the performance of their applications.

&nbsp;

In this blog I will finish the construction of this abstract machine, forming the final component: the Mali GPU itself.&nbsp; This blog assumes you have read the first two parts in the series, so I would recommend starting with those if you have not read them already.

&nbsp;

## 
GPU Architecture

&nbsp;

The &quot;Midgard&quot; family of Mali GPUs&nbsp; (the Mali-T600 and Mali-T700 series) use a unified shader core architecture, meaning that only a single type of shader core exists in the design. This single core can execute all types of programmable shader code, including
 vertex shaders, fragment shaders, and compute kernels.

&nbsp;

The exact number of shader cores present in a particular silicon chip varies; our silicon partners can choose how many shader cores they implement based on their performance needs and silicon area constraints. The Mali-T760 GPU can scale from a single core
 for low-end devices all the way up to 16 cores for the highest performance designs, but between 4 and 8 cores are the most common implementations.

&nbsp;

[![mali-top-level.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-2906-7387/mali-top-level.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-2906-7387/mali-top-level.png)

The graphics work for the GPU is queued in a pair of queues, one for vertex/tiling workloads and one for fragment workloads, with all work for one render target being submitted as a single submission into each queue. Workloads from both queues can be processed
 by the GPU at the same time, so vertex processing and fragment processing for different render targets can be running in parallel (see the first blog for more details on this pipelining methodology). The workload for a single render target is broken into smaller
 pieces and distributed across all of the shader cores in the GPU, or in the case of tiling workloads (see the second blog in this series for an overview of tiling) a fixed function tiling unit.

&nbsp;

The shader cores in the system share a level 2 cache to improve performance, and to reduce memory bandwidth caused by repeated data fetches. Like the number of cores, the size of the L2 is configurable by our silicon partners, but is typically in the range
 of 32-64KB per shader core in the GPU depending on how much silicon area is available. The number and bus width of the memory ports this cache has to external memory is configurable, again allowing our partners to tune the implementation to meet their performance,
 power, and area needs. In general we aim to be able to write one 32-bit pixel per core per clock, so it would be reasonable to expect an 8-core design to have a total of 256-bits of memory bandwidth (for both read and write) per clock cycle.

&nbsp;

## 
Mali GPU Shader Core

&nbsp;

The Mali shader core is structured as a number of fixed-function hardware blocks wrapped around a programmable &quot;tripipe&quot; execution core. The fixed function units perform the setup for a shader operation - such as rasterizing triangles or performing depth testing
 - or handling the post-shader activities - such as blending, or writing back a whole tile's worth of data at the end of rendering. The tripipe itself is the programmable part responsible for the execution of shader programs.

&nbsp;

[![mali-top-core.png](https://community.arm.com/servlet/JiveServlet/downloadImage/38-2906-7388/mali-top-core.png)](https://community.arm.com/servlet/JiveServlet/showImage/38-2906-7388/mali-top-core.png)

&nbsp;

### 
The Tripipe

&nbsp;

There are three classes of execution pipeline in the tripipe design: one handling arithmetic operations, one handling memory load/store and varying access, and one handling texture access. There is one load/store and one texture pipe per shader core, but the
 number of arithmetic pipelines can vary depending on which GPU you are using; most silicon shipping today will have two arithmetic pipelines, but GPU variants with up to four pipelines are also available.

&nbsp;

### 
Massively Multi-threaded Machine

&nbsp;

Unlike a traditional CPU architecture, where you will typically only have a single thread of execution at a time on a single core, the tripipe is a massively multi-threaded processing engine. There may well be hundreds of hardware threads running at the same
 time in the tripipe, with one thread created for each vertex or fragment which is shaded. This large number of threads exists to hide memory latency; it doesn't matter if some threads are stalled waiting for memory, as long as at least one thread is available
 to execute then we maintain efficient execution.

&nbsp;

### 
Arithmetic Pipeline: Vector Core

&nbsp;

The arithmetic pipeline (A-pipe) is a&nbsp;[SIMD](https://community.arm.com/external-link.jspa?url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FSIMD)&nbsp;(single
 instruction multiple data) vector processing engine, with arithmetic units which operate on 128-bit quad-word registers. The registers can be flexibly accessed as either 2 x FP64, 4 x FP32, 8 x FP16, 2 x int64, 4 x int32, 8 x int16, or 16 x int8\. It is therefore
 possible for a single arithmetic vector task to operate on 8 &quot;mediump&quot; values in a single operation, and for OpenCL kernels operating on 8-bit luminance data to process 16 pixels per SIMD unit per clock cycle.

&nbsp;

While I can't disclose the internal architecture of the arithmetic pipeline, our public performance data for each GPU can be used to give some idea of the number of maths units available. For example, the Mali-T760 with 16 cores is rated at 326 FP32 GFLOPS
 at 600MHz. This gives a total of 34 FP32 FLOPS per clock cycle for this shader core; it has two pipelines, so that's 17 FP32 FLOPS per pipeline per clock cycle. The available performance in terms of operations will increase for FP16/int16/int8 and decrease
 for FP64/int64 data types.

&nbsp;

### 
Texture Pipeline

&nbsp;

The texture pipeline (T-pipe) is responsible for all memory access to do with textures. The texture pipeline can return one bilinear filtered texel per clock; trilinear filtering requires us to load samples from two different mipmaps in memory, so requires
 a second clock cycle to complete.

&nbsp;

### 
Load/Store Pipeline

&nbsp;

The load/store pipeline (LS-pipe) is responsible for all memory accesses which are not related to texturing.&nbsp; For graphics workloads this means reading attributes and writing varyings during vertex shading, and reading varyings during fragment shading. In general
 every instruction is a single memory access operation, although like the arithmetic pipeline they are vector operations and so could load an entire &quot;highp&quot; vec4 varying in a single instruction.

&nbsp;

### 
Early ZS Testing and Late ZS Testing

&nbsp;

In the OpenGL ES specification &quot;fragment operations&quot; - which include depth and stencil testing - happen at the end of the pipeline, after fragment shading has completed. This makes the specification very simple, but implies that you have to spend lots of time
 shading something, only to throw it away at the end of the frame if it turns out to be killed by ZS testing. Coloring fragments just to discard them would cost a huge amount of performance and wasted energy, so where possible we will do ZS testing early (i.e.
 before fragment shading), only falling back to late ZS testing (i.e. after fragment shading) where it is unavoidable (e.g. a dependency on fragment which may call &quot;discard&quot; and as such has indeterminate depth state until it exits the tripipe).

&nbsp;

In addition to the traditional early-z schemes, we also have some overdraw removal capability which can stop fragments which have already been rasterized from turning into real rendering work if they do not contribute to the output scene in a useful way. My
 colleague&nbsp;[seanellis](https://community.arm.com/people/seanellis)&nbsp;has
 a great blog looking at this technology -<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; line-height:1.5em">&nbsp;</span>[Killing
 Pixels - A New Optimization for Shading on ARM Mali GPUs&nbsp;](https://community.arm.com/groups/arm-mali-graphics/blog/2013/08/08/killing-pixels--a-new-optimization-for-shading-on-arm-mali-gpus)<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; line-height:1.5em">- so I won't dive into
 any more detail here.</span>

&nbsp;

## 
Memory System

&nbsp;

This section is an after-the-fact addition to this blog, so if you have read this blog before and don't remember this section, don't worry you're not going crazy. We have been getting a lot of questions from developers writing OpenCL kernels and OpenGL ES compute
 shaders asking for more information about the GPU cache structure, as it can be really beneficial to lay out data structures and buffers to optimize cache locality. The salient facts are:

&nbsp;

*   Two 16KB L1 data caches per shader core; one for texture access and one for generic memory access.
*   A single logical L2 which is shared by all of the shader cores. The size of this is variable and can be configured by the silicon integrator, but is typically between 32 and 64 KB per instantiated shader core.
*   Both cache levels<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; line-height:1.5em">&nbsp;use 64 byte cache lines.</span>

&nbsp;

If you are new to optimization of massively multi-threaded algorithms on massively multi-threaded architectures I would heartily recommend the SGEMM matrix multiplication video on our Mali Developer portal here:

&nbsp;

*   [http://malideveloper.arm.com/develop-for-mali/opencl-renderscript-tutorials/#example](https://community.arm.com/external-link.jspa?url=http%3A%2F%2Fmalideveloper.arm.com%2Fdevelop-for-mali%2Fopencl-renderscript-tutorials%2F%23example)

&nbsp;

... as the overall system behavior can be very different to what you are used to if you are coming from a traditional CPU background.

&nbsp;

## 
GPU Limits

&nbsp;

Based on this simple model it is possible to outline some of the fundamental properties underpinning the GPU performance.

&nbsp;

*   The GPU can issue one vertex per shader core per clock
*   The GPU can issue one fragment per shader core per clock
*   The GPU can retire one pixel per shader core per clock
*   We can issue one instruction per pipe per clock, so for a typical shader core we can issue four instructions in parallel if we have them available to run

    *   We can achieve 17 FP32 operations per A-pipe
    *   One vector load, one vector store, or one vector varying per LS-pipe
    *   One bilinear filtered texel per T-pipe

*   The GPU will typically have 32-bits of DDR access (read and write) per core per clock [configurable]

&nbsp;

<span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; line-height:1.5em">If we scale this to a Mali-T760 MP8 running at 600MHz we can calculate the theoretical
 peak performance as:</span>

&nbsp;

*   Fillrate:

    *   8 pixels per clock = 4.8 GPix/s
    *   <span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-family:inherit; vertical-align:baseline">That's 2314 complete 1080p frames per second!</span>

*   Texture rate:

    *   8 bilinear texels per clock = 4.8 GTex/s
    *   <span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-family:inherit; vertical-align:baseline">That's 38 bilinear filtered texture lookups per pixel for 1080p @ 60 FPS!</span>

*   Arithmetic rate:

    *   17 FP32 FLOPS per pipe per core = 163 FP32 GFLOPS
    *   <span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-family:inherit; vertical-align:baseline">That's 1311 FLOPS per pixel for 1080p @ 60 FPS!</span>

*   Bandwidth:

    *   256-bits of memory access per clock = 19.2GB/s read and write bandwidth<sup>1</sup><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; line-height:1.5em">.</span>
    *   <span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-size:10pt; font-family:inherit; vertical-align:baseline; line-height:1.5em"><span style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-size:13.3333px; font-family:inherit; vertical-align:baseline">That's
 154 bytes per pixel for 1080p @ 60 FPS!</span></span>

&nbsp;

## 
OpenCL and Compute

&nbsp;

The observant reader will have noted that I've talked a lot about vertices and fragments - the staple of graphics work - but have mentioned very little about how OpenCL and RenderScript compute threads come into being inside the core. Both of these types of
 work behave almost identically to vertex threads - you can view running a vertex shader over an array of vertices as a 1-dimensional compute problem. So the vertex thread creator also spawns compute threads, although more accurately I would say the compute
 thread creator also spawns vertices&nbsp;<span class="emoticon-inline emoticon_wink" style="margin:0px; padding:0px; border:0px; font-weight:inherit; font-style:inherit; font-family:inherit; vertical-align:baseline; display:inline-block; height:16px; width:16px"></span>.

&nbsp;

## 
Next Time ...

&nbsp;

This blog concludes the first chapter of this series, developing the abstract machine which defines the basic behaviors which an application developer should expect to see for a Mali GPU in the Midgard family. Over the rest of this series I'll start to put
 this new knowledge to work, investigating some common application development pitfalls, and useful optimization techniques, which can be identified and debugged using the Mali integration into the ARM DS-5 Streamline profiling tools.

&nbsp;

<span style="margin:0px; padding:0px; border:0px; font-style:inherit; font-family:inherit; vertical-align:baseline">EDIT: Next blog now available:</span>

*   [Mali
 Performance 1: Checking the Pipeline](https://community.arm.com/groups/arm-mali-graphics/blog/2014/04/02/mali-graphics-performance-1-checking-the-pipeline)

&nbsp;

Comments and questions welcomed as always,

TTFN,

Pete

&nbsp;

### 
Footnotes

&nbsp;

1.  ... 19.2GB/s subject to the ability of the rest of the memory system outside of the GPU to give us data this quickly. Like most features of an ARM-based chip, the down-stream memory system is highly configurable in order to allow different vendors to tune power,
 performance, and silicon area according to their needs. For most SoC parts the rest of the system will throttle the available bandwidth before the GPU runs out of an ability to request data. It is unlikely you would want to sustain this kind of bandwidth for
 prolonged periods, but short burst performance is important.

            <div>
                作者：qwertyu1234 发表于2015/12/3 10:08:43 [原文链接](http://blog.csdn.net/qwertyu1234/article/details/50156923)
            </div>
            <div>
            阅读：4 评论：0 [查看评论](http://blog.csdn.net/qwertyu1234/article/details/50156923#comments)
            </div>