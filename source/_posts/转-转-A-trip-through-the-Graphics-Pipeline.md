title: '[转][转] A trip through the Graphics Pipeline'
tags: []
date: 2015-09-29 16:56:02
---

转一个国外牛人的文章；深入浅出的介绍了现代GPU的方方面面，从软件到硬件都有。理解了这些对理解新一代图形API大有好处！![微笑](http://static.blog.csdn.net/xheditor/xheditor_emot/default/smile.gif)

原文&nbsp;[A trip through the Graphics Pipeline 2011: Index](https://fgiesen.wordpress.com/2011/07/09/a-trip-through-the-graphics-pipeline-2011-index/)

PDF下载：[http://download.csdn.net/detail/qwertyu1234/9041011](http://download.csdn.net/detail/qwertyu1234/9041011)

**A trip through the Graphics Pipeline 2011: Index**

&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; July 9, 2011&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; Welcome.

This is the index page for a series of blog posts I’m currently writing aboutthe D3D/OpenGL graphics pipelines&nbsp;_as actually implemented by GPUs_.A lot of this is well known among graphics programmers, and there’s tons ofpapers on various bits and pieces
 of it, but one bit I’ve been annoyed with isthat while there’s both broad overviews and very detailed information onindividual components, there’s not much in between, and what little there is ismostly out of date.

This series is intended for graphics programmers that know a modern 3D API (atleast OpenGL 2.0&#43; or D3D9&#43;) well and want to know how it all looks under thehood. It’s&nbsp;_not_&nbsp;a description of the graphics pipeline fornovices; if you haven’t used a 3D API,
 most if not all of this will becompletely useless to you. I’m also assuming a working understanding ofcontemporary hardware design – you should at the very least know whatregisters, FIFOs, caches and pipelines are, and understand how they work.Finally, you
 need a working understanding of at least basic parallelprogramming mechanisms. A GPU is a massively parallel computer, there’s no wayaround it.

Some readers have commented that this is a really low-level description of thegraphics pipeline and GPUs; well, it all depends on where you’re standing. GPUarchitects would call this a&nbsp;_high-level_&nbsp;description of a GPU.Not quite as high-level as the
 multicolored flowcharts you tend to see onhardware review sites whenever a new GPU generation arrives; but, to be honest,that kind of reporting tends to have a very low information density, even whenit’s done well. Ultimately, it’s not meant to explain how
 anythingactually&nbsp;_works_&nbsp;– it’s just technology porn that’s trying toshow off shiny new gizmos. Well, I try to be a bit more substantial here, whichunfortunately means less colors and less benchmark results, but instead lotsand lots of text, a few mono-colored
 diagrams and even some (_shudder_)equations. If that’s okay with you, then here’s the index:

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 1</span>](http://fgiesen.wordpress.com/2011/07/01/a-trip-through-the-graphics-pipeline-2011-part-1/): Introduction; the Software stack.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 2</span>](http://fgiesen.wordpress.com/2011/07/02/a-trip-through-the-graphics-pipeline-2011-part-2/): GPU memory architecture and the Command Processor.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 3</span>](http://fgiesen.wordpress.com/2011/07/03/a-trip-through-the-graphics-pipeline-2011-part-3/): 3D pipeline overview, vertex processing.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 4</span>](http://fgiesen.wordpress.com/2011/07/04/a-trip-through-the-graphics-pipeline-2011-part-4/): Texture samplers.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 5</span>](http://fgiesen.wordpress.com/2011/07/05/a-trip-through-the-graphics-pipeline-2011-part-5/): Primitive Assembly, Clip/Cull, Projection, and Viewporttransform.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 6</span>](http://fgiesen.wordpress.com/2011/07/06/a-trip-through-the-graphics-pipeline-2011-part-6/): (Triangle) rasterization and setup.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 7</span>](http://fgiesen.wordpress.com/2011/07/08/a-trip-through-the-graphics-pipeline-2011-part-7/): Z/Stencil processing, 3 different ways.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 8</span>](http://fgiesen.wordpress.com/2011/07/10/a-trip-through-the-graphics-pipeline-2011-part-8/): Pixel processing – “fork phase”.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 9</span>](http://fgiesen.wordpress.com/2011/07/12/a-trip-through-the-graphics-pipeline-2011-part-9/): Pixel processing – “join phase”.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 10</span>](http://fgiesen.wordpress.com/2011/07/20/a-trip-through-the-graphics-pipeline-2011-part-10/): Geometry Shaders.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 11</span>](http://fgiesen.wordpress.com/2011/08/14/a-trip-through-the-graphics-pipeline-2011-part-11/): Stream-Out.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 12</span>](http://fgiesen.wordpress.com/2011/09/06/a-trip-through-the-graphics-pipeline-2011-part-12/): Tessellation.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[<span style="color:#336699">Part 13</span>](http://fgiesen.wordpress.com/2011/10/09/a-trip-through-the-graphics-pipeline-2011-part-13/): Compute Shaders.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

<table border="0" cellspacing="0" cellpadding="0" width="927">
</table>

**A trip through the GraphicsPipeline 2011, part 1**

&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp; July 1, 2011

It’s been awhile since I posted something here, and I figured I might use thisspot to explain some general points about graphics hardware and software as of2011; you can find functional descriptions of what the graphics stack in yourPC does, but usually not
 the “how” or “why”; I’ll try to fill in the blankswithout getting too specific about any particular piece of hardware. I’m goingto be mostly talking about DX11-class hardware running D3D9/10/11 on Windows,because that happens to be the (PC) stack I’m most
 familiar with – not that theAPI details etc. will matter much past this first part; once we’re actually onthe GPU it’s all native commands.

**

The application**

This is your code. These are also your bugs. Really. Yes, the API runtime andthe driver have bugs, but this is not one of them. Now go fix it already.

**

The API runtime**

You make your resource creation / state setting / draw calls to the API. TheAPI runtime keeps track of the current state your app has set, validatesparameters and does other error and consistency checking, manages user-visibleresources, may or may not validate
 shader code and shader linkage (or at leastD3D does, in OpenGL this is handled at the driver level) maybe batches worksome more, and then hands it all over to the graphics driver – more precisely,the user-mode driver.

**

The user-mode graphics driver (or UMD)

**This is where most of the “magic” on the CPU side happens. If your appcrashes because of some API call you did, it will usually be in here :). It’scalled “nvd3dum.dll” (NVidia) or “atiumd*.dll” (AMD). As the name suggests,this is user-mode code; it’s
 running in the same context and address space asyour app (and the API runtime) and has no elevated privileges whatsoever. It implementsa lower-level API (the DDI) that is called by D3D; this API is fairly similarto the one you’re seeing on the surface, but
 a bit more explicit about thingslike memory management and such.

This module is where things like shader compilation happen. D3D passes apre-validated shader token stream to the UMD – i.e. it’s already checked thatthe code is valid in the sense of being syntactically correct and obeying D3Dconstraints (using the right types,
 not using more textures/samplers thanavailable, not exceeding the number of available constant buffers, stuff likethat). This is compiled from HLSL code and usually has quite a number ofhigh-level optimizations (various loop optimizations, dead-code elimination,constant
 propagation, predicating ifs etc.) applied to it – this is good newssince it means the driver benefits from all these relatively costlyoptimizations that have been performed at compile time. However, it also has abunch of lower-level optimizations (such as
 register allocation and loopunrolling) applied that drivers would rather do themselves; long story short,this usually just gets immediately turned into a intermediate representation(IR) and then compiled some more; shader hardware is close enough to D3Dbytecode
 that compilation doesn’t need to work wonders to give good results(and the HLSL compiler having done some of the high-yield and high-costoptimizations already definitely helps), but there’s still lots of low-leveldetails (such as HW resource limits and scheduling
 constraints) that D3Dneither knows nor cares about, so this is not a trivial process.

And of course, if your app is a well-known game, programmers at NV/AMD haveprobably looked at your shaders and wrote hand-optimized replacements for theirhardware – though they better produce the same results lest there be a scandal:). These shaders get detected
 and substituted by the UMD too. You’re welcome.

More fun: Some of the API state may actually end up being compiled into theshader – to give an example, relatively exotic (or at least infrequently used)features such as texture borders are probably not implemented in the texturesampler, but emulated with extra
 code in the shader (or just not supported atall). This means that there’s sometimes multiple versions of the same shaderfloating around, for different combinations of API states.

Incidentally, this is also the reason why you’ll often see a delay the firsttime you use a new shader or resource; a lot of the creation/compilation workis deferred by the driver and only executed when it’s actually necessary (youwouldn’t believe how much unused
 crap some apps create!). Graphics programmersknow the other side of the story – if you want to make sure something isactually created (as opposed to just having memory reserved), you need to issuea dummy draw call that uses it to “warm it up”. Ugly and annoying,
 but this hasbeen the case since I first started using 3D hardware in 1999 – meaning, it’spretty much a fact of life by this point, so get used to it. :)

Anyway, moving on. The UMD also gets to deal with fun stuff like all the D3D9“legacy” shader versions and the fixed function pipeline – yes, all of thatwill get faithfully passed through by D3D. The 3.0 shader profile ain’t thatbad (it’s quite reasonable in
 fact), but 2.0 is crufty and the various 1.xshader versions are seriously whack – remember 1.3 pixel shaders? Or, for thatmatter, the fixed-function vertex pipeline with vertex lighting and such? Yeah,support for all that’s still there in D3D and the guts
 of every modern graphicsdriver, though of course they just translate it to newer shader versions by now(and have been doing so for quite some time).

Then there’s things like memory management. The UMD will get things liketexture creation commands and need to provide space for them. Actually, the UMDjust suballocates some larger memory blocks it gets from the KMD (kernel-modedriver); actually mapping and
 unmapping pages (and managing which part of videomemory the UMD can see, and conversely which parts of system memory the GPU mayaccess) is a kernel-mode privilege and can’t be done by the UMD.

But the UMD can do things like&nbsp;[<span style="color:#336699">swizzling textures</span>](http://fgiesen.wordpress.com/2011/01/17/texture-tiling-and-swizzling/)&nbsp;(unlessthe GPU can do this in hardware, usually using 2D
 blitting units not the real3D pipeline) and schedule transfers between system memory and (mapped) videomemory and the like. Most importantly, it can also write command buffers (or“DMA buffers” – I’ll be using these two names interchangeably) once the KMD hasallocated
 them and handed them over. A command buffer contains, well, commands:). All your state-changing and drawing operations will be converted by the UMDinto commands that the hardware understands. As will a lot of things you don’ttrigger manually – such as uploading
 textures and shaders to video memory.

In general, drivers will try to put as much of the actual processing into theUMD as possible; the UMD is user-mode code, so anything that runs in it doesn’tneed any costly kernel-mode transitions, it can freely allocate memory, farmwork out to multiple threads,
 and so on – it’s just a regular DLL (even thoughit’s loaded by the API, not directly by your app). This has advantages for driverdevelopment too – if the UMD crashes, the app crashes with it, but not thewhole system; it can just be replaced while the system
 is running (it’s just aDLL!); it can be debugged with a regular debugger; and so on. So it’s not onlyefficient, it’s also convenient.

But there’s a big elephant in the room that I haven’t mentioned yet.

**Did I say “user-mode driver”? I meant “user-mode drivers”.**

As said, the UMD is just a DLL. Okay, one that happens to have the blessing ofD3D and a direct pipe to the KMD, but it’s still a regular DLL, and in runs inthe address space of its calling process.

But we’re using multi-tasking OSes nowadays. In fact, we have been for sometime.

This “GPU” thing I keep talking about? That’s a shared resource. There’s onlyone that drives your main display (even if you use SLI/Crossfire). Yet we havemultiple apps that try to access it (and pretend they’re the only ones doingit). This doesn’t just work
 automatically; back in The Olden Days, the solutionwas to only give 3D to one app at a time, and while that app was active, allothers wouldn’t have access. But that doesn’t really cut it if you’re trying tohave your windowing system use the GPU for rendering.
 Which is why you needsome component that arbitrates access to the GPU and allocates time-slices andsuch.

**Enter the scheduler.

**This is a system component – note the “the” is somewhat misleading; I’mtalking about the graphics scheduler here, not the CPU or IO schedulers. Thisdoes exactly what you think it does – it arbitrates access to the 3D pipelineby time-slicing it between
 different apps that want to use it. A context switchincurs, at the very least, some state switching on the GPU (which generatesextra commands for the command buffer) and possibly also swapping some resourcesin and out of video memory. And of course only one
 process gets to actuallysubmit commands to the 3D pipe at any given time.

You’ll often find console programmers complaining about the fairly high-level,hands-off nature of PC 3D APIs, and the performance cost this incurs. But thething is that 3D APIs/drivers on PC really have a more complex problem to solvethan console games – they
 really do need to keep track of the full currentstate for example, since someone may pull the metaphorical rug from under themat any moment! They also work around broken apps and try to fix performanceproblems behind their backs; this is a rather annoying
 practice that no-one’shappy with, certainly including the driver authors themselves, but the fact isthat the business perspective wins here; people expect stuff that runs tocontinue running (and doing so smoothly). You just won’t win any friends byyelling
 “BUT IT’S WRONG!” at the app and then sulking and going through anultra-slow path.

Anyway, on with the pipeline. Next stop: Kernel mode!

**The kernel-mode driver (KMD)**

This is the part that actually deals with the hardware. There may be multipleUMD instances running at any one time, but there’s only ever one KMD, and ifthat crashes, then boom you’re dead – used to be “blue screen” dead, but by nowWindows actually knows how
 to kill a crashed driver and reload it (progress!).As long as it happens to be just a crash and not some kernel memory corruptionat least – if that happens, all bets are off.

The KMD deals with all the things that are just there once. There’s only oneGPU memory, even though there’s multiple apps fighting over it. Someone needsto call the shots and actually allocate (and map) physical memory. Similarly,someone must initialize the
 GPU at startup, set display modes (and get modeinformation from displays), manage the hardware mouse cursor (yes, there’s HWhandling for this, and yes, you really only get one! :), program the HWwatchdog timer so the GPU gets reset if it stays unresponsive
 for a certaintime, respond to interrupts, and so on. This is what the KMD does.

There’s also this whole content protection/DRM bit about setting up aprotected/DRM’ed path between a video player and the GPU so no the actualprecious decoded video pixels aren’t visible to any dirty user-mode code thatmight do awful forbidden things like dump
 them to disk (…whatever). The KMD hassome involvement in that too.

Most importantly for us, the KMD manages the&nbsp;_actual_&nbsp;commandbuffer. You know, the one that the hardware actually consumes. The commandbuffers that the UMD produces aren’t the real deal – as a matter of fact,they’re just random slices of GPU-addressable
 memory. What actually happenswith them is that the UMD finishes them, submits them to the scheduler, whichthen waits until that process is up and then passes the UMD command buffer onto the KMD. The KMD then writes a call to command buffer into the main commandbuffer,
 and depending on whether the GPU command processor can read from mainmemory or not, it may also need to DMA it to video memory first. The maincommand buffer is usually a (quite small)&nbsp;[<span style="color:#336699">ring
 buffer</span>](http://fgiesen.wordpress.com/2010/12/14/ring-buffers-and-queues/)&nbsp;– the only thing that ever gets written there is system/initializationcommands and calls to the “real”, meaty 3D command buffers.

But this is still just a buffer in memory right now. Its position is known tothe graphics card – there’s usually a read pointer, which is where the GPU isin the main command buffer, and a write pointer, which is how far the KMD haswritten the buffer yet (or
 more precisely, how far it has&nbsp;_told_&nbsp;theGPU it has written yet). These are hardware registers, and they arememory-mapped – the KMD updates them periodically (usually whenever it submitsa new chunk of work)…

**The bus**…

but of course that write doesn’t go directly to the graphics card (at leastunless it’s integrated on the CPU die!), since it needs to go through the busfirst – usually PCI Express these days. DMA transfers etc. take the same route.This doesn’t take very long,
 but it’s yet another stage in our journey. Untilfinally…

**The command processor!**

This is the frontend of the GPU – the part that actually reads the commands theKMD writes. I’ll continue from here in the next installment, since this post islong enough already :)

**Small aside: OpenGL**

OpenGL is fairly similar to what I just described, except there’s not as sharpa distinction between the API and UMD layer. And unlike D3D, the (GLSL) shadercompilation is not handled by the API at all, it’s all done by the driver. Anunfortunate side effect
 is that there are as many GLSL frontends as there are3D hardware vendors, all of them basically implementing the same spec, but withtheir own bugs and idiosyncrasies. Not fun. And it also means that the drivershave to do all the optimizations themselves whenever
 they get to see theshaders – including expensive optimizations. The D3D bytecode format is reallya cleaner solution for this problem – there’s only one compiler (so no slightlyincompatible dialects between different vendors!) and it allows for somecostlier
 data-flow analysis than you would normally do.

**Omissions and simplifcations**

This is just an overview; there’s tons of subtleties that I glossed over. Forexample, there’s not just one scheduler, there’s multiple implementations (thedriver can choose); there’s the whole issue of how synchronization between CPUand GPU is handled that
 I didn’t explain at all so far. And so on. And I mighthave forgotten something important – if so, please tell me and I’ll fix it! Butnow, bye and hopefully see you next time

&nbsp;

&nbsp;

**A trip through the GraphicsPipeline 2011, part 2**

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp; July 2, 2011

**

Not so fast.

**In the previous part I explained the various stages that your 3D renderingcommands go through on a PC before they actually get handed off to the GPU;short version: it’s more than you think. I then finished by name-dropping thecommand processor and
 how it actually finally does something with the commandbuffer we meticulously prepared. Well, how can I say this – I lied to you.We’ll indeed be meeting the command processor for the first time in thisinstallment, but remember, all this command buffer stuff
 goes through memory –either system memory accessed via PCI Express, or local video memory. We’regoing through the pipeline in order, so before we get to the command processor,let’s talk memory for a second.

**The memory subsystem**

GPUs don’t have your regular memory subsystem – it’s different from what yousee in general-purpose CPUs or other hardware, because it’s designed for verydifferent usage patterns. There’s two fundamental ways in which a GPU’s memorysubsystem differs from what
 you see in a regular machine:

The first is that GPU memory subsystems are&nbsp;_fast_. Seriously fast. ACore i7 2600K will hit maybe 19 GB/s memory bandwidth – on a good day. Withtail wind. Downhill. A GeForce GTX 480, on the other hand, has a total memorybandwidth of close to 180 GB/s
 – nearly an order of magnitude difference! Whoa.

The second is that GPU memory subsystems are&nbsp;_slow_. Seriously slow.A cache miss to main memory on a Nehalem (first-generation Core i7) takes about140 cycles if you divide the&nbsp;[<span style="color:#336699">memory
 latencyas given by AnandTech</span>](http://www.anandtech.com/show/2542/5)&nbsp;by the clock rate. The GeForce GTX 480 I mentionedpreviously has a&nbsp;[<span style="color:#336699">memory access
 latency of 400-800 clocks</span>](http://www.stanford.edu/dept/ICME/docs/seminars/Rennich-2011-04-25.pdf). So let’s just say that,measured in cycles, the GeForce GTX 480 has a bit more than 4x the averagememory latency of a Core i7\. Except that Core i7 I just mentioned is clocked at2.93GHz, whereas GTX 480 shader clock is 1.4
 GHz – that’s it, another 2x rightthere. Woops – again, nearly an order of magnitude difference! Wait, somethingfunny is going on here. My common sense is tingling. This must be one of thosetrade-offs I keep hearing about in the news!

Yep – GPUs get a massive increase in bandwidth, but they pay for it with amassive increase in latency (and, it turns out, a sizable hit in power drawtoo, but that’s beyond the scope of this article). This is part of a generalpattern – GPUs are all about throughput
 over latency; don’t wait for resultsthat aren’t there yet, do something else instead!

That’s almost all you need to know about GPU memory, except for one generalDRAM tidbit that will be important later on: DRAM chips are organized as a 2Dgrid – both logically and physically. There’s (horizontal) row lines and(vertical) column lines. At each
 intersection between such lines is atransistor and a capacitor; if at this point you want to know how to actuallybuild memory from these ingredients,&nbsp;[<span style="color:#336699">Wikipedia
 isyour friend</span>](http://en.wikipedia.org/wiki/DRAM#Operation_principle). Anyway, the salient point here is that the address of alocation in DRAM is split into a row address and a column address, and DRAMreads/writes internally always end up accessing all columns in the given row atthe same time. What this
 means is that it’s much cheaper to access a swath ofmemory that maps to exactly one DRAM row than it is to access the same amountof memory spread across multiple rows. Right now this may seem like just arandom bit of DRAM trivia, but this will become important
 later on; in otherwords, pay attention: this will be on the exam. But to tie this up with thefigures in the previous paragraphs, just let me note that you can’t reach thosepeak memory bandwidth figures above by just reading a few bytes all overmemory; if you
 want to saturate memory bandwidth, you better do it one full DRAMrow at a time.

**The PCIe host interface**

From a graphics programmer standpoint, this piece of hardware isn’tsuper-interesting. Actually, the same probably goes for a GPU hardwarearchitect too. The thing is, you still start caring about it once it’s so slowthat it’s a bottleneck. So what you do is
 get good people on it to do itproperly, to make sure that doesn’t happen. Other than that, well, this givesthe CPU read/write access to video memory and a bunch of GPU registers, the GPUread/write access to (a portion of) main memory, and everyone a headachebecause
 the latency for all these transactions is even worse than memorylatency because the signals have to go out of the chip, into the slot, travel abit across the mainboard then get to someplace in the CPU about a week later(or that’s how it feels compared to the
 CPU/GPU speeds anyway). The bandwidthis decent though – up to about 8GB/s (theoretical) peak aggregate bandwidthacross the 16-lane PCIe 2.0 connections that most GPUs use right now, so betweenhalf and a third of the aggregate CPU memory bandwidth; that’s a
 usable ratio.And unlike earlier standards like AGP, this is a symmetrical point-to-pointlink – that bandwidth goes both directions; AGP had a fast channel from the CPUto the GPU, but not the other way round.

**Some final memory bits and pieces**

Honestly, we’re&nbsp;_very very_&nbsp;close to actually seeing 3D commandsnow! So close you can almost taste them. But there’s one more thing we need toget out of the way first. Because now we have two kinds of memory – (local)video memory and mapped system memory.
 One is about a day’s worth of travel tothe north, the other is a week’s journey to the south along the PCI Expresshighway. Which road do we pick?

The easiest solution: Just add an extra address line that tells you which wayto go. This is simple, works just fine and has been done plenty of times. Ormaybe you’re on a unified memory architecture, like some game consoles (but notPCs). In that case, there’s
 no choice; there’s just&nbsp;_the memory_, whichis where you go, period. If you want something fancier, you add a MMU (memorymanagement unit), which gives you a fully virtualized address space and allowsyou to pull nice tricks like having frequently accessed
 parts of a texture invideo memory (where they’re fast), some other parts in system memory, and mostof it not mapped at all – to be conjured up from thing air, or, more usually,by a magic disk read that will only take about 50 years or so – and by the way,this
 is not hyperbole; if you stay with the “memory access = 1 day” metaphor,that’s really how long a single HD read takes. A quite fast one at that. Diskssuck. But I digress.

So, MMU. It also allows you to defragment your video memory address spacewithout having to actually copy stuff around when you start running out ofvideo memory. Nice thing, that. And it makes it much easier to have multipleprocesses share the same GPU. It’s
 definitely allowed to have one, but I’m notactually sure if it’s a requirement or not, even though it’s certainly reallynice to have (anyone care to help me out here? I’ll update the article if I getclarification on this, but tbh right now I just can’t be
 arsed to look it up).Anyway, a MMU/virtual memory is not really something you can just add on theside (not in an architecture with caches&nbsp;&nbsp;and memory consistencyconcerns anyway), but it really isn’t specific to any particular stage – I haveto mention it somewhere,
 so I just put it here.

There’s also a DMA engine that can copy memory around without having to involveany of our precious 3D hardware/shader cores. Usually, this can at least copybetween system memory and video memory (in both directions). It often can alsocopy from video memory
 to video memory (and if you have to do any VRAMdefragmenting, this is a useful thing to have). It usually can’t do systemmemory to system memory copies, because this is a GPU, not a memory copyingunit – do your system memory copies on the CPU where they don’t
 have to passthrough PCIe in both directions!

&nbsp;![]()

**Update**: I’ve drawn a&nbsp;[<span style="color:#336699">picture</span>](http://www.farbrausch.de/~fg/gpu/gpu_memory.jpg)&nbsp;(link since this layoutis too narrow to put big diagrams in the text). This also shows
 some moredetails – by now your GPU has multiple memory controllers, each of whichcontrols multiple memory banks, with a fat hub in the front. Whatever it takesto get that bandwidth. :)

Okay, checklist. We have a command buffer prepared on the CPU. We have the PCIehost interface, so the CPU can actually tell us about this, and write itsaddress to some register. We have the logic to turn that address into a loadthat will actually return data
 – if it’s from system memory it goes throughPCIe, if we decide we’d rather have the command buffer in video memory, the KMDcan set up a DMA transfer so neither the CPU nor the shader cores on the GPUneed to actively worry about it. And then we can get the
 data from our copy invideo memory through the memory subsystem. All paths accounted for, we’re setand finally ready to look at some commands!

**At long last, the command processor!

**Our discussion of the command processor starts, as so many things do thesedays, with a single word:

“Buffering…”

As mentioned above, both of our memory paths leading up to here arehigh-bandwidth but also high-latency. For most later bits in the GPU pipeline,the method of choice to work around this is to run lots of independent threads.But in this case, we only have a
 single command processor that needs to chewthrough our command buffer in order (since this command buffer contains thingssuch as state changes and rendering commands that need to be executed in theright sequence). So we do the next best thing: Add a large
 enough buffer andprefetch far enough ahead to avoid hiccups.&nbsp;

From that buffer, it goes to the actual command processing front end, which isbasically a state machine that knows how to parse commands (with ahardware-specific format). Some commands deal with 2D rendering operations –unless there’s a separate command processor
 for 2D stuff and the 3D frontendnever even sees it. Either way, there’s still dedicated 2D hardware hidden onmodern GPUs, just as there’s a VGA chip somewhere on that die that stillsupports text mode, 4-bit/pixel bit-plane modes, smooth scrolling and all thatstuff.
 Good luck finding any of that on the die without a microscope. Anyway,that stuff exists, but henceforth I shall not mention it again. :) Then there’scommands that actually hand some primitives to the 3D/shader pipe, woo-hoo!I’ll take about them in upcoming
 parts. There’s also commands that go to the3D/shader pipe but never render anything, for various reasons (and in variouspipeline configurations); these are up even later.

Then there’s commands that change state. As a programmer, you think of them asjust changing a variable, and that’s basically what happens. But a GPU is amassively parallel computer, and you can’t just change a global variable in aparallel system and hope that
 everything works out OK – if you can’t guaranteethat everything will work by virtue of some invariant you’re enforcing, there’sa bug and you will hit it eventually. There’s several popular methods, andbasically all chips use different methods for different
 types of state.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Whenever you change a state, you require that all pendingwork that might refer to that state be finished (i.e. basically a partialpipeline flush). Historically, this is how graphics chips handled most state changes– it’s simple and not that costly
 if you have a low number of batches, fewtriangles and a short pipeline. Alas, batch and triangle counts have gone upand pipelines have gotten long, so the cost for this type of approach has shotup. It’s still alive and kicking for stuff that’s either changed
 infrequently(a dozen partial pipeline flushes aren’t that big a deal over the course of awhole frame) or just too expensive/difficult to implement with more specificschemes though.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;You can make hardware units completely stateless. Justpass the state change command through up to the stage that cares about it; thenhave that stage append the current state to everything it sends downstream,every cycle. It’s not stored anywhere
 – but it’s always around, so if somepipeline stage wants to look at a few bits in the state it can, because they’repassed in (and then passed on to the next stage). If your state happens to bejust a few bits, this isn’t fairly cheap and practical. If it happens
 to be thefull set of active textures along with texture sampling state, not so much.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sometimes storing just one copy of the state and havingto flush every time that stage changes serializes things too much, but thingswould really be fine if you had two copies (or maybe four?) so yourstate-setting frontend could get a bit ahead. Say
 you have enough registers(“slots”) to store two versions of every state, and some active job referencesslot 0\. You can safely modify slot 1 without stopping that job, or otherwiseinterfering with it at all. Now you don’t need to send the whole state aroundthrough
 the pipeline – only a single bit per command that selects whether touse slot 0 or 1\. Of course, if both slot 0 and 1 are busy by the time a statechange command is encountered, you still have to wait, but you can get one stepahead. The same technique works
 with more than two slots.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For some things like sampler or texture Shader ResourceView state, you could be setting very large numbers of them at the same time,but chances are you aren’t. You don’t want to reserve state space for 2*128active textures just because you’re keeping
 track of 2 in-flight state sets soyou might need it. For such cases, you can use a kind of register renamingscheme – have a pool of 128 physical texture descriptors. If someone actuallyneeds 128 textures in one shader, then state changes are gonna be slow.
 (Toughbreak). But in the more likely case of an app using less than 20 textures, youhave quite some headroom to keep multiple versions around.

This is not meant to be a comprehensive list – but the main point is thatsomething that looks as simple as changing a variable in your app (and even inthe UMD/KMD and the command buffer for that matter!) might actually need anontrivial amount of supporting
 hardware behind it just to prevent it fromslowing things down.

**Synchronization**

Finally, the last family of commands deals with CPU/GPU and GPU/GPU synchronization.

Generally, all of these have the form “if event X happens, do Y”. I’ll dealwith the “do Y” part first – there’s two sensible options for what Y can behere: it can be a push-model notification where the GPU yells at the CPU to dosomething&nbsp;_right now_&nbsp;(“Oi!
 CPU! I’m entering the verticalblanking interval on display 0 right now, so if you want to flip bufferswithout tearing, this would be the time to do it!”), or it can be a pull-modelthing where the GPU just memorizes that something happened and the CPU canlater
 ask about it (“Say, GPU, what was the most recent command buffer fragmentyou started processing?” – “Let me check… sequence id 303.”). The former istypically implemented using interrupts and only used for infrequent and high-priorityevents because interrupts
 are fairly expensive. All you need for the latter issome CPU-visible GPU registers and a way to write values into them from thecommand buffer once a certain event happens.

Say you have 16 such registers. Then you could assign currentCommandBufferSeqIdto register 0\. You assign a sequence number to every command buffer you submitto the GPU (this is in the KMD), and then at the start of each command buffer,you add a “If you get
 to this point in the command buffer, write&nbsp;&nbsp;toregister 0″. And voila, now we know which command buffer the GPU is currentlychewing on! And we know that the command processor finishes commands strictlyin sequence, so if the first command in command buffer 303
 was executed, thatmeans all command buffers up to and including sequence id 302 are finished andcan now be reclaimed by the KMD, freed, modified, or turned into a cheesyamusement park.

We also now have an example of what X could be: “if you get here” – perhaps thesimplest example, but already useful. Other examples are “if all shaders havefinished all texture reads coming from batches before this point in the commandbuffer” (this marks safe
 points to reclaim texture/render target memory), “ifrendering to all active render targets/UAVs has completed” (this marks pointsat which you can actually safely use them as textures), “if all operations upto this point are fully completed”, and so on.

Such operations are usually called “fences”, by the way. There’s differentmethods of picking the values you write into the status registers, but as faras I am concerned, the only sane way to do it is to use a sequential counterfor this (probably stealing some
 of the bits for other information). Yeah, I’mreally just dropping that one piece of random information without any rationalewhatsoever here, because I think you should know. I might elaborate on it in alater blog post (though not in this series) :).

So, we got one half of it – we can now report status back from the GPU to theCPU, which allows us to do sane memory management in our drivers (notably, wecan now find out when it’s safe to actually reclaim memory used for vertexbuffers, command buffers, textures
 and other resources). But that’s not all ofit – there’s a puzzle piece missing. What if we need to synchronize purely onthe GPU side, for example? Let’s go back to the render target example. We can’tuse that as a texture until the rendering is actually finished
 (and some othersteps have taken place – more details on that once I get to the texturingunits). The solution is a “wait”-style instruction: “Wait until register Mcontains value N”. This can either be a compare for equality, or less-than(note you need to deal
 with wraparounds here!), or more fancy stuff – I’m justgoing with equals for simplicity. This allows us to do the render target syncbefore we submit a batch. It also allows us to build a full GPU flushoperation: “Set register 0 to &#43;&#43;seqId if all pending jobs
 finished” / “Waituntil register 0 contains seqId”. Done and done. GPU/GPU synchronization:solved – and until the introduction of DX11 with Compute Shaders that haveanother type of more fine-grained synchronization, this was usually the&nbsp;_only_&nbsp;synchronizationmechanism
 you had on the GPU side. For regular rendering, you simply don’t needmore.

By the way, if you can write these registers from the CPU side, you can usethis the other way too – submit a partial command buffer including a wait for aparticular value, and then change the register from the CPU instead of the GPU.This kind of thing can be
 used to implement D3D11-style multithreaded renderingwhere you can submit a batch that references vertex/index buffers that arestill locked on the CPU side (probably being written to by another thread). Yousimply stuff the wait just in front of the actual
 render call, and then the CPUcan change the contents of the register once the vertex/index buffers areactually unlocked. If the GPU never got that far in the command buffer, thewait is now a no-op; if it did, it spend some (command processor) time spinninguntil
 the data was actually there. Pretty nifty, no? Actually, you canimplement this kind of thing even without CPU-writeable status registers if youcan modify the command buffer after you submit it, as long as there’s a commandbuffer “jump” instruction. The details
 are left to the interested reader :)

Of course, you don’t necessarily need the set register/wait register model; forGPU/GPU synchronization, you can just as simply have a “rendertarget barrier”instruction that makes sure a render target is safe to use, and a “flush everything” command. But I like
 the set register-style model more because itkills two birds (back-reporting of in-use resources to the CPU, and GPUself-synchronization) with one well-designed stone.

![]()

**Update:**&nbsp;Here, I’ve drawn&nbsp;[<span style="color:#336699">a diagram</span>](http://www.farbrausch.de/~fg/gpu/command_processor.jpg)&nbsp;for you. It got a bit convoluted so I’m going to lower the amount of
 detail in the future. The basicidea is this: The command processor has a FIFO in front, then the commanddecode logic, execution is handled by various blocks that communicate with the 2Dunit, 3D front-end (regular 3D rendering) or shader units directly (computeshaders),
 then there’s a block that deals with sync/wait commands (which hasthe publicly visible registers I talked about), and one unit that handlescommand buffer jumps/calls (which changes the current fetch address that goesto the FIFO). And all of the units we dispatch
 work to need to send us backcompletion events so we know when e.g. textures aren’t being used anymore andtheir memory can be reclaimed.

**Closing remarks

**Next step down is the first one doing any actual rendering work. Finally,only 3 parts into my series on GPUs, we actually start looking at some vertexdata! (No, no triangles being rasterized yet. That will take some more time).

Actually, at this stage, there’s already a fork in the pipeline; if we’rerunning compute shaders, the next step would already be … running computeshaders. But we aren’t, because compute shaders are a topic for later parts!Regular rendering pipeline first.

Small disclaimer: Again, I’m giving you the broad strokes here, going into details where it’s necessary (or interesting), but trust me, there’s a lot ofstuff that I dropped for convenience (and ease of understanding). That said, Idon’t think I left out anything
 really important. And of course I might’vegotten some things wrong. If you find any bugs, tell me!

Until the next part…

**A trip through the Graphics Pipeline 2011, part 3**&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp; July 3, 2011&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;

At this point, we’ve sent draw calls down from our app all the way throughvarious driver layers and the command processor; now,&nbsp;_finally_&nbsp;we’reactually going to do some graphics processing on it! In this part, I’ll look atthe vertex pipeline. But before
 we start…

**Have some Alphabet Soup!

**We’re now in the 3D pipeline proper, which in turn consists of severalstages, each of which does one particular job. I’m gonna give names to all thestages I’ll talk about – mostly sticking with the “official” D3D10/11 names forconsistency – plus the
 corresponding acronyms. We’ll see all of theseeventually on our grand tour, but it’ll take a while (and several more parts)until we see most of them – seriously, I made a small outline of the ground Iwant to cover, and this series will keep me busy for at
 least 2 weeks! Anyway,here goes, together with a one-sentence summary of what each stage does.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IA — Input Assembler. Reads index and vertex data.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;VS — Vertex shader. Gets input vertex data, writes outprocessed vertex data for the next stage.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PA — Primitive Assembly. Reads the vertices that make upa primitive and passes them on.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HS — Hull shader; accepts patch primitives, writestransformed (or not) patch control points, inputs for the domain shader, plussome extra data that drives tessellation.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TS — Tessellator stage. Creates vertices and connectivityfor tessellated lines or triangles.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DS — Domain shader; takes shaded control points, extradata from HS and tessellated positions from TS and turns them into verticesagain.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GS — Geometry shader; inputs primitives, optionally withadjacency information, then outputs different primitives. Also the primary hubfor…

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SO — Stream-out. Writes GS output (i.e. transformed primitives)to a buffer in memory.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RS — Rasterizer. Rasterizes primitives.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PS — Pixel shader. Gets interpolated vertex data, outputspixel colors. Can also write to UAVs (unordered access views).

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;OM — Output merger. Gets shaded pixels from PS, doesalpha blending and writes them back to the backbuffer.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CS — Compute shader. In its own pipeline all by itself.Only input is constant buffers&#43;thread ID; can write to buffers and UAVs.

And now that that’s out of the way, here’s a list of the various data pathsI’ll be talking about, in order: (I’ll leave out the IA, PA, RS and OM stagesin here, since for our purposes they don’t actually do anything&nbsp;_to_&nbsp;thedata, they just rearrange/reorder
 it – i.e. they’re essentially glue)

1.&nbsp;&nbsp;&nbsp;&nbsp; VS→PS: Ye Olde ProgrammablePipeline. In D3D9, this was all you got. Still the most important path forregular rendering by far. I’ll go through this from beginning to end thendouble back to the fancier paths once I’m done.

2.&nbsp;&nbsp;&nbsp;&nbsp; VS→GS→PS: Geometry Shading(new with D3D10).

3.&nbsp;&nbsp;&nbsp;&nbsp; VS→HS→TS→DS→PS,VS→HS→TS→DS→GS→PS: Tessellation (new in D3D11).

4.&nbsp;&nbsp;&nbsp;&nbsp; VS→SO, VS→GS→SO,VS→HS→TS→DS→GS→SO: Stream-out (with and without tessellation).

5.&nbsp;&nbsp;&nbsp;&nbsp; CS: Compute. New in D3D11.

And now that you know what’s coming up, let’s get started on vertex shaders!

**Input Assembler stage**

The very first thing that happens here is loading indices from the index buffer– if it’s an indexed batch. If not, just pretend it was an identity indexbuffer (0 1 2 3 4 …) and use that as index instead. If there is an indexbuffer, its contents are read from
 memory at this point – not directly though,the IA usually has a data cache to exploit locality of index/vertex bufferaccess. Also note that index buffer reads (in fact, all resource accesses inD3D10&#43;) are bounds checked; if you reference elements outside the
 originalindex buffer (for example, issue a DrawIndexed with IndexCount == 6 from a5-index buffer) all out-of-bounds reads return zero. Which (in this particularcase) is completely useless, but well-defined. Similarly, you can issue aDrawIndexed with a NULL
 index buffer set – this behaves the same way as if youhad an index buffer of size zero set, i.e. all reads are out-of-bounds andhence return zero. With D3D10&#43;, you have to work some more to get into therealm of undefined behavior. :)

Once we have the index, we have all we need to read both per-vertex andper-instance data (the current instance ID is just another counter, fairlystraightforward, at this stage anyway) from the input vertex streams. This isfairly straightforward – we have a
 declaration of the data layout; just read itfrom the cache/memory and unpack it into the float format that our shader coreswant for input. However, this read isn’t done immediately; the hardware isrunning a cache of shaded vertices, so that if one vertex is
 referenced bymultiple triangles (and in a fully regular closed triangle mesh, each vertexwill be referenced by about 6 tris!) it doesn’t need to be shaded every time –we just reference the shaded data that’s already there!

**Vertex Caching and Shading**_

Note_: The contents of this section are, in part, guesswork. They’re basedon public comments made by people “in the know” about current GPUs, but thatonly gives me the “what”, not the “why”, so there’s some extrapolation here.Also, I’m simply guessing some
 of the details here. That said, I’m not talkingcompletely out of my ass here – I’m confident that what I’m describing here isboth reasonable and works (in the general sense), I just can’t guarantee thatit’s actually that way in real HW or that I didn’t miss
 any tricky details. :)

Anyway. For a long time (up to and including the shader model 3.0 generation ofGPUs), vertex and pixel shaders were implemented with different units that haddifferent performance trade-offs, and vertex caches were a fairly simpleaffair: usually just a FIFO
 for a small number (think one or two dozen) ofvertices, with enough space for a worst-case number of output attributes, usingthe vertex index as a tag. As said, fairly straightforward stuff.

And then unified shaders happened. If you unify two types of shaders that usedto be different, the design is necessarily going to be a compromise. So on theone hand, you have vertex shaders, which (at that time) touched maybe up to 1million vertices a frame
 in normal use. On the other hand you had pixelshaders, which at 1920×1200 need to touch&nbsp;_at least_&nbsp;2.3 millionpixels a frame&nbsp;_just to fill the whole screen once_&nbsp;– and a lotmore if you want to render anything interesting. So guess which of the
 twounits ended up pulling the short straw?

Okay, so here’s the deal: instead of the vertex shader units of old that shadedmore or less one vertex at a time, you now have a huge beast of a unifiedshader unit that’s designed for maximum throughput, not latency, and hencewants large batches of work (How
 large? Right now, the magic number seems to bebetween 16 and 64 vertices shaded in one batch).

So you need between 16-64 vertex cache misses until you can dispatch one vertexshading load, if you don’t want to shade inefficiently. But the whole FIFOthing doesn’t really play ball with this idea of batching up vertex cachemisses and shading them in one
 go. The problem is this: if you shade a wholebatch of vertices at once, that means you can only actually start assemblingtriangles once all those vertices have finished shading. At which point you’vejust added a whole batch (let’s just say 32 here and in the
 following) ofvertices to the end of the FIFO, which means 32 old vertices now fell out – buteach of these 32 vertices might’ve been a vertex cache hit for one of thetriangles in the current batch we’re trying to assemble! Uh oh, that doesn’twork. Clearly,
 we can’t actually count the 32 oldest verts in the FIFO asvertex cache hits, because by the time we want to reference them they’ll begone! Also, how big do we want to make this FIFO? If we’re shading 32 verts ina batch, it needs to be at least 32 entries large,
 but since we can’t use the32 oldest entries (since we’ll be shifting them out), that means we’lleffectively start with an empty FIFO on every batch. So, make it bigger, say 64entries? That’s pretty big. And note that every vertex cache lookup involvescomparing
 the tag (vertex index) against all tags in the FIFO – this is fullyparallel, but it also a power hog; we’re effectively implementing a fullyassociative cache here. Also, what do we do between dispatching a shading loadof 32 vertices and receiving results –
 just wait? This shading will take a fewhundred cycles, waiting seems like a stupid idea! Maybe have two shading loadsin flight, in parallel? But now our FIFO needs to be at least 64 entries long,and we can’t count the last 64 entries as vertex cache hits,
 since they’ll beshifted out by the time we receive results. Also, one FIFO vs. lots of shadercores?&nbsp;[<span style="color:#336699">Amdahl’s law</span>](http://en.wikipedia.org/wiki/Amdahl%27s_law)&nbsp;still holds – puttingone
 strictly serial component in a pipeline that’s otherwise completelyparallel is a surefire way to make it the bottleneck.

This whole FIFO thing really doesn’t adapt well to this environment, so, well,just throw it out. Back to the drawing board. What do we actually want to do?Get a decently-sized batch of vertices to shade, and not shade vertices (much)more often than necessary.

So, well, keep it simple: Reserve enough buffer space for 32 vertices (=1batch), and similarly cache tag space for 32 entries. Start with an empty“cache”, i.e. all entries invalid. For every primitive in the index buffer, doa lookup on all the indices; if it’s
 a hit in the cache, fine. If it’s a miss,allocate a slot in the current batch and add the new index to the cache tagarray. Once we don’t have enough space left to add a new primitive anymore,dispatch the whole batch for vertex shading, save the cache tag array
 (i.e. the32 indices of the vertices we just shaded), and start setting up the nextbatch, again from an empty cache – ensuring that the batches are completelyindependent.

Each batch will keep a shader unit busy for some while (probably at least a fewhundred cycles!). But that’s no problem, because we got plenty of them – justpick a different unit to execute each batch! Presto parallelism. We’lleventually get the results back.
 At which point we can use the saved cache tagsand the original index buffer data to assemble primitives to be sent down thepipeline (this is what “primitive assembly” does, which I’ll cover in the laterpart).

By the way, when I say “get the results back”, what does that mean? Where dothey end up? There’s two major choices: 1\. specialized buffers or 2\. somegeneral cache/scratchpad memory. It used to be 1), with a fixed organizationdesigned around vertex data (with
 space for 16 float4 vectors of attributes pervertex and so on), but lately GPUs seem to be moving towards 2), i.e. “justmemory”. It’s more flexible, and has the distinct advantage that you can usethis memory for other shader stages, whereas things like specialized
 vertexcaches are fairly useless for the pixel shading or compute pipeline, to givejust one example.

&nbsp;![]()

**Update**: And here’s a&nbsp;[<span style="color:#336699">picture</span>](http://www.farbrausch.de/~fg/gpu/vertex_shade.jpg)&nbsp;of the vertex shading data flow as described so far.

**Shader Unit internals**

Short versions: It’s pretty much what you’d expect from looking at disassembledHLSL compiler output (fxc /dumpbin is your friend!). Guess what, it’s justprocessors that are&nbsp;_really good_&nbsp;at running that kind of code,and the way that kind of thing is
 done in hardware is building something thateats something fairly close to shader bytecode, in spirit anyway. And unlikethe stuff that I’ve been talking about so far, it’s fairly well documented too– if you’re interested, just check out conference presentations
 from AMD andNVidia or read the documentation for the CUDA/Stream SDKs.

Anyway, here’s the executive summary: fast ALU mostly built around a FMAC(Floating Multiply-ACcumulate) unit, some HW support for (at least) reciprocal,reciprocal square root, log2, exp2, sin, cos, optimized for high throughput andhigh density not low latency,
 running a high number of threads to cover saidlatency, fairly small number of registers per thread (since you’re running somany of them!), very good at executing straight-line code, bad at branches(especially if they’re not coherent).

All that is common to pretty much all implementations. There’s somedifferences, too; AMD hardware used to stick directly with the 4-wide SIMDimplied by the HLSL/GLSL and shader bytecode (even though they seem to bemoving away from that lately), while NVidia
 decided to rather turn the 4-waySIMD into scalar instructions a while back. Again though, all that’s on the Webalready!

What’s interesting to note though is the&nbsp;_differences_&nbsp;betweenthe various shader stages. The short version is that really are rather few ofthem; for example, all the arithmetic and logic instructions are exactly thesame across all stages. Some constructs
 (like derivative instructions andinterpolated attributes in pixel shaders) only exist in some stages; butmostly, the differences are just what kind (and format) of data are passed inand out.

There’s one special bit related to shaders though that’s a big enough subjectto deserve a part on its own. That bit is texture sampling (and texture units).Which, it turns out, will be our topic next time! See you then.

**Closing remarks**

Again, I repeat my disclaimer from the “Vertex Caching and Shading” section:Part of that is conjecture on my part, so take it with a grain of salt. Ormaybe a pound. I don’t know.&nbsp;

I’m also not going into any detail on how scratch/cache memory is managed; thebuffer sizes depend (primarily) on the size of batches you process and thenumber of vertex output attributes you expect. Buffer sizing and management is&nbsp;_really_importantfor
 performance, but I can’t meaningfully explain it here, nor do I want to;while interesting, this stuff is very specific to whatever hardware you’retalking about, and not really very insightful.

**A trip through the GraphicsPipeline 2011, part 4**&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;

&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp; July 4, 2011&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

Welcome back. Last part was about vertex shaders, with some coverage of GPUshader units in general. Mostly, they’re just vector processors, but they haveaccess to one resource that doesn’t exist in other vector architectures:Texture samplers. They’re an integral
 part of the GPU pipeline and arecomplicated (and interesting!) enough to warrant their own article, so heregoes.

**Texture state

**Before we start with the actual texturing operations, let’s have a look atthe API state that drives texturing. In the D3D11 part, this is composed of 3distinct parts:

1.&nbsp;&nbsp;&nbsp;&nbsp; The sampler state. Filtermode, addressing mode, max anisotropy, stuff like that. This controls howtexture sampling is done in a general way.

2.&nbsp;&nbsp;&nbsp;&nbsp; The underlying textureresource. This boils down to a pointer to the raw texture bits in memory. Theresource also determines whether it’s a single texture or a texture array, whatmultisample format the texture has (if any), and the physical layout
 of thetexture bits – i.e. at the resource level, it’s not yet decided how the valuesin memory are to be interpreted exactly, but their memory layout is naileddown.

3.&nbsp;&nbsp;&nbsp;&nbsp; The shader resource view (SRVfor short). This determines how the texture bits are to be interpreted by thesampler. In D3D10&#43;, the resource view links to the underlying resource, so younever specify the resource explicitly.

Most of the time, you will create a texture resource with a given format, let’ssay RGBA, 8 bits per component, and then just create a matching SRV. But youcan also create a texture as “8 bits per component, typeless” and then haveseveral different SRVs for
 the same resource that read the underlying data indifferent formats, e.g. once as UNORM8_SRGB (unsigned 8-bit value in sRGB spacethat gets mapped to float 0..1) and once as UINT8 (unsigned 8-bit integer).&nbsp;

Creating the extra SRV seems like an annoying extra step at first, but thepoint is that this allows the API runtime to do all type checking at SRVcreation time; if you get a valid SRV back, that means the SRV and resourceformats are compatible, and no further
 type checking needs to be done whilethat SRV exists. In other words, it’s all about API efficiency here.

Anyway, at the hardware level, what this boils down to is just a bag of stateassociated with a texture sampling operation – sampler state, texture/format touse, etc. – that needs to get kept somewhere (see&nbsp;[<span style="color:#336699">part
 2</span>](http://fgiesen.wordpress.com/2011/07/02/a-trip-through-the-graphics-pipeline-2011-part-2/)&nbsp;for an explanation of various ways to manage statein a pipelined architecture). So again, there’s various methods, from “pipelineflush every time any state changes” to “just go completely stateless in thesampler and send the full set along with
 every texture request”, with variousoptions inbetween. It’s nothing you need to worry about – this is the kind ofthing where HW architects whip up a cost-benefit analysis, simulate a fewworkloads and then take whichever method comes out ahead – but it’s worthrepeating:
 as PC programmer, don’t assume the HW adheres to any particularmodel.

Don’t assume that texture switches are expensive – they might be fullypipelined with stateless texture samplers so they’re basically free. But don’tassume they’re completely free either – maybe they are not fully pipelined orthere’s a cap on the maximum number
 of different sets of texture states in thepipeline at any given time. Unless you’re on a console with fixed hardware (oryou hand-optimize your engine for every generation of graphics HW you’retargeting), there’s just no way to tell. So when optimizing, do
 the obviousstuff – sort by material where possible to avoid unnecessary state changes andthe like – which certainly saves you some API work at the very least, and thenleave it at that. Don’t do anything fancy based on any particular model of whatthe HW is
 doing, because it can (and will!) change in the blink of an eyebetween HW generations.

**Anatomy of a texture request

**So, how much information do we need to send along with a texture samplerequest? It depends on the texture type and which kind of sampling instructionwe’re using. For now, let’s assume a 2D texture. What information do we need tosend if we want to do
 a 2D texture sample with, say, up to 4x anisotropicsampling?

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The 2D texture coordinates – 2 floats, and sticking withthe D3D terminology in this series, I’m going to call them u/v and not s/t.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The partial derivatives of u and v along the screen “x”direction:&nbsp;,&nbsp;.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Similarly, we need the partial derivative in the “y”direction too:&nbsp;,&nbsp;.

So, that’s 6 floats for a fairly pedestrian 2D sampling request (of theSampleGrad variety) – probably more than you thought. The 4 gradient values areused both for mipmap selection and to choose the size and shape of theanisotropic filtering kernel. You can
 also use texture sampling instructionsthat explicitly specify a mipmap level (in HLSL, that would be SampleLevel) –these don’t need the gradients, just a single value containing the LODparameter, but they also can’t do anisotropic filtering – the best you’ll
 getis trilinear! Anyway, let’s stay with those 6 floats for a while. That sureseems like a lot. Do we really need to send them along with every texturerequest?

The answer is: depends. In everything but Pixel Shaders, the answer is yes, wereally have to (if we want anisotropic filtering that is). In Pixel Shaders,turns out we don’t; there’s a trick that allows Pixel Shaders to give yougradient instructions (where you
 can compute some value and then ask thehardware “what is the approximate screen-space gradient of this value?”), andthat same trick can be employed by the texture sampler to get all the requiredpartial derivatives just from the coordinates. So for a PS 2D
 “sample”instruction, you really only need to send the 2 coordinates which imply therest, provided you’re willing to do some more math in the sampler units.

Just for kicks: What’s the worst-case number of parameters required for asingle texture sample? In the current D3D11 pipeline, it’s a SampleGrad on aCubemap array. Let’s see the tally:

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3D texture coordinates – u, v, w: 3 floats.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cubemap array index: one int (let’s just bill that at thesame cost as a float here).

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Gradient of (u,v,w) in the screen x and y directions: 6floats.

For a total of 10 values&nbsp;_per pixel sampled_&nbsp;– that’s 40 bytesif you actually store it like that. Now, you might decide that you don’t needfull 32 bits for all of this (it’s probably overkill for the array index andgradients), but it’s still a lot of
 data to be sending around.

In fact, let’s check what kind of bandwidth we’re talking about here. Let’sassume that most of our textures are 2D (with a few cubemaps thrown in), thatmost of our texture sampling requests come from the Pixel Shader with little tono texture samples in the
 Vertex Shader, and that the regular Sample-typerequests are the most frequent, followed by SampleLevel (all of this is prettytypical for actual rendering you see in games). That means the average numberof 32-bit floats values sent per pixel will be somewhere
 between 2 (u&#43;v) and 3(u&#43;v&#43;w / u&#43;v&#43;lod), let’s say 2.5, or 10 bytes.

Assume a medium resolution – say, 1280×720, which is about 0.92 million pixels.How many texture samples does your average game pixel shader have? I’d say atleast 3\. Let’s say we have a modest amount of overdraw, so during the 3Drendering phase, we touch each
 pixel on the screen roughly twice. And then wefinish it off with a few texture-heavy full-screen passes to dopost-processing. That probably adds at least another 6 samples per pixel,taking into account that some of that postprocessing will be done at a lowerresolution.
 Add it all up and we have 0.92 * (3*2 &#43; 6) = about 11 milliontexture samples per frame, which at 30 fps is about 330 million a second. At 10bytes per request, that’s 3.3 GB/s_just for texture request payloads_.Lower bound, since there’s some extra overhead
 involved (we’ll get to that in asecond). Note that I’m *cough* erring “a bit” on the low side with all of thesenumbers :). An actual modern game on a good DX11 card will run in significantlyhigher resolution, with more complex shaders than I listed, comparable
 amountof overdraw or even somewhat less (deferred shading/lighting to the rescue!),higher frame rate, and way more complex postprocessing – go ahead, do a quickback-of-the-envelope calculation how much texture request bandwidth adecent-quality SSAO pass in
 quarter-resolution with bilateral upsampling takes…

Point being, this whole texture bandwidth thing is not something you can justhand-wave away. The texture samplers aren’t part of the shader cores, they’reseparate units some distance away on the chip, and shuffling multiple gigabytesper second around isn’t
 something that just happens by itself. This is anactual architectural issue – and it’s a good thing we don’t use SampleGrad onCubemap arrays for everything :)

**But who asks for a single texture sample?**

The answer is of course:&nbsp;_No one_. Our texture requests are comingfrom shader units, which we know process somewhere between 16 and 64 pixels /vertices / control points / … at once. So our shaders won’t be sendingindividual texture samples, they’ll dispatch
 a bunch of them at once. Thistime, I’ll use 16 as the number – simply because the 32 I chose last time isnon-square, which just seems weird when talking about 2D texture requests. So,16 texture requests at once – build that texture request payload, add somecommand
 fields at the start so the sampler knows what to do, add some morefields so the sampler knows which texture and sampler state to use (again, seethe remarks above on state), and send that off to a texture sampler somewhere.

This will take a while.

No, seriously. Texture samplers have a seriously long pipeline (we’ll soon seewhy); a texture sampling operation takes&nbsp;_way_too long for a shaderunit to just sit idle for all that time. Again, say it with me:&nbsp;_throughput_.So what happens is that
 on a texture sample, a shader unit will just quietlyswitch to another thread/batch and do some other work, then switch back a whilelater when the results are there. Works just fine as long as there’s enoughindependent work for the shader units to do!

**

And once the texture coordinates arrive…**

Well, there’s a bunch of computations to be done first: (In here and thefollowing, I’m assuming a simple bilinear sample; trilinear and anisotropictake some more work, see below).

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If this is a Sample or SampleBias-type request, calculatetexture coordinate gradients first.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If no explicit mip level was given, calculate the miplevel to be sampled from the gradients and add the LOD bias if specified.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For each resulting sample position, apply the addressmodes (wrap / clamp / mirror etc.) to get the right position in the texture tosample from, in normalized [0,1] coordinates.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If this is a cubemap, we also need to determine whichcube face to sample from (based on the absolute values and signs of the u/v/wcoordinates), and do a division to project the coordinates onto the unit cubeso they are in the [-1,1] interval. We
 also need to drop one of the 3coordinates (based on the cube face) and scale/bias the other 2 so they’re inthe same [0,1] normalized coordinate space we have for regular texture samples.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Next, take the [0,1] normalized coordinates and convertthem into fixed-point pixel coordinates to sample from – we need somefractional bits for the bilinear interpolation.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Finally, from the integer x/y/z and texture array index,we can now compute the address to read texels from. Hey, at this point, what’sa few more multiplies and adds among friends?

If you think it sounds bad summed up like that, let me take remind you thatthis is a simplified view. The above summary doesn’t even cover fun issues suchas texture borders or sampling cubemap edges/corners. Trust me, it may soundbad now, but if you were to
 actually write out the code for everything thatneeds to happen here, you’d be positively horrified. Good thing we havededicated hardware to do it for us. :) Anyway, we now have a memory address toget data from. And wherever there’s memory addresses, there’s
 a cache or twonearby.

**Texture cache**

Everyone seems to be using a two-level texture cache these days. Thesecond-level cache is a completely bog-standard cache that happens to cachememory containing texture data. The first-level cache is not quite as standard,because it’s got additional smarts.
 It’s also smaller than you probably expect– on the order of 4-8kb per sampler. Let’s cover the size first, because ittends to come as a surprise to most people.

The thing is this: Most texture sampling is done in Pixel Shaders withmip-mapping enabled, and the mip level for sampling is specifically chosen tomake the screen pixel:texel ratio roughly 1:1 – that’s the whole point. Butthis means that, unless you happen
 to hit the exact same location in a textureagain and again, each texture sampling operation will miss about 1 texel onaverage – the actual measured value with bilinear filtering is around 1.25misses/request (if you track pixels individually). This value stays
 more orless unchanged for a long time even as you change texture cache size, and thendrops dramatically as soon as your texture cache is large enough to contain thewhole texture (which usually is between a few hundred kilobytes and severalmegabytes, totally
 unrealistic sizes for a L1 cache).

Point being,&nbsp;_any_&nbsp;texture cache whatsoever is a massive win(since it drops you down from about 4 memory accesses per bilinear sample downto 1.25). But unlike with a CPU or shared memory for shader cores, there’s verylittle gain in going from say 4k
 of cache to 16k; we’re streaming larger texturedata through the cache no matter what.

Second point: Because of the 1.25 misses/sample average, texture samplerpipelines need to be long enough to sustain a full read from memory per samplewithout stalling. Let me phrase that differently: texture sampler pipes arelong enough to not stall for a memory
 read&nbsp;_even though it takes 400-800cycles_. That’s one seriously long pipeline right there – and it really is apipeline in the literal sense, handing data from one pipeline register to thenext for a few hundred cycles without any processing until the
 memory read iscompleted.

So, small L1 cache, long pipeline. What about the “additional smarts”? Well,there’s compressed texture formats. The ones you see on PC – S3TC aka DXTC akaBC1-3, then BC4 and 5 which were introduced with D3D10 and are just variationson DXT, and finally BC6H
 and 7 which were introduced with D3D11 – are allblock-based methods that encode blocks of 4×4 pixels individually. If youdecode them during texture sampling, that means you need to be able to decodeup to 4 such blocks (if your 4 bilinear sample points happen
 to land in theworst-case configuration of straddling 4 blocks) per cycle and get a singlepixel from each. That, frankly, just sucks. So instead, the 4×4 blocks aredecoded when it’s brought into the L1 cache: in the case of BC3 (aka DXT5), youfetch one 128-bit
 block from texture L2, and then decode that into 16 pixels inthe texture cache. And suddenly, instead of having to partially decode up to 4blocks per sample, you now only need to decode 1.25/(4*4) = about 0.08 blocksper sample, at least if your texture access
 patterns are coherent enough to hitthe other 15 pixels you decoded alongside the one you actually asked for :).Even if you only end up using part of it before it goes out of L1 again, that’sstill a massive improvement. Nor is this technique limited to DXT
 blocks; youcan handle most of the differences between the &gt;50 different texture formatsrequired by D3D11 in your cache fill path, which is hit about a third as oftenas the actual pixel read path – nice. For example, things like UNORM sRGBtextures can be handled
 by converting the sRGB pixels into a 16-bitinteger/channel (or 16-bit float/channel, or even 32-bit float if you want) inthe texture cache. Filtering then operates on that, properly, in linear space.Mind that this does end up increasing the footprint of texels
 in the L1 cache,so you might want to increase L1 texture size; not because you need to cachemore texels, but because the texels you cache are fatter. As usual, it’s atrade-off.

**Filtering**

And at this point, the actual bilinear filtering process is fairlystraightforward. Grab 4 samples from the texture cache, use the fractionalpositions to blend between them. That’s a few more of our usual standby, themultiply-accumulate unit. (Actually a lot
 more – we’re doing this for 4channels at the same time…)

Trilinear filtering? Two bilinear samples and another linear interpolation.Just add some more multiply-accumulates to the pile.

Anisotropic filtering? Now that actually takes some extra work earlier in thepipe, roughly at the point where we originally computed the mip-level to samplefrom. What we do is look at the gradients to determine not just the area butalso the shape of a screen
 pixel in texel space; if it’s roughly as wide as itis high, we just do a regular bilinear/trilinear sample, but if it’s elongatedin one direction, we do several samples across that line and blend the resultstogether. This generates several sample positions,
 so we end up looping throughthe full bilinear/trilinear pipeline several times, and the actual way thesamples are placed and their relative weights are computed is a closely guardedsecret for each hardware vendor; they’ve been hacking at this problem foryears,
 and by now both converged on something pretty damn good at reasonablehardware cost. I’m not gonna speculate what it is they’re doing; truth be told,as a graphics programmer, you just don’t need to care about the underlyinganisotropic filtering algorithm as
 long as it’s not broken and produces eitherterrible artifacts or terrible slowdowns.

Anyway, aside from the setup and the sequencing logic to loop over the requiredsamples, this does not add a significant amount of computation to the pipe. Atthis point we have enough multiply-accumulate units to compute the weighted suminvolved in anisotropic
 filtering without a lot of extra hardware in the actualfiltering stage. :)

**Texture returns**

And now we’re almost at the end of the texture sampler pipe. What’s the resultof all this? Up to 4 values (r, g, b, a) per texture sample requested. Unliketexture requests where there’s significant variation in the size of requests,here the most common case
 by far is just the shader consuming all 4 values. Mindyou, sending 4 floats back is nothing to sneeze at from a bandwidth point ofview, and again you might want to shave bits in some case. If your shader issampling a 32-bit float/channel texture, you’d better
 return 32-bit floats, butif it’s reading a 8-bit UNORM SRGB texture, 32 bit returns are just overkill,and you can save bandwidth by using a smaller format on the return path.

And that’s it – the shader unit now has its texture sampling results back andcan resume working on the batch you submitted – which concludes this part. Seeyou again in the next installment, when I talk about the work that needs to bedone before we can actually
 start rasterizing primitives.&nbsp;**Update**:And here’s a&nbsp;[<span style="color:#336699">picture</span>](http://www.farbrausch.de/~fg/gpu/texture_sample.jpg)&nbsp;of the texture samplingpipeline, including an amusing
 mistake that I’ve fixed in post like a pro!

&nbsp;

&nbsp;![]()

**The usual post-script

**This time, no big disclaimers. The numbers I mentioned in the bandwidthexample are honestly just made up on the spot since I couldn’t be arsed to lookup some actual figures for current games :), but other than that, what Idescribe here should be pretty
 close to what’s on your GPU right now, eventhough I hand-waved past some of the corner cases in filtering and such (mainlybecause the details are more nauseating than they are enlightening).

As for texture L1 cache containing decompressed texture data, to the best of myknowledge this is accurate for current hardware. Some older HW would keep someformats compressed even in L1 texture cache, but because of the “1.25misses/sample for a large range
 of cache sizes” rule, that’s not a big win andprobably not worth the complexity. I think that stuff’s all gone now.

An interesting bit are embedded/power-optimized graphics chips, e.g. PowerVR;I’ll not go into these kinds of chips much in this series since my focus hereis on the high-performance parts you see in PCs, but I have some notes aboutthem in the comments for previous
 parts if you’re interested. Anyway, the PVRchips have their own texture compression format that’s not block-based and verytightly integrated with their filtering hardware, so I would assume that theydo keep their textures compressed even in L1 texture cache
 (actually, I don’tknow if they even have a second cache level!). It’s an interesting method andprobably at a fairly sweet spot in terms of useful work done per area andenergy consumed. But I think the “depack to L1 cache” method gives higherthroughput overall,
 and as I can’t mention often enough, it’s all aboutthroughput on high-end PC GPUs :)

&nbsp;

&nbsp;

&nbsp;

&nbsp;

**A trip through the GraphicsPipeline 2011, part 5**

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp; July 5, 2011&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

After the last post about texture samplers, we’re now back in the 3D frontend.We’re done with vertex shading, so now we can start actually rendering stuff,right? Well, not quite. You see, there’s a bunch still left to do before weactually start rasterizing
 primitives. So much so in fact that we’re not goingto see any rasterization in this post – that’ll have to wait until next time.

**Primitive Assembly**

When we left the vertex pipeline, we had just gotten a block of shaded verticesback from the shader units, with the implicit promise that this block containsan integral number of primitives – i.e., we don’t allow triangles, lines or patchesto be split across
 multiple blocks. This is important, because it means we cantruly treat each block independently and never need to buffer more than oneblock of shader output – we can, of course, but we don’t have to.

The next step is to assemble all the vertices belonging to a single primitive(hence “primitive assembly”). If that primitive happens to be a point, thisjust reads exactly one vertex and passes it on. If it’s lines, it reads twovertices. If it’s triangles, three.
 And so on for patches with larger numbersof control points.

In short, all that happens here is that we gather vertices. We can either dothis by reading the original index buffer and keeping a copy of our vertexindex-&gt;cache position map around (as I described), or we can store theindices for the fully expanded primitives
 along with the shaded vertex data,which might take a bit more space for the output buffer but means we don’t haveto read the indices again here. Either way works fine.

And now we have expanded out all the vertices that make up a primitive. Inother words, we now have complete triangles, not just a bunch of vertices. Socan we rasterize them already? Not quite.

**Viewport culling and clipping**

Oh yeah, that. Yeah, I guess we’d better do that first, huh? This is one partof pipeline that really does exactly what you’d expect, pretty much the way youwould expect it too (i.e. the way it’s explained in the docs). So I’m not gonnaexplain polygon clipping
 in general here, you can look that up in any computer graphicstextbook, although most make a terrible mess of it; if you want a goodexplanation, use Jim Blinn’s (chapter 13 of&nbsp;[<span style="color:#336699">this
 book</span>](http://www.amazon.com/Jim-Blinns-Corner-Graphics-Pipeline/dp/1558603875)[url=]), although you probably want to pass on hisalternative [0,w] clip space these days, to avoid confusion if nothingelse.[/url]

[url=]Anyway, clipping. The short version is this: Your vertex shader returnsvertex positions on homogeneous clip space. Clip space is chosen to make theequations that describe the view frustum as simple as possible; in the case ofD3D, they are&nbsp;,&nbsp;,&nbsp;, and&nbsp;;
 note that all the last equationreally does is exclude the homogeneous point (0,0,0,0), which is something of adegenerate case.[/url][url=]We first need to find out if the triangle ispartially or even completely outside any of these clip planes. This can be
 donevery efficiently using[/url][<span style="color:#336699">Cohen-Sutherland</span>](http://en.wikipedia.org/wiki/Cohen%E2%80%93Sutherland)-style out-codess. You computethe clip out-code (or just clip-code) for each
 vertex (this can be done atvertex shading time and stored along with the positions, for example). Then,for each primitive, the bitwise AND of the clip-codes will tell you all theview-frustum planes that&nbsp;_all_&nbsp;vertices in the primitive are onthe wrong
 side of (if there’s any, that means the primitive is completelyoutside the view frustum and can be thrown away), and the bitwise OR of theclip-codes will tell you the planes that you need to clip the primitiveagainst. Given the clipcodes, all this is just
 a few gates worth of hardware –simple stuff.

Additionally, the shaders can also generate a set of “cull distances” (atriangle will be discarded if any one cull distance for all vertices is lessthan zero), and a set of “clip distances” (which define additional clippingplanes). These get considered for
 primitive rejection/clip testing too.

The actual clipping process, if invoked, can take one of two forms: we caneither use an actual polygon clipping algorithm (which adds extra vertices andtriangles), or we can add the clipping planes as extra edge equations to therasterizer (if that sounds like
 gibberish to you, wait until the next partwhere I explain rasterization – it’ll ask make sense eventually). The latter ismore elegant and doesn’t require an actual polygon clipper at all, but we needto be able to handle all normalized 32-bit floating point
 values as validvertex coordinates; there might be a trick for building a fast HW rasterizerthat does this, but it seems tricky to say the least. So I’m assuming there’san actual clipper, with all that involves (generation of extra triangles etc).This is a
 nuisance, but it’s also very infrequent (more so than you think, I’llget to that in a second), so it’s not a big deal. Not sure if that’s specialhardware either, or if that path grabs a shader unit to do the actual clipping;depends on whether dispatching a
 new vertex shading load at this stage isawkward or not, how big a dedicated clipping unit is, and how many of them youneed. I don’t know the answer to these questions, but at least from theperformance side of things, it doesn’t much matter: we don’t really
 clip thatoften. That’s because we can use guard-band clipping.

**Guard-band clipping**

The name is something of a misnomer; it’s not a fancy way of doing clipping. Infact, it’s quite the opposite: a straight-forward way of not doing clipping. :)

The underlying idea is very simple: Most primitives that are partially outsidethe left, right, top and bottom clip planes don’t need to be clipped at all.Triangle rasterization on GPUs works by, in effect, scanning over the fullscreen area (or more precisely,
 the scissor rect) and asking for every pixel:“is this pixel covered by the current triangle?” (In reality it’s a bit morecomplicated and way more efficient than that, but that’s the general idea). Andthat works just as well for triangles completely within
 the viewport as it doesfor triangles that extend past, say, the right and top clipping planes. As longas our triangle coverage test is reliable, we don’t need to clip against theleft, right, top and bottom planes at all!

That test is usually done in integer arithmetic with some fixed precision. Andeventually, as you move say one triangle vertex further and further out, you’llget integer overflows and wrong test results. I think we can all agree that therasterizer producing
 pixels that aren’t actually inside the triangle is, at thevery least, extremely offensive behavior and should be illegal! Which it infact is – hardware that does this is in violation of the spec.

There’s two solutions for this problem: The first is to make sure that yourtriangle tests never, ever generate the wrong results, no matter how your inputtriangle looks. If you manage that, then you don’t ever need to clip againstthe aforementioned four planes.
 This is called “infinite guard-band” because,well, the guard-band is effectively infinite. Solution two is to clip triangleseventually, just as they’re about to go outside the safe range where therasterizer calculations can’t overflow. For example, say that
 your rasterizerhas enough internal bits to deal with integer triangle coordinates that have&nbsp;,&nbsp;&nbsp;(note I’m using capital X and Yto denote screen-space positions; I’ll stick with this convention). You stilldo your viewport cull test (i.e. “is this triangle outside
 the view frustum”)with the regular view planes, but only actually clip against the guard-bandclip planes which are chosen so that after the projection and viewporttransforms, the resulting coordinates are in the safe range. I guess it’s timefor an image:

![]()

Guard-band clipping

The small white rectangle with blue outline that’s roughly in the middlerepresents our viewport, while the big salmon-colored area around it is ourguard band. It looks like a small viewport in this image, but I actually pickeda huge one so you can see anything!
 With our -32768 .. 32767 guard-band cliprange, that viewport would be about 5500 pixels wide – yes, that’s some hugetriangles right there :). Anyway, the triangles show off some of the importantcases. The yellow triangle is the most common case – a triangle
 that extendsoutside the viewport but not the guard band. This just gets passed straightthrough, no further processing necessary. The green triangle is fully withinthe guard band, but outside the viewport region, so it would never get here –it’s been rejected
 above by the viewport cull. The blue triangle extendsoutside the guard-band clip region and would need to be clipped, but again it’sfully outside the viewport region and gets rejected by the viewport cull.Finally, the purple triangle extends both inside the
 viewport and outside theguard band, and so actually needs to be clipped.

As you can see, the kinds of triangles you need to actually have to clipagainst the four side planes are pretty extreme. As said, it’s infrequent –don’t worry about it.

**Aside: Getting clipping right**

None of this should be terribly surprising; nor should it sound too difficult,at least if you’re familiar with the algorithms. But the devil’s in thedetails, always. Here’s some of the non-obvious rules the triangle clipper hasto obey in practice. If it ever
 breaks&nbsp;_any_&nbsp;of these rules,there’s cases where it will produce cracks between adjacent triangles thatshare an edge. This isn’t allowed.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vertex positions that are inside the view frustum must bepreserved, bit-exact, by the clipper.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Clipping an edge AB against a plane must produce the sameresults, bit-exact, as clipping the edge BA (orientation reversed) against thatplane. (This can be ensured by either making the math completely symmetric, oralways clipping an edge in the same
 direction, say from the outside in).

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Primitives that are clipped against multiple planes mustalways clip against planes in the same order. (Either that or clip against allplanes at once)

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If you use a guard band, you&nbsp;_must_&nbsp;clipagainst the guard band planes; you can’t use a guard band for some trianglesbut then clip against the original viewport planes if you actually need toclip. Again, failing to do this will cause cracks
 – and if I remember correctlythere was actually a piece of graphics hardware in the bad old days thatshipped with this bug enshrined in silicon. Oops. :)

**

Those pesky near and far planes**

Okay, so we have a really nice quick solution for the 4 side planes, but whatabout near and far? Particularly the near plane is bothersome, since with allthe stuff that’s only slightly outside the viewport handled, that’s the planewe do most of our clipping
 for. So what can we do? A z guard band? But howwould that work – we’re not actually rasterizing along the z axis at all! Infact, it’s just some value we interpolate over the triangle, damn!

On the plus side, though, it’s just some value we interpolate over thetriangle. And in fact the z-near test () is&nbsp;_really easy_&nbsp;to doonce you interpolate Z – it’s just the sign bit. z-far () is an extra compare though (not I’musing Z not z here, i.e.
 these are “screen” or post-projection coordinates).But still, we’re doing Z-compares per pixel anyway (Z test!), so it’s not a bigextra expense. It depends, but doing z-clip this way is definitely an option. Andyou need to be able to skip z-near/z-far clipping
 if you want to support thingslike NVidias ‘depth clamp’ OpenGL extension; in fact, I would argue theexistence of that extension is a pretty good hint that they’re doing this, orat least used to for a while.

So we’re down to one of the regular clip planes:&nbsp;. Can we get rid of this one too? Theanswer is yes, with a rasterization algorithm that works in homogeneouscoordinates, e.g.&nbsp;[<span style="color:#336699">this
 one</span>](http://www.cs.unc.edu/~olano/papers/2dh-tri/). I’m not sure whetherhardware uses that one though. It’s nice an elegant, but it seems like it wouldbe hard to obey the (very strict!) D3D11 rasterization rules to the letterusing that algorithm. But maybe there’s some cool tricks that I’m not
 aware of.Anyway, that’s about it with clipping.

**Projection and viewport transform**

Projection just takes the x, y and z coordinates and divides them by w (unlessyou’re using a homogeneous rasterizer which doesn’t actually project – but I’llignore that possibility in the following). This gives us normalized devicecoordinates, or NDCs, between
 -1 and 1\. We then apply the viewport transformwhich maps the projected x and y to pixel coordinates (which I’ll call X and Y)and the projected z into the range [0,1] (I’ll call this value Z), such that atthe z-near plane Z=0 and at the z-far plane Z=1.

At this point, we also snap pixels to fractional coordinates on the sub-pixelgrid. As of D3D11, hardware is required to have exactly 8 bits of subpixelprecision for triangle coordinates. This snapping turns some&nbsp;_very_&nbsp;thinslivers (which would otherwise
 cause problems) into degenerate triangles (whichdon’t need to be rendered at all).

**Back-face and other triangle culling**

Once we have X and Y for all vertices, we can calculate the signed trianglearea using a cross product of the edge vectors. If the area is negative, thetriangle is wound counter-clockwise (here, negative areas correspond tocounter-clockwise because we’re now
 in the pixel coordinate space, and in D3Dpixel space y increases downwards not upwards, so signs are inverted). If thearea is positive, it’s wound clockwise. If it’s zero, it’s degenerate anddoesn’t cover any pixels, so it can be safely culled. At this point,
 we knowthe triangle orientation so we can do back-face culling (if enabled).

And that’s it! We’re now ready for rasterization… almost. Actually we have todo triangle setup first. But doing that requires some knowledge of howrasterization will be performed, so I’ll put that off until the next part… seeyou then!

**Final remarks

**Again, I skipped some parts and simplified others, so here’s the usualreminder that things are a bit more complicated in reality: For example, Ipretended that you just use the regular homogeneous clipping algorithm. Mostly,you do – but you can have
 some vertex shader attributes flagged as usingscreen-space linear instead of perspective-correct interpolation. Now, theregular homogeneous clip always does perspective-correct interpolation; in thecase of screen-space linear attributes, you actually need
 to do some extra workto make it not perspective-correct. :)

I talk about primitives some of the time, but mostly I’m just focusing ontriangles here. Points and lines aren’t hard, but let’s be honest, they’re notwhat we’re here for either. You can work out the details if you’re interested.:)

There’s tons of rasterization algorithms out there, some of which (like Olanos2DH method that I cited) allow you to skip nearly all clipping, but as Imentioned, D3D11 has very strict requirements on the triangle rasterizer sothere’s not much wiggle room for
 HW implementations; I’m not sure if thosemethods can be tweaked to exactly follow the spec (there’s a lot of subtlepoints that I’ll cover next time). So here and in the following I’m assumingyou can’t do the ultra-sleek thing; then again, the not-quite-so-sleekapproaches
 I’m running with have slightly less math per pixel in therasterizer, so they might win for HW implementations anyway. And of course Imight be missing the magic pixie dust right around the corner that solves allof these problems. That occurs surprisingly often
 in graphics. If you know anawesome solution, give me a shout in the comments!

Lastly, the triangle culling I’m describing here is the bare minimum; forexample, the class of triangles that will generate zero pixels uponrasterization is much larger than just zero-area tris, and if you can find itout quickly enough (or with few enough gates),
 you can drop the triangleimmediately and don’t need to go through triangle setup. This is the last pointwhere you can cull cheaply before going through triangle setup and at leastsome rasterization – finding other ways to early-reject tris pays offhandsomely
 here.

&nbsp;

A trip through the Graphics Pipeline 2011, part 6

July 6, 2011

Welcome back. This time we’re actually gonna see triangles being rasterized –finally! But before we can rasterize triangles, we need to do triangle setup,and before I can discuss triangle setup, I need to explain what we’re settingthings up for; in other words,
 let’s talk hardware-friendly trianglerasterization algorithms.

**How not to render a triangle**

First, a little heads-up to people who’ve been at this game long enough to havewritten their own optimized software texture mappers: First, you’re probablyused to thinking of triangle rasterizers as this amalgamated blob that does abunch of things at once:
 trace the triangle shape, interpolate u and vcoordinates (or, for perspective correct mapping, u/z, v/z and 1/z), do theZ-buffer test (and for perspective correct mapping, you probably used a 1/zbuffer instead), and then do the actual texturing (plus shading),
 all in onebig loop that’s meticulously scheduled and probably uses all availableregisters. You know the kind of thing I’m talking about, right? Yeah, forgetabout that here. This is hardware. In hardware, you package things up into nicetidy little modules that
 are easy to design and test in isolation. In hardware,the “triangle rasterizer” is a block that tells you what (sub-)pixels atriangle covers; in some cases, it’ll also give you barycentric coordinates ofthose pixels inside the triangle. But that’s it. No u’s
 or v’s – not even1/z’s. And certainly no texturing and shading, through with the dedicated textureand shader units that should hardly come as a surprise.

Second, if you’ve written your own triangle mappers “back in the day”, youprobably used an incremental scanline rasterizer of the kind described in ChrisHecker’s series on Perspective Texture Mapping. That happens to be a great wayto do it in sofware on processors
 without SIMD units, but it doesn’t map wellto modern processors with fast SIMD units, and even worse to hardware – notthat it’s stopped people from trying. In particular, there’s a certain datedgame console standing in the corner trying very hard to look nonchalant
 rightnow. The one with that triangle rasterizer that had really fast guard-bandclipping on the bottom and right edges of the screen, and not so fastguard-band clipping for the top and left edges (that, my friends, is what wecall a “tell”). Just saying.

So, what’s bad about that algorithm for hardware? First, it really rasterizestriangles scan-line by scan-line. For reasons that will become obvious once Iget to Pixel Shading, we want our rasterizer to output in groups of 2×2 pixels(so-called “quads” – not
 to be confused with the “quad” primitive that’s beendecomposed into a pair of triangles at this stage in the pipeline). This is allkinds of awkward with the scan-line algorithm because not only do we now needto run two “instances” of it in parallel, they also
 each start at the firstpixel covered by the triangle in their respective scan lines, which may bepretty far apart and doesn’t nicely lead to generating the 2×2 quads we’d liketo get. It’s also hard to parallelize efficiently, not symmetrical in the x andy
 directions – which means a triangle that’s 8 pixels wide and 100 pixelsstresses very different parts of the rasterizer than a triangle that’s 100 pixelswide and 8 pixels high. Really annoying because now you have to make the “x”and “y” stepping “loops” equally
 fast in order to avoid bottlenecks – but we doall our work on the “y” steps, the loop in “x” is trivial! As said, it’s amess.

**A better way**

A much simpler (and more hardware-friendly) way to rasterize triangles waspresented in a 1988 paper by Pineda. The general approach can be summarized in2 sentences: the signed distance to a line can be computed with a 2D dotproduct (plus an add) – just as a
 signed distance to a plane can be computewith a 3D dot product (plus add). And the interior of a triangle can be definedas the set of all points that are on the correct side of all three edges. So…just loop over all candidate pixels and test whether they’re
 actually insidethe triangle. That’s it. That’s the basic algorithm.

Note that when we move e.g. one pixel to the right, we add one to X and leave Ythe same. Our edge equations have the form E(X,Y) = aX &#43; bY &#43; c, with a, b, cbeing per-triangle constants, so for X&#43;1 it will be E(X&#43;1,Y) = a(X&#43;1) &#43; bY &#43; c= E(X,Y) &#43; a. In other
 words, once you have the values of the edge equations ata given point, the values of the edge equations for adjacent pixels are just afew adds away. Also note that this is absolutely trivial to parallelize: sayyou want to rasterize 8×8 = 64 pixels at once,
 as AMD hardware likes to do (orat least the Xbox 360 does, according to the 3rd edition of Real-timeRendering). Well, you just compute ia &#43; jb for 0 \le i, j \le 7 once for eachtriangle (and edge) and keep that in registers; then, to rasterize a 8×8 blockof
 pixels, you just compute the 3 edge equation for the top-left corner, fireoff 8×8 parallel adds of the constants we’ve just computed, and then test theresulting sign bits to see whether each of the 8×8 pixels is inside or outsidethat edge. Do that for 3 edges,
 and presto, one 8×8 block of a trianglerasterized in a truly embarrassingly parallel fashion, and with nothing morecomplicated than a bunch of integer adders! And by the way, this is why there’ssnapping to a fixed-point grid in the previous part – so we can
 use integermath here. Integer adders are much, much simpler than any floating-point mathunit. And of course we can choose the width of the adders just right to supportthe viewport sizes we want, with sufficient subpixel precision, and probably a2x-4x factor
 on top of that so we get a decently-sized guard band.

By the way, there’s another thorny bit here, which is fill rules; you need tohave tie-breaking rules to ensure that for any pair of triangles sharing anedge, no pixel near that edge will ever be skipped or rasterized twice. D3D andOpenGL both use the so-called
 “top-left” fill rule; the details are explainedin the respective manuals. I won’t talk about it here except to note that withthis kind of integer rasterizer, it boils down to subtracting 1 from theconstant term on some edges during triangle setup. That makes
 it guaranteedwatertight, no fuss at all – compare with the kind of contortions Chris has togo through in his article to make this work properly! Sometimes things justcome together beautifully.

We have a problem though: How do we find out which 8×8 blocks of pixels to testagainst? Pineda mentions two strategies: 1) just scanning over the wholebounding box of the triangle, or 2) a smarter scheme that stops to “turnaround” once it notices that it didn’t
 hit any triangle samples anymore. Well,that’s just fine if you’re testing one pixel at a time. But we’re doing 8×8pixels now! Doing 64 parallel adds only to find out at the very end thatexactly none of them hit any pixels whatsoever is a lot of wasted work.
 So…don’t do that!

**What we need around here is more hierarchy**

What I’ve just described is what the “fine” rasterizer does (the one thatactually outputs sample coverage). Now, to avoid wasted work at the pixellevel, what we do is add another rasterizer in front of it that doesn’trasterize the triangle into pixels, but
 “tiles” – our 8×8 blocks (This paper byMcCormack and McNamara has some details, as does Greene’s “Hierarchical PolygonTiling with Coverage Masks” that takes the idea to its logical conclusion).Rasterizing edge equations into covered tiles works very similarly
 torasterizing pixels; what we do is compute lower and upper bounds for the edgeequations over full tiles; since the edge equations are linear, such extremaoccur on the boundary of the tile – in fact, it’s enough to loop at the 4corner points, and from the
 signs of the ‘a’ and ‘b’ terms in the edgeequation, we can determine which corner. Bottom line, it’s really not much moreexpensive than what we already discussed, and needs exactly the same machinery– a few parallel integer adders. As a bonus, if we evaluate
 the edge equationsat one corner of the tile anyway, we might as well just pass that through tothe fine rasterizer: it needs one reference value per 8×8 block, remember? Verynice.

So what we do now is run a “coarse” rasterizer first that tells us which tilesmight be covered by the triangle. This rasterizer can be made smaller (8×8 atthis level really seems like overkill!), and it doesn’t need to be as fast(because it’s only run for each
 8×8 block). In other words, at this level, thecost of discovering empty blocks is correspondingly lower.

We can think this idea further, as in Greene’s paper or Mike Abrash’sdescription of Rasterization on Larrabee, and do a full hierarchicalrasterizer. But with a hardware rasterizer, there’s little to no point: itactually increases the amount of work done for
 small triangles (unless you canskip levels of the hierarchy, but that’s not how you design HW dataflows!), andif you have a triangle that’s large enough to actually produce significantrasterization work, the architecture I describe should already be fast enoughto
 generate pixel locations faster than the shader units can consume them.

In fact, the actual problem here isn’t big triangles in the first place; theyare easy to deal with efficiently for pretty much any algorithm (certainlyincluding scan-line rasterizers). The problem is small triangles! Even if youhave a bunch of tiny triangles
 that generate 0 or 1 visible pixels, you stillneed to go through triangle setup (that I still haven’t described, but we’regetting close), at least one step of coarse rasterization, and then at leastone fine rasterization step for an 8×8 block. With tiny triangles,
 it’s easy toget either triangle setup or coarse rasterization bound.

One thing to note is that with this kind of algorithm, slivers (long, very thintriangles) are seriously bad news – you need to traverse tons of tiles and onlyget very few covered pixels for each of them. So, well, they’re slow. Avoidthem when you can.

**So what does triangle setup do?**

Well, now that I’ve described what the rasterization algorithm is, we just needto look what per-edge constants we used throughout; that’s exactly what we needto set up during triangle setup.

In our case, the list is this:

&nbsp; &nbsp; The edge equations – a, b, c for all 3 triangle edges.

&nbsp; &nbsp; Some of the derived values, like the ia &#43; jb for 0 \le i, j \le 7that I mentioned; note that you wouldn’t actually store a full 8×8 matrix ofthese in hardware, certainly not if you’re gonna add another value to itanyway. The best way to do this is in HW
 probably to just compute the ia andjb, use a Carry-save adder (aka 3:2 reducer, I wrote about them before) toreduce the ia &#43; jb &#43; c expression to a single sum, and then finish that offwith a regular adder. Or something similar, anyway.

&nbsp; &nbsp; Which reference corner of the tiles to use to get the upper/lowerbounds of the edge equations for coarse rasterizer.

&nbsp; &nbsp; The initial value of the edge equations at the first referencepoint for the coarse rasterizer (adjusted for fill rule).

…so that’s what triangle setup computes. It boils down to several large integermultiplies for the edge equations and their initial values, a few smallermultiplies for the step values, and some cheap combinatorial logic for therest.

**Other rasterization issues and pixel output**

One thing I didn’t mention so far is the scissor rect. That’s just ascreen-aligned rectangle that masks pixels; no pixel outside that rect will begenerated by the rasterizer. This is fairly easy to implement – the coarserasterizer can just reject tiles that
 don’t overlap the scissor rect outright,and the fine rasterizer ANDs all generated coverage masks with the “rasterized”scissor rectangle (where “rasterization” here boils down to a one integercompare per row and column and some bitwise ANDs). Simple stuff,
 moving on.

Another issue is multisample antialiasing. What changes is now you have to testmore samples per pixel – as of DX11, HW needs to support at least 8x MSAA. Notethat the sample locations inside each pixel aren’t on a regular grid (which isbadly behaved for near-horizontal
 or near-vertical edges), but dispersed togive good results across a wide range of multiple edge orientations. Theseirregular sample locations are a total pain to deal with in a scanlinerasterizer (another reason not to use them!) but very easy to support in
 aPineda-style algorithm: it boils down to computing a few more per-edge offsetsin triangle setup and multiple additions/sign tests per pixel instead of justone.

For, say 4x MSAA, you can do two things in an 8×8 rasterizer: you can treateach sample as a distinct “pixel”, which means your effective tile size is now4×4 actual screen pixels after the MSAA resolve and each block of 2×2 locationsin the fine rast grid now
 corresponds to one pixel after resolve, or you canstick with 8×8 actual pixels and just run through it four times. 8×8 seems abit large to me, so I’m assuming that AMD does the former. Other MSAA levelswork analogously.

Anyway, we now have a fine rasterizer that gives us locations of 8×8 blocks plusa coverage mask in each block. Great, but it’s just half of the story – currenthardware also does early Z and hierarchical Z testing (if possible) beforerunning pixel shaders, and
 the Z processing is interwoven with actualrasterization. But for didactic reasons it seemed better to split this up; soin the next part, I’ll be talking about the various types of Z processing, Zcompression, and some more triangle setup – so far we’ve just
 covered setup forrasterization, but there’s also various interpolated quantities we want for Zand pixel shading, and they need to be set up too! Until then.

**Caveats**

I’ve linked to a few rasterization algorithms that I think are representativeof various approaches (they also happen to be all on the Web). There’s a lot more.I didn’t even try to give you a comprehensive introduction into the subjecthere; that would be a (lengthy!)
 serious of posts on its own – and rather dullafter a fashion, I fear.

Another implicit assumption in this article (I’ve stated this multiple times,but this is one of the places to remind you) is that we’re on high-end PChardware; a lot of parts, particularly in the mobile/embedded range, areso-called tile renderers, which partition
 the screen into tiles and render eachof them individually. These are not the same as the 8×8 tiles for rasterizationI used throughout this article. Tiled renderes need at least another“ultra-coarse” rasterization stage that runs early and finds out which of
 the(large) tiles are covered by each triangle; this stage is usually called“binning”. Tiled renderers work differently and have different designparameters than the “sort-last” architectures (that’s the official name) Idescribe here. When I’m done with the
 D3D11 pipeline (and that’s still a waysoff!) I might throw in a post or two on tiled renderers (if there’s interest),but right now I’m just ignoring them, so be advised that e.g. the PowerVR chipsyou so often find in smartphones handle some of this differently.

The 8×8 blocking (other block sizes have the same problem) means that trianglessmaller than a certain size, or with inconvenient aspect ratios, take a lotmore rasterization work than you would think, and get crappy utilization duringthe process. I’d love to
 be able to tell you that there’s a magic algorithmthat’s easy to parallelize and good with slivers and the like, but if there isI don’t know it, and since there’s still regular reminders by the HW vendorsthat slivers are bad, apparently neither do they. So
 for the time being, thisjust seems to be a fact of life with HW rasterization. Maybe someone will comeup with a great solution for this eventually.

The “edge function lower bound” thing I described for coarse rast works fine,but generates false positives in certain cases (false positives in the sensethat it asks for fine rasterization in blocks that don’t actually cover anypixels). There’s tricks to reduce
 this, but again, detecting some of the rarercases is trickier / more expensive than just rasterizing the occasional fine blockthat doesn’t have any pixels lit. Another trade-off.

Finally the blocks used during rasterization are often snapped on a grid (whythat would help will become clearer in the next part). If that’s the case, evena triangle that just covers 2 pixels might straddle 2 tiles and make yourasterize two 8×8 blocks. More
 inefficiency.

The point is this: Yes, all this is fairly simple and elegant, but it’s notperfect, and actual rasterization for actual triangles is nowhere neartheoretical peak rasterization rates (which always assume that all of the fineblocks are completely filled). Keep
 that in mind.

&nbsp;

<span style="color:#999999">本帖最后由</span><span style="color:#999999"> iceworld </span>
<span style="color:#999999">于</span><span style="color:#999999"> 2012-2-23 02:30</span><span style="color:#999999">编辑</span>

A trip through the Graphics Pipeline 2011, part 7

July 8, 2011

In this installment, I’ll be talking about the (early) Z pipeline and how itinteracts with rasterization. Like the last part, the text won’t proceed inactual pipeline order; again, I’ll describe the underlying algorithms first,and then fill in the pipeline
 stages (in reverse order, because that’s theeasiest way to explain it) after the fact.

**Interpolated values**

Z is interpolated across the triangle, as are all the attributes output by thevertex shader. So let me take a minute to explain how that works. At this pointI originally had a section on how the math behind interpolation is derived, andwhy perspective interpolation
 works the way it works. I struggled with that forhours, because I was trying to limit it to maybe one or two paragraphs (sinceit’s an aside), and what I can say now is that if I want to explain itproperly, I need more space than that, and at least one or two
 pictures; apicture may say more than thousand words, but a nice diagram takes me about aslong to prepare as a thousand words of text, so that’s not necessarily a winfrom my perspective :). Anyway, this is something of a tangent anyway, so I’madding it to my
 pile of “graphics-related things to write up properly at somepoint”. For now, I’m giving you the executive summary:

Just linearly interpolating attributes (colors, texture coordinates etc.)across the screen-space triangle does not produce the right results (unless theinterpolation mode is one of the “no perspective” ones, in which case ignorewhat I just wrote). However,
 say we want to interpolate a 2D texture coordinatepair (s,t). It turns out you do get the right results if you linearlyinterpolate \frac{1}{w}, \frac{s}{w} and \frac{t}{w} in screen-space (w here isthe homogeneous clip-space w from the vertex position), then
 per-pixel take thereciprocal of \frac{1}{w} to get w, and finally multiply the other twointerpolated fractions by w to get s and t. The actual linear interpolationboils down to setting up a plane equation and then plugging the screen-spacecoordinates in. And
 if you’re writing a software perspective texture mapper,that’s the end of it. But if you’re interpolating more than two values, abetter approach is to compute (using perspective interpolation) barycentriccoordinates – let’s call them \lambda_0 and \lambda_1
 – for the current pixelin the original clip-space triangle, after which you can interpolate the actualvertex attributes using regular linear interpolation without having to multiplyeverything by w afterwards.

So how much work does that add to triangle setup? Setting up the \frac{\lambda_0}{w}and \frac{\lambda_1}{w} for the triangle requires 4 reciprocals, the trianglearea (which we already computed for back-face culling!), and a fewsubtractions, multiplies and adds.
 Setting up the vertex attributes forinterpolation is really cheap with the barycentric approach – two subtractionsper attribute (if you don’t use barycentric, you get some more multiply-addaction here). Follow me? Probably not, unless you’ve implemented this
 before.Sorry about that – but it’s fairly safe to ignore all this if you don’tunderstand it.

Let’s get back to why we’re here: the one value we want to interpolate rightnow is Z, and because we computed Z as \frac{z}{w} at the vertex level as partof projection (see previous part), so it’s already divided by w and we can justinterpolate it linearly
 in screen space. Nice. What we end up with is a planeequation for Z = aX &#43; bY &#43; c that we can just plug X and Y into to get a value.So, here’s the punchline of my furious hand-waving in the last few paragraphs:Interpolating Z at any given point boils down
 to two multiply-adds. (Startingto see why GPUs have fast multiply-accumulate units? This stuff is absolutelyeverywhere!).

**Early Z/Stencil**

Now, if you believe the place that graphics APIs traditionally put Z/Stencilprocessing into – right before alpha blend, way at the bottom of the pixelpipeline – you might be confused a bit. Why am I even discussing Z at the pointin the pipeline where we are
 right now? We haven’t even started shading pixels!The answer is simple: the Z and stencil tests reject pixels. Potentially themajority of them. You really, really don’t want to completely shade a detailedmesh with complicated materials, to then throw away
 95% of the work you justdid because that mesh happens to be mostly hidden behind a wall. That’s just areally stupid waste of bandwidth, processing power and energy. And in mostcases, it’s completely unnecessary: most shaders don’t do anything that wouldinfluence
 the results of the Z test, or the values written back to theZ/stencil buffers.

So what GPUs actually do when they can is called “early Z” (as opposed to lateZ, which is actually at the late stage in the pipeline that traditional APImodels generally display it at). This does exactly what it sounds like – executethe Z/stencil tests and
 writes early, right after the triangle has beenrasterized, and before we start sending off pixels to the shaders. That way, wenotice all the rejected pixels early, without wasting a lot of computation onthem. However, we can’t always do this: the pixel shader
 may ignore theinterpolated depth value, and instead provide its own depth to be written tothe Z-buffer (e.g. depth sprites); or it might use discard, alpha test, oralpha-to-coverage, all of which “kill” pixels/samples during pixel shaderexecution and mean
 that we can’t update the Z-buffer or stencil buffer earlybecause we might be updating depth values for samples that later get discardedin the shader!

So GPUs actually have two copies of the Z/stencil logic; one right after therasterizer and in front of the pixel shader (which does early Z) and one afterthe shader (which does late Z). Note that we can still, in principle, do thedepth testing in the early-Z
 stage even if the shader uses some of thesample-killing mechanism. It’s only writes that we have to be careful with. Theonly case that really precludes us from doing any early Z-testing at all iswhen we write the output depth in the pixel shader – in that
 case the early Zunit simply has nothing to work with.

Traditionally, APIs just pretended none of this early-out logic existed;Z/Stencil was in a late stage in the original API model, and any optimizationssuch as early-Z had to be done in a way that was 100% functionally consistentwith that model; i.e. drivers
 had to detect when early-Z was applicable, andcould only turn it on when there were no observable differences. By now APIshave closed that gap; as of DX11, shaders can be declared as “force early-Z”,which means they run with full early-Z processing even when
 the shader usesprimitives that aren’t necessarily “safe” for early-Z, and shaders that writedepth can declare that the interpolated Z value is conservative (i.e. early Zreject can still happen).

**Z/stencil writes: the full truth**

Okay, wait. As I’ve described it, we now have two parts in the pipeline – earlyZ and late Z – that can both write to the Z/stencil buffers. For any givenshader/render state combination that we look at, this will work – in the steadystate. But that’s not how
 it works in practice. What actually happens is thatwe render a few hundred to a few thousand batches per frame, switching shadersand render state regularly. Most of these shaders will allow early Z, but somewon’t. Switching from a shader that does early Z
 to one that does late Z is noproblem. But going back from late Z to early Z is, if early Z does any writes:early Z is, well, earlier in the pipeline than early Z – that’s the wholepoint! So we may start early-Z processing for one shader, merrily writing to
 thedepth buffer while there’s still stuff down in the pipeline for our old shaderthat’s running late-Z and may be trying to write the same location at the sametime – classic race condition. So how do we fix this? There’s a bunch ofoptions:

&nbsp; &nbsp; Once you go from early-Z to late-Z processing within a frame (orat least a sequence of operations for the same render target), you stay atlate-Z until the next point where you flush the pipeline anyway. This works butpotentially wastes lots of shader cycles
 while early-Z is unnecessarily off.

&nbsp; &nbsp; Trigger a (pixel) pipeline flush when going from a late-Z shaderto an early-Z shader – also works, also not exactly subtle. This time, we don’twaste shader cycles (or memory bandwidth) but stall instead – not much of animprovement.

&nbsp; &nbsp; But in practice, having Z-writes in two places is just bad news.Another option is to not ever write Z in the early-Z phase; always do theZ-writes in late-Z. Note that you need to be careful to make conservativeZ-testing decisions during early Z if you do
 this! This avoids the racecondition but means the early Z-test results may be stale because the Z-writefor the currently-dispatched pixels won’t happen until a while later.

&nbsp; &nbsp; Use a separate unit that keeps track of Z-writes for us andenforces the correct ordering; both early-Z and late-Z must go through thisunit.

All of these methods work, and all have their own advantages and drawbacks.Again I’m not sure what current hardware does in these cases, but I have strongreason to believe that it’s one of the last two options. In particular, we’llmeet a functional unit later
 down the road (and the pipeline) that would be agood place to implement the last option.

But we’re still doing all this testing per pixel. Can’t we do better?

**Hierarchical Z/Stencil**

The idea here is that we can use our tile trick from rasterization again, andtry to Z-reject whole tiles at a time, before we even descend down to the pixellevel! What we do here is a strictly conservative test; it may tell us that“there might be pixels that
 pass the Z/stencil-test in this tile” when thereare none, but it will never claim that all pixels are rejected when in factthey weren’t.

Assume here that we’re using “less”, “less-equal”, or “equal” as Z-comparemode. Then we need to store the maximum Z-value we’ve written for that tile,per tile. When rasterizing a triangle, we calculate the minimum Z-value theactive triangle is going to write
 to the current tile (one easy conservativeapproximation is to take the min of the interpolated Z-values at the fourcorners of the current tile). If our triangle minimum-Z is larger than thestored maximum-Z for the current tile, the triangle is guaranteed to
 becompletely occluded. That means we now need to track maximum-Z per-tile, andkeep that value up to date as we write new pixels – though again, it’s fine ifthat information isn’t completely up to date; since our Z-test is of the “less”variety, values in the
 Z buffer will only get smaller over time. If we use aper-tile maximum-Z that’s a bit out of date, it just means we’ll get slightlyworse early rejection rates than we could; it doesn’t cause any other problems.

The same thing works (with min/max and compare directions swapped) if we’reusing one of the “greater”, “greater-equal” or “equal” Z-tests. What we can’teasily do is change from one of the “less”-based tests to a “greater”-basedtests in the middle of the frame,
 because that would make the information we’vebeen tracking useless (for less-based tests we need maximum-Z per tile, forgreater-based tests we need minimum-Z per tile). We’d need to loop over thewhole depth buffer to recompute min/max for all tiles, but what
 GPUs actuallydo is turn hierarchical-Z off once you do this (up until the next Clear). So:don’t do that.

Similar to the hierarchical-Z logic I’ve described, current GPUs also havehierarchical stencil processing. However, unlike hierarchical-Z, I haven’t seenmuch in the way of published literature on the subject (meaning, I haven’t runinto it – there might be papers
 on it, but I’m not aware of them); as a gameconsole developer you get access to low-level GPU docs which include adescription of the underlying algorithms, but frankly, I’m definitely notcomfortable writing about something here where really the only good sources
 Ihave are various GPU docs that came with a thick stack of NDAs. Instead I’lljust nebulously note that there’s magic pixie dust that can do certain kinds ofstencil testing very efficiently under controlled circumstances, and leave youto ponder what that might
 be and how it might work, in the unlikely case thatyou deeply care about this – presumably because your father was killed by ahierarchical stencil unit and you’re now collecting information on its weakpoints for your revenge, or something like that.

**Putting it all together**

Okay, we now have all the algorithms and theory we need – let’s see how we cantake our new set of toys and wire it up with what we already have!

First off, we now need to do some extra triangle setup for Z/attributeinterpolation. Not much to be done about it – more work for triangle setup;that’s how it goes. After that’s coarse rasterization, which I’ve discussed inthe previous part.

Then there’s hierarchical Z (I’m assuming less-style comparisons here). We wantto run this between coarse and fine rasterization. First, we need the logic tocompute the minimum Z estimates for each tile. We also need to store theper-tile maximum Zs, which don’t
 need to be exact: we can shave bits as long aswe always round up! As usual, there’s a trade-off here between space used andearly-rejection efficiency. In theory, you could put the Z-max info intoregular memory. In practice, I don’t think anyone does this,
 because you wantto make the hierarchical-Z decision without a ton of extra latency. The otheroption is to put dedicated memory for hierarchical Z onto the chip – usually asSRAM, the kind of memory you also make caches out of. For 24-bit Z, youprobably need
 something like 10-14 bits per tile to store a reasonable-accuracyZ-max in a compact encoding. Assuming 8×8 tiles, that means less than 1MBit(128k) of SRAM to support resolutions up to 2048×2048 – sounds like a plausibleorder of magnitude to me. Note that these
 things are fixed size and shared forthe whole chip; if you do a context switch, you lose. If you allocate the wrongdepth buffers to this memory, you can’t use hierarchical Z on the depth buffersthat actually matter, and you lose. That’s just how it goes. This
 kind ofthings is why hardware vendors regularly tell you to create your most importantrender targets and depth buffers first; they have a limited supply of this typeof memory (there’s more like it, as you’ll see), and when it runs out, you’reout of luck. Note
 they don’t necessarily need to do this all-or-nothing; forexample, if you have a really large depth buffer, you might only gethierarchical Z in the top left 2048×1536 pixels, because that’s how much fitsinto the Z-max memory. It’s not ideal, but still much
 better than disablinghierarchical-Z outright.

And by the way, “Real-Time Rendering” mentions at this point that “it is likelythat GPUs are using hierarchical Z-buffers with more than two levels”. I doubtthis is true, for the same reason that I doubt they use a multilevelhierarchical rasterizer: adding
 more levels makes the easy cases (largetriangles) even faster while adding latency and useless work for smalltriangles: if you’re drawing a triangle that fits inside a single 8×8 tile, anycoarser hierarchy level is pure overhead, because even at the 8×8 level,
 you’djust do one test to trivial-reject the triangle (or not). And again, forhardware, it’s not that big a performance issue; as long as you’re notconsuming extra bandwidth or other scarce resources, doing more compute workthan strictly necessary isn’t a big
 problem, as long as it’s within reasonablelimits,

Hierarchical stencil is also there and should also happen prior to fine rast,most likely in parallel with hierarchical Z. We’ve established that this runson air, love and magic pixie dust, so it doesn’t need any actual hardware andis probably always exactly
 right in its predictions. Ahem. Moving on.

After that is fine rasterization, followed in turn by early Z. And for early Z,there’s two more important points I need to make.

**Revenge of the API order**

For the past few parts, I’ve been playing fast and loose with the order thatprimitives are submitted in. So far, it didn’t matter; not for vertex shading,nor primitive assembly, triangle setup or rasterization. But Z is different.For Z-compare modes like “less”
 or “lessequal”, it’s very important what orderthe pixels arrive in; if we mess with that, we risk changing the results andintroducing nondeterministic behavior. More importantly, as per the spec, we’refree to execute operations in any order so long as it isn’t
 visible to the app;well, as I just said, for Z processing, order is important, so we need to makesure that triangles arrive at Z processing in the right order (this goes forboth early and late Z).

What we do in cases like this is go back in the pipeline and look for areasonable spot to sort things into order again. In our current path, the bestcandidate location seems to be primitive assembly; so when we start assemblingprimitives from shaded vertex
 blocks, we make sure to assemble them strictly inthe original order as submitted by the app to the API. This means we mightstall a bit more (if the PA buffer holds an output vertex block, but it’s notthe correct one, we need to wait and can’t start setting
 up primitives yet),but that’s the price of correctness.

**Memory bandwidth and Z compression**

The second big point is that Z/Stencil is a serious bandwidth hog. This has acouple of reasons. For one, this is the one thing we really run for all samplesgenerated by the rasterizer (assuming Z/Stencil isn’t off, of course). Shaders,blending etc. all benefit
 from the early rejection we do; but even Z-rejectedpixels do a Z-buffer read first (unless they were killed by hierarchical Z). That’sjust how it works. The other big reason is that, when multisampling is enabled,the Z/stencil buffer is per sample; so 4x MSAA
 means 4x the memory bandwidthcost of Z? For something that takes a substantial amount of memory bandwidtheven at no MSAA, that’s seriously bad news.

So what GPUs do is Z compression. There’s various approaches, but the generalidea is always the same: assuming reasonably-sized triangles, we expect a lotof tiles to just contain one or maybe two triangles. If that happens, theninstead of storing Z-values for
 the whole tile, we just store the planeequation of the triangle that filled up this tile. That plane equation is(hopefully) smaller than the actual Z data. Without MSAA, one tile covers 8×8actual pixels, so triangles need to be relatively big to cover a full
 tile; butwith 4x MSAA, a tile effectively shrinks to 4×4 pixels, and covering full tilesgets easier. There’s also extensions that can support 2 triangles etc., but forreasonably-sized tiles, you can’t go much larger than 2-3 tris and stillactually save bandwidth:
 the extra plane equations and coverage masks aren’tfree!

Anyway, point is: this compression, when it works, is fully lossless, but it’snot applicable to all tiles. So we need some extra space to denote whether atile is compressed or not. We could store this in regular memory, but thatwould mean we now need to wait
 two full memory round-trips latencies to do aZ-read. That’s bad. So again, we add some dedicated SRAM that allows us tostore a few (1-3) bits per tile. At its simplest, it’s just a “compressed” or“not compressed” flag, but you can get fancy and add multiple
 compression modesand such. A nice side effect of Z-compression is that it allows us to do fastZ-clears: e.g. when clearing to Z=1, we just set all tiles to “compressed” andstore the plane equation for a constant Z=1 triangle.

All of the Z-compression thing, much like texture compression in the texturesamplers, can be folded into memory access/caching logic, and made completelytransparent to everyone else. If you don’t want to send the plane equations (oradd the interpolator logic)
 to the Z memory access block, it can just inferthem from the Z data and use some integer delta-coding scheme. This kind ofapproach usually needs extra bits per sample to actually allow losslessreconstruction, but it can lead to simpler data paths and nicer
 interfacebetween units, which hardware guys love.

And that’s it for today! Next up: Pixel shading and what happens around it.

**Postscript**

As I said earlier, the topic of setting up interpolated attributes wouldactually make for a nice article on its own. I’m skipping that for now – mightdecide to fill this gap later, who knows.

Z processing has been in the 3D pipeline for ages, and a serious bandwidthissue for most of the time; people have thought long and hard about thisproblem, and there’s a zillion tricks that go into doing “production-quality”Z-buffering for GPUs, some big, some
 small. Again, I’m just scratching thesurface here; I tried to limit myself to the bits that are useful to know for agraphics programmer. That’s why I don’t spend much time on the details ofhierarchical Z computations or Z compression and the like; all of this
 is veryspecific on hardware details that change slightly in every generation, andultimately, mostly there’s just no practical way you get to exploit any of thisusefully: If a given Z-compression scheme works well for your scene, that’ssome memory bandwidth
 you can spend on other things. If not, what are you gonnado? Change your geometry and camera position so that Z-compression is moreefficient? Not very likely. To a hardware designer, these are all algorithms tobe improved on in every generation, but to a programmer,
 they’re just facts oflife to deal with.

This time, I’m not going into much detail on how memory accesses work in thisstage of the pipeline. That’s intentional. There’s a key to high-throughputpixel shading and other per-pixel or per-sample processing, but it’s later inthe pipeline, and we’re not
 there yet. Everything will be revealed in due time:)

&nbsp;

&nbsp;

A trip through the GraphicsPipeline 2011, part 8

July 10, 2011

In this part, I’ll be dealing with the first half of pixel processing: dispatchand actual pixel shading. In fact, this is really what most graphics programmerthink about when talking about pixel processing; the alpha blend and late Zstages we’ll encounter in
 the next part seem like little more than anafterthought. In hardware, the story is a bit more complicated, as we’ll see –there’s a reason I’m splitting pixel processing into two parts. But I’m gettingahead of myself. At the point where we’re entering this
 stage, the coordinatesof pixels (or, actually, quads) to shade, plus associated coverage masks,arrive from the rasterizer/early-Z unit – with triangle in the exact same orderas submitted by the application, as I pointed out last time. What we need to dohere
 is to take that linear, sequential stream of work and farm it out tohundreds of shader units, then once the results are back, we need to merge itback into one linear stream of memory updates.

That’s a textbook example of fork/join-parallelism. This part deals with thefork phase, where we go wide; the next part will explain the join phase, wherewe merge the hundreds of streams back into one. But first, I have a few morewords to say about rasterization,
 because what I just told you about therebeing just one stream of quads coming in isn’t quite true.

**Going wide during rasterization**

To my defense, what I told you used to be true for quite a long time, but it’sa serial part of the pipeline, and once you throw in excess of 300 shader unitsat a problem, serial parts of the pipeline have the tendency to becomebottlenecks. So GPU architects
 started using multiple rasterizers; as of 2010,NVidia employs four rasterizers and AMD uses two. As a side note, the NVpresentation also has a few notes on the requirement to keep stuff in APIorder. In particular, you really do need to sort primitives back
 into orderprior to rasterization/early-Z, like I mentioned last time; doing it justbefore alpha blend (as you might be inclined to do) doesn’t work.

The work distribution between rasterizers is based on the tiles we’ve alreadyseen for early-Z and coarse rasterization. The frame buffer is divided intotile-sized regions, and each region is assigned to one of the rasterizers.After setup, the bounding box of
 the triangle is consulted to find out whichtriangles to hand over to which rasterizers; large triangles will always besent to all rasterizers, but smaller ones can hit as little as one tile andwill only be sent to the rasterizer that owns it.

The beauty of this scheme is that it only requires changes to the workdistribution and the coarse rasterizers (which traverse tiles); everything thatonly sees individual tiles or quads (that is, the pipeline from hierarchical Zdown) doesn’t need to be modified.
 The problem is that you’re now dividing jobsbased on screen locations; this can lead to a severe load imbalance between therasterizers (think a few hundred tiny triangles all inside a single tile) thatyou can’t really do anything about. But the nice thing
 is that everything thatadds ordering constraints to the pipeline (Z-test/write order, blend order)comes attached to specific frame-buffer locations, so screen-space subdivisionworks without breaking API order – if this wasn’t the case, tiled rendererswouldn’t
 work.

**You need to go wider!**

Okay, so we don’t get just one linear stream of quad coordinates plus coveragemasks in, but between two and four. We still need to farm them out to hundredsof shader units. It’s time for another dispatch unit! Which first means anotherbuffer. But how big are
 the batches we send off to the shaders? Here I go withNVidia figures again, simply because they mention this number in public whitepapers; AMD probably also states that information somewhere, but I’m notfamiliar with their terminology for it so I couldn’t
 do a direct search for it.Anyway, for NVidia, the unit of dispatch to shader units is 32 threads, whichthey call a “Warp”. Each quad has 4 pixels (each of which in turn can behandled as one thread), so for each shading batch we issue, we need to grab 8incoming
 quads from the rasterizer before we can send off a batch to the shaderunits (we might send less in case there’s a shader switch or pipeline flush).

Also, this is a good point to explain why we’re dealing with quads of 2×2pixels and not individual pixels. The big reason is derivatives. Texturesamplers depend on screen-space derivatives of texture coordinates to do theirmip-map selection and filtering (as
 we saw back in part 4); and, as of shadermodel 3.0 and later, the same machinery is directly available to pixel shadersin the form of derivative instructions. In a quad, each pixel has both ahorizontal and vertical neighbor within the same quad; this can be
 used toestimate the derivatives of parameters in the x and y directions using finitedifferencing (it boils down to a few subtractions). This gives you a very cheapway to get derivatives at the cost of always having to shade groups of 2×2pixels at once. This
 is no problem in the interior of large triangles, butmeans that between 25-75% of the shading work for quads generated for triangleedges is wasted. That’s because all pixels in a quad, even the masked ones, getshaded. This is necessary to produce correct derivatives
 for the pixels in thequad that are visible. The invisible but still-shaded pixels are called “helperpixels”. Here’s an illustration for a small triangle:

The triangle intersects 4 quads, but only generates visible pixels in 3 ofthem. Furthermore, in each of the 3 quads, only one pixel is actually covered(the sampling points for each pixel region are depicted as black circles) – thepixels that are filled are
 depicted in red. The remaining pixels in eachpartially-covered quad are helper pixels, and drawn with a lighter color. Thisillustration should make it clear that for small triangles, a large fraction ofthe total number of pixels shaded are helper pixels, which
 has attracted someresearch attention on how to merge quads of adjacent triangles. However, whileclever, such optimizations are not permissible by current API rules, andcurrent hardware doesn’t do them. Of course, if the HW vendors at some pointdecide that
 wasted shading work on quads is a significant enough problem toforce the issue, this will likely change.

**Attribute interpolation**

Another unique feature of pixel shaders is attribute interpolation – all other shadertypes, both the ones we’ve seen so far (VS) and the ones we’re still to talkabout (GS, HS, DS, CS) get inputs directly from a preceding shader stage ormemory, but pixel shaders
 have an additional interpolation step in front ofthem. I’ve already talked a bit about this in the previous part when discussingZ, which was the first interpolated attribute we saw.

Other interpolated attributes work much the same way; a plane equation for themis computed during triangle setup (GPUs may choose to defer this computationsomewhat, e.g. until it’s known that at least one tile of the triangle passedthe hierarchical Z-test,
 but that shall not concern us here), and then duringpixel shading, there’s a separate unit that performs attribute interpolationusing the pixel positions of the quads and the plane equations we justcomputed.

Update: Marco Salvi points out (in the comments below) that while there used tobe dedicated interpolators, by now the trend is towards just having them returnthe barycentric coordinates to plug into the plane equations. The actualevaluation (two multiply-adds
 per attribute) can be done in the shader unit.

All of this shouldn’t be surprising, but there’s a few extra interpolationtypes to discuss. First, there’s “constant” interpolators, which are(surprise!) constant across the primitive and take the value for each vertexattribute from the “leading vertex” (which
 vertex that is is determined duringprimitive setup). Hardware may either have a fast-path for this or just set upa corresponding plane equation; either way works fine.

Then there’s no-perspective interpolation. This will usually set up the planeequations differently; the plane equations for perspective-correctinterpolation are set up either for X, Y-based interpolation by dividing theattribute values at each vertex by the
 corresponding w, or for barycentricinterpolation by building the triangle edge vectors. Non-perspectiveinterpolated attributes, however, are cheapest to evaluate when their planeequation is set up for X, Y-based interpolation without dividing the values ateach
 vertex by the corresponding w.

**“Centroid” interpolation is tricky**

Next, we have “centroid” interpolation. This is a flag, not a separate mode; itcan be combined both with the perspective and no-perspective modes (but notwith constant interpolation, because it would be pointless). It’s also terriblynamed and a no-op unless
 multisampling is enabled. With multisampling ob, it’sa somewhat hacky solution to a real problem. The issue is that withmultisampling, we’re evaluating triangle coverage at multiple sample points inthe rasterizer, but we’re only doing the actual shading once
 per pixel.Attributes such as texture coordinates will be interpolated at the pixel centerposition, as if the whole pixel was covered by the primitive. This can lead toproblems in situations such as this:

MSAA sample problem

Here, we have a pixel that’s partially covered by a primitive; the four smallcircles depict the 4 sampling points (this is the default 4x MSAA pattern)while the big circle in the middle depicts the pixel center. Note that the bigcircle is outside the primitive,
 and any “interpolated” value for it willactually be linear extrapolation; this is a problem if the app uses textureatlases, for example. Depending on the triangle size, the value at the pixelcenter can be very far off indeed. Centroid sampling solves this
 problem. Theoriginal explanation was that the GPU takes all of the samples covered by theprimitive, computes their centroid, and samples at that position (hence thename). This is usually followed by the addition that this is just a conceptualmodel, and GPUs
 are free to do it differently, so long as the point they pickfor sampling is within the primitive.

If you think it somewhat unlikely that the hardware actually counts the coveredsamples, sums them up, then divides by the count, then join the club. Here’swhat actually happens:

&nbsp; &nbsp; If all sample points cover the primitive, interpolation is doneas usual, i.e. at the pixel center (which happens to be the centroid of allsample positions for all reasonable sampling patterns).

&nbsp; &nbsp; If not all sample points cover the triangle, the hardware picksone of the sample points that do and evaluates there. All covered sample pointsare (by definition) inside the primitive so this works.

That picking used to be arbitrary (i.e. left to the hardware); I believe by nowDX11 actually prescribes exactly how it’s done, but this more a matter ofgetting consistent results between different pieces of hardware than it issomething that API users will actually
 care about. As said, it’s a bit hacky.It also tends to mess up derivative calculations for quads that have partiallycovered pixels – tough luck. What can I say, it may be industrial-strength ducttape, but it’s still duct tape.

Finally (new in DX11!) there’s “pull-model” attribute interpolation. Regularattribute interpolation is done automatically before the pixel shader starts;pull-model interpolation adds actual instructions that do the interpolation tothe pixel shader. This allows
 the shader to compute its own position to samplevalues at, or to only interpolate attributes in some branches but not inothers. What it boils down to is the pixel shader being able to send additionalrequests to the interpolation unit while the shader is running.

**The actual shader body**

Again, the general shader principles are well-explained in the APIdocumentation, so I’m not going to talk about how individual instructions work;generally, the answer is “as you would expect them to”. There are however someinteresting bits about pixel shader
 execution that are worth talking about.

The first one is: texture sampling! Wait, didn’t I wax on about texturesamplers for quite some time in part 4 already? Yes, but that was the texturesampler side of things – and if you remember, there was that one bit abouttexture cache misses being so frequent
 that samplers are usually designed tosustain at least one miss to main memory per request (which is 16-32 pixels, remember!)without stalling. That’s a lot of cycles – hundreds of them. And it would be atremendous waste of perfectly good ALUs to keep them idle
 while all this isgoing on.

So what shader units actually do is switch to a different batch after they’veissued a texture sample; then when that batch issues a texture sample (orcompletes), it switches back to one of the previous batches and checks if thetexture samples are there yet.
 As long as each shader unit has a few batches itcan work on at any given time, this makes good use of available resources. Itdoes increase latency for completion of individual batches though – again, alatency-vs-throughput trade-off. By now you should know
 which side wins onGPUs: Throughput! Always. One thing to note here is that keeping multiplebatches (or “Warps” on NVidia hardware, or “Wavefronts” for AMD) running at thesame time requires more registers. If a shader needs a lot of registers, ashader unit
 can keep less warps around; and if there are less of them, the chancethat at some point you’ll run out of runnable batches that aren’t waiting ontexture results is higher. If there’s no runnable batches, you’re out of luckand have to stall until one of them
 gets its results back. That’s unfortunate,but there’s limited hardware resources for this kind of thing – if you’re outof memory, you’re out of memory, period.

Another point I haven’t talked about yet: Dynamic branches in shaders (i.e.loops and conditionals). In shader units, work on all elements of each batch usuallyproceeds in lockstep. All “threads” run the same code, at the same time. Thatmeans that ifs are a
 bit tricky: If any of the threads want to execute the“then”-branch of an if, all of them have to – even though most of them may endup ignoring the results using a technique called predication, because theydidn’t want to descend down there in the first place..
 Similarly for the “else”branch. This works great if conditionals tend to be coherent across elements,and not so great if they’re more or less random. Worst case, you’ll alwaysexecute both branches of every if. Ouch. Loops work similarly – as long as atleast
 one thread wants to keep running a loop, all of the threads in thatbatch/Warp/Wavefront will.

Another pixel shader specific is the discard instruction. A pixel shader candecide to “kill” the current pixel, which means it won’t get written. Again, ifall pixels inside a batch get discarded, the shader unit can stop and go toanother batch; but if there’s
 at least one thread left standing, the rest willbe dragged along. DX11 adds more fine-grained control here by way of writingthe output pixel coverage from the pixel shader (this is always ANDed with theoriginal triangle/Z-test coverage, to make sure that a
 shader can’t writeoutside its primitive, for sanity). This allows the shader to discardindividual samples instead of whole pixels; it can be used to implementAlpha-to-Coverage with a custom dithering algorithm in the shader, for example.

Pixel shaders can also write the output depth (this feature has been around forquite some time now). In my experience, this is an excellent way to shoot downearly-Z, hierarchical Z and Z compression and in general get the slowest pathpossible. By now, you know
 enough about how these things work to see why. :)

Pixel shaders produce several outputs – in general, one 4-component vector foreach render target, of which there can be (currently) up to 8\. The shader thensends the results on down the pipeline towards what D3D calls the “OutputMerger”. This’ll be our topic
 next time.

But before I sign off, there’s one final thing that pixel shaders can dostarting with D3D11: they can write to Unordered Access Views (UAVs) –something which only compute and pixel shaders can do. Generally speaking, UAVstake the place of render targets during
 compute shader execution; but unlikerender targets, the shader can determine the position to write to itself, andthere’s no implicit API order guarantee (hence the “unordered access” part ofthe name). For now, I’ll only mention that this functionality exists
 – I’lltalk more about it when I get to Compute Shaders.

Update: In the comments, Steve gave me a heads-up about the correct AMDterminology (the first version of the post didn’t have the “Wavefronts” namebecause I couldn’t remember it) and also posted a link to this greatpresentation by Kayvon Fatahalian that explains
 shader execution on GPUs, witha lot more pretty pictures that I can be bothered to make :). You should reallycheck it out if you’re interested in how shader cores work.

And… that’s it! No big list of caveats this time. If there’s something missinghere, it’s because I’ve genuinely forgotten about it, not because I decided itwas too arcane or specific to write up here. Feel free to point out omissionsin the comments and I’ll
 see what I can do.

&nbsp;

&nbsp;

A trip through the GraphicsPipeline 2011, part 9

July 12, 2011

Welcome back! This post deals with the second half of pixel processing, the“join phase”. The previous phase was all about taking a small number of inputstreams and turning them into lots of independent tasks for the shader units.Now we need to fold that large
 number of independent computations back into one(correctly ordered) stream of memory operations. As I already did in the posts onrasterization and early Z, I’ll first give a quick description of what needs tobe done on a general level, and then I’ll go into
 how this is mapped tohardware.

**Merging pixels again: blend and late Z**

At the bottom of the pipeline (in what D3D calls the “Output Merger” stage), wehave late Z/stencil processing and blending. These two operations are bothrelatively simple computationally, and they both update the render target(s) /depth buffer respectively.
 “Update” operation here means they’re of the read-modify-writevariety. Because all of this happens for every quad that makes it this farthrough the pipeline, it’s also bandwidth-intensive. Finally, it’sorder-sensitive (both blending and Z processing need to
 happen in API order),so we need to make sure to sort processed quads into order first.

I’ve already explained Z-processing, and blending is one of these things thatwork pretty much as you’d expect; it’s a fixed-function block that performs amultiply, a multiply-add and maybe some subtractions first, per render target.This block is kept deliberately
 simple; it’s separate from the shader units soit needs its own ALU, and we’d really prefer for it to be as small as possible:we want to spend our chip area (and power budget) on ALUs in the shader units,where they benefit every code that runs on the GPU, not
 on a fixed-functionunit that’s only used at the end of the pixel pipeline. Also, we need it tohave a short, predictable latency: this part of the pipeline needs to processdata in-order to be correct. This limits our options as far as tradingthroughput for
 latency is concerned; we can still process quads that don’toverlap in parallel, but if we e.g. draw lots of small triangles, we’ll havemultiple quads coming in for every screen location, and we’d better be able towrite them out as quickly as they come, or
 else all our massively parallelpixel processing was for nought.

**Meet the ROPs**

ROPs are the hardware units that handle this part of the pipeline (as you cantell by the plural, there’s more than one). The acronym, depending on who youasks, stands for “Render OutPut unit”, “Raster Operations Pipeline”, or “RasterOperations Processor”. The
 actual name is fairly archaic – it derives from thedays of pure 2D hardware acceleration, with hardware whose main purpose was todo fast Bit blits. The classic 2D ROP design has three inputs – the current(destination) pixel value in the frame buffer, the source
 data, and a maskinput – then computes some function of the 3 values and writes the results backto the frame buffer. Note this is before true color displays: the image datawas usually in bit plane format and the function was some binary logicfunction. Then
 at some point bit planes died out (in favor of “chunky”representations that keep the bits for a pixel together), true color became thenorm, the on-off mask was replaced with an alpha channel and the bitwiseoperations with blends, but the name stuck. So even
 now in 2011, when about thelast remnant of that original architecture is the “logic op” in OpenGL, westill call them ROPs.

So what do we need to do, in hardware, for blend/late Z? A simple plan:

&nbsp; &nbsp; Read original render target/depth buffer contents from memory –memory access, long latency. Might also involve depth buffer and render targetdecompression! (I’ll explain render target compression later)

&nbsp; &nbsp; Sort incoming shaded quads into the right (API) order. This takessome buffering so we don’t immediately stall when quads don’t finish in theright order (think loops/branches, discard, and variable texture fetchlatency). Note we only need to sort based on
 primitive ID here – two quads fromthe same primitive can never overlap, and if they don’t overlap they don’t needto be sorted!

&nbsp; &nbsp; Perform the actual blend/late Z/stencil operation. This is math –maybe a few dozen cycles worth, even with deeply pipelined units.

&nbsp; &nbsp; Write the results back to memory again, compressing etc. alongthe way – long latency again, though this time we’re not waiting for results soit’s less of a problem at this end.

So, build the late-Z/blending unit, add some compression logic, wire it up tomemory on one side and do some buffering of shaded quads on the other side andwe’re done, right?

Well, in theory anyway.

Except we need to cover the long latencies somehow. And all this happens forevery single pixel (well, quad, actually). So we need to worry about memorybandwidth too… memory bandwidth? Wasn’t there something about memory bandwidth?Watch closely now as I pull
 a bunny out of a hat after I put it there way backin part 2 (uh oh, that was more than a week ago – hope that critter is still OKin there…).

**Memory bandwidth redux: DRAM pages**

In part 2, I described the 2D layout of DRAM, and how it’s faster to staywithin a single row because changing the active row takes time – so for idealbandwidth you want to stay in the same row between accesses. Well, the thingis, single DRAM rows are kinda
 large. Individual DRAM chips go up into theGigabit range in size these days, and while they’re not necessarily square (infact a 2:1 aspect ratio seems to be preferred), you can still do a roughcalculation of how many rows and columns there would be; for 512
 Megabit(=64MB), we’d expect something like 16384×32768, i.e. a single row is about 32kbits or 4k bytes (or maybe 2k, or 8k, but somewhere in that ballpark – you getthe idea). That’s a rather inconvenient size to be making memory transactionsin.

Hence, a compromise: the page. A DRAM page is some more conveniently sizedslice of a row (by now, usually 256 or 512 bits) that’s commonly transferred ina single burst. Let’s take 512 bits (64 bytes) for now. At 32 bits per pixel –the standard for depth buffers
 and still fairly common for render targetsalthough rendering workloads are definitely shifting towards 64 bit/pixelformats – that’s enough memory to fit data for 16 pixels in. Hey, that’s funny– we’re usually shading pixels in groups of 16 to 64! (NV is a
 bit closer tothe smaller end, AMD favors the larger counts). In fact, the 8×8 tile size I’vebeen quoting in the rasterizer / early Z parts comes from AMD; I wouldn’t besurprised if NV did coarse traversal (and hierarchical Z, which they dub“Z-cull”) on 4×4
 tiles, though a quick web search turned up nothing to eitherconfirm this or rule it out. Either way, the plot thickens. Could it be thatwe’re trying to traverse pixels in an order that gives good DRAM pagecoherency? You bet we are. Note that this has implications
 for internal rendertarget layout too: we want to make sure pixels are stored such that a singleDRAM page actually has a useful shape; for shading purposes, a 4×4 or 8×2 pixelDRAM page is a lot more useful than a 16×1 pixel one (remember – quads). Whichis why
 render targets usually don’t have a fully linear layout in memory.

That gives us yet another reason to shade pixels in groups, and also yetanother reason to do a two-level traversal. But can we milk this some more? Youbet we can: we still have the memory latency to cover. Usual disclaimer: Thisis one of the places where I
 don’t have detailed information on what GPUsactually do, so what I’m describing here is a guess, not a fact. Anyway, assoon as we’ve rasterized a tile, we know whether it generates any pixels ornot. At that point, we can select a ROP to handle our quads for
 that tile, andqueue a command to fetch the associated frame buffer data into a buffer. By thepoint we get shaded quads back from the shader units, that data should bethere, and we can start blending without delay (of course, if blending is offor identity,
 we can skip this load altogether). Similarly for Z data – if werun early Z before the pixel shader, we might need to allocate a ROP and fetchdepth/stencil data earlier, maybe as soon as a tile has passes the coarse Ztest. If we run late Z, we can just prefetch
 the depth buffer data at the sametime we grab the framebuffer pixels (unless Z is off completely, that is).

All of this is early enough to avoid latency stalls for all but the fastestpixel shaders (which are usually memory bandwidth-bound anyway). There’s alsothe issue of pixel shaders that output to multiple render targets, but thatdepends on how exactly that feature
 is implemented. You could run the shadermultiple times (not efficient but easiest if you have fixed-size output buffers),or you could run all the render targets through the same ROP (but up to 8rendertargets with up to 128 bits/pixels – that’s a lot of buffer
 space we’retalking), or you could allocate one ROP per output render target.

An of course, if we have these buffers in the ROPs anyway, we might as welltreat them as a small cache (i.e. keep them around for a while). This wouldhelp if you’re drawing lots of small triangles – as long as they’re spatiallylocalized, anyway. Again, I’m
 not sure if GPUs actually do this, but it seemslike a reasonable thing to do (you’d probably want to flush these bufferssomething like once per batch or so though, to avoid thesynchronization/coherency issues that full write-back caches bring).

Okay, that explains the memory side of things, and the computational part we’vealready covered. Next up: Compression!

**Depth buffer and color buffer compression**

I already explained the basic workings of this in part 7 while talking about Z;in fact, I don’t have much to add about depth buffer compression here. But allthe bandwidth issues I mentioned there exist for color values too; it’s not sobad for regular rendering
 (unless the Pixel Shaders output pixels fast enoughto hit memory bandwidth limits), but it is a serious issue for MSAA, where wesuddenly store somewhere between 2 and 8 samples per pixel. Like Z, we wantsome lossless compression scheme to save bandwidth in
 common cases. Unlike Z,plane equations per tile are not a good fit to textured pixel data.

However, that’s no problem, because actually, MSAA pixel data is even easier tooptimize for: Remember that pixel shaders only run once per pixel, not persample – unless you’re using sample-frequency shading anyway, but that’s aD3D11 feature and not commonly
 used (yet?). Hence, for all pixels that arefully covered by a single primitive, the 2-8 samples stored will usually be thesame. And that’s the idea behind the common color buffer compression schemes:Write a flag bit (either per pixel, or per quad, or on an
 even larger granularity)that denotes whether for all the pixels in a compression block, all theper-sample colors are in fact the same. And if that’s the case, we only need tostore the color once per pixel after all. This is fairly simple to detectduring write-back,
 and again (much like depth compression), it requires sometag bits that we can store in a small on-chip SRAM. If there’s an edge crossingthe pixels, we need the full bandwidth, but if the triangles aren’t too small(and they’re basically never all small), we
 can save a good deal of bandwidthon at least part of the frame. And again, we can use the same machinery toaccelerate clears.

On the subject of clears and compression, there’s another thing to mention:Some GPUs have “hierarchical Z”-like mechanisms that store, for a large blockof pixels (a rasterizer tile, maybe even larger) that the block was recentlycleared. Then you only need to
 store one color value for the whole tile (orlarger block) in memory. This gives you very fast color clears for some buffers(again, you need some tag bits for this!). However, as soon as any pixel withnon-clear color is written to the tile (or larger block),
 the “this was justcleared” flag needs to be… well, cleared. But we do save a lot of memorybandwidth on the clear itself and the first time a tile is read from memory.

And that’s it for our first rendering data path: just Vertex and Pixel Shaders(the most common path). In the next part, I’ll talk about Geometry Shaders andhow that pipeline looks. But before I conclude this post, I have a small bonustopic that fits into this
 section.

**Aside: Why no fully programmable blend?**

Everyone who writes rendering code wonders about this at some point – theregular blend pipeline a serious pain to work with sometimes. So why can’t weget fully programmable blend? We have fully programmable shading, after all!Well, we now have the necessary
 framework to look into this properly. There’stwo main proposals for this that I’ve seen – let’s look at the both in turn:

&nbsp; &nbsp; Blend in Pixel Shader – i.e. Pixel Shader reads framebuffer,computes blend equation, writes new output value.

&nbsp; &nbsp; Programmable Blend Unit – “Blend Shaders”, with subset of fullshader instruction set if necessary. Happen in separate stage after PS.

**1\. Blend in Pixel Shader**

This seems like a no-brainer: after all, we have loads and texture samples inshaders already, right? So why not just allow a read to the current rendertarget? Turns out that unconstrained reads are a really bad idea, because itmeans that every pixel being shaded
 could (potentially) influence every otherpixel being shaded. So what if I reference a pixel in the quad over to theleft? Well, a shader for that quad could be running this instant. Or I could besampling half of my current quad and half of another quads that’s
 currentlyactive – what do I do now? What exactly would be the correct results in thatregard, never mind that we’d probably have to shade all quads sequentially toreliably get them? No, that’s a can of worms. Unconstrained reads from theframe buffer in Pixel
 Shaders are out. But what if we get a special rendertarget read instruction that samples one of the active render targets at thecurrent location? Now, that’s a lot better – now we only need to worry aboutwrites to the location of the current quad, which is
 a way more tractableproblem.

However, it still introduces ordering constraints; we have to check all quadsgenerated by the rasterizer vs. the quads currently being pixel-shaded. If aquad just generated by the rasterizer wants to write to a sample that’ll bewritten by one of the Pixel Shaders
 that are currently in flight, we need towait until that PS is completed before we can dispatch the new quad. Thisdoesn’t sound too bad, but how do we track this? We could just have a “thissample is currently being shaded” bit flag… so how many of these bits
 do weneed? At 1920×1080 with 8x MSAA, about 2MB worth of them (that’s bytes notbits) – and that memory is global, shared and determines the rate at which wecan issue new quads (since we need to mark a quad as busy before we can issueit). Worse, with the hierarchical
 Z etc. tag bits, they were just a hint; if weran out of them, we could still render, albeit more slowly. But this memory isnot optional. We can’t guarantee correctness unless we’re really tracking everysample! What if we just tracked the “busy” state per pixel
 (or even quad), andany write to a pixel would block all other such writes? That would work, but itwould massively harm our MSAA performance: If we track per sample, we can shadeadjacent, non-overlapping triangles in parallel, no problem. But if we trackper
 pixel (or at lower granularity), we effectively serialize all the edgequads. And what happens to our fill rate for e.g. particle systems with lots ofoverdraw? With the pipeline I described, these render (more or less) as fast asthe ROPs can merge the incoming
 pixels into the store buffers. But if we needto avoid conflicts, we really end up shading the individual overlappingparticles in order. This isn’t good news for our shader units that are designedto trade latency for throughput, not at all.

Okay, so this whole tracking thing is a problem. What if we just force shadingto execute in order? That is, keep the whole thing pipelined and all shadersrunning in lockstep; now we don’t need tracking because pixels will finish inthe same order we put them
 into the pipeline! But the problem here is that weneed to make sure the shaders in a batch actually always take the exact sametime, which has unfortunate consequences: You always have to wait theworst-case delay time for every texture sample, need to always
 execute bothsides of every branch (someone might at some point need the then/else branches,and we need everything to take the same time!), always runs all loops throughfor the same number of iterations, can’t stop shading on discard… no, thatdoesn’t sound
 like a winner either.

Okay, time to face the music: Pixel Shader blend in the architecture I’vedescribed comes with a bunch of seriously tricky problems. So what about thesecond approach?

**2\. “Blend Shaders”**

I’ll say it right now: This can be made to work, but…

Let’s just say it has its own problems. For once, we now need another full ALU&#43; instruction decoder/sequencer etc. in the ROPs. This is not a small change –not in design effort, nor in area, nor in power. Second, as I mentioned nearthe start of this post, our
 regular “just go wide” tactics don’t work so wellfor blend, because this is a place where we might well get a bunch of quadshitting the same pixels in a row and need to process them in order, so we wantlow latency. That’s a very different design point than
 our regular unifiedshader units – so we can’t use them for this (it also means texturesampling/memory access in Blend Shaders is a big no, but I doubt that shocksanyone at this point). Third, pure serial execution is out at this point – toolow throughput.
 So we need to pipeline it. But to pipeline it, we need to knowhow long the pipeline is! For a regular blend unit, it’s a fixed length, soit’s easy. A blend shader would probably be the same. In fact, due to thedesign constraints, you’re unlikely to get a blend
 shader – more like a blendregister combiner, really, completely with a (presumably relatively low) upperlimit on the number of instructions, as determined by the length of thepipeline.

Point being, the serial execution here really constrains us to designs that arestill relatively low-level; nowhere near the fully programmable shader unitswe’ve come to love. A nicer blend unit with some extra blend modes, you candefinitely get; a more open
 register combiner-style design, possibly, thoughneither the API guys nor the hardware guys will like it much (the API becauseit’s a fixed function block, the hardware guys because it’s big and needs a bigALU&#43;control logic where they’d rather not have it).
 Fully programmable, withbranches, loops, etc. – not going to happen. At that point you might as wellbite the bullet and do what it takes to get the “Blend in Pixel Shader”scenario to work properly.

…and that’s it for this post! See you next time.

&nbsp;

&nbsp;

A trip through the GraphicsPipeline 2011, part 10

July 20, 2011

Welcome back. Last time, we dove into bottom end of the pixel pipeline. Thistime, we’ll switch back to the middle of the pipeline to look at what isprobably the most visible addition that came with D3D10: Geometry Shaders. Butfirst, some more words on how I
 decompose the graphics pipeline in this series,and how that’s different from the view the APIs will present to you.

**There’s multiple pipelines / anatomy of a pipeline stage**

This goes back to part 3, but it’s important enough to repeat it: if you lookin, for example, the D3D10 documentation, you’ll find a diagram of the “D3D10pipeline” that includes all stages that might be active. The “D3D10 pipeline”includes Geometry Shading,
 even if you don’t have a Geometry shader set, andthe same for Stream-Out. In the purely functional model of D3D10, the GeometryShading stage is always there; if you don’t set a Geometry Shader, it justhappens to be very simple (and boring): data is just passed
 through unmodifiedto the next pipeline stage(s) (Rasterization/Stream-Out).

That’s the right way to specify the API, but it’s the wrong way to think aboutit in this series, where we’re concerned with how that functional model isactually implemented in hardware. So how do the two shader stages we’ve seen sofar look? For VS, we went
 through the Input Assembler, which prepared a blockof vertices for shading, then dispatched that batch to a shader unit (whichchews on it for a while), and then some time later we get the results back,write them into a buffer (for Primitive Assembly), make
 sure they’re in theright order, then send them down to the next pipeline stage (Culling/Clippingetc.). For PS, we receive to-be-shaded quads from the rasterizer, batch themup, buffer them for a while until a shader unit is free to accept a new batch,dispatch
 a batch to a shader unit (which chews on it for a while), and then sometime later we get the results back, write them into a buffer (for the ROPs),make sure they’re in the right order, then do blend/late Z and send the resultson to memory. Sounds kind of familiar,
 doesn’t it?

In fact, this is how it always looks when we want to get something done by theshader units: we need a buffer in the front, then some dispatching logic (whichis in fact pretty universal for all shader types and can be shared), then we gowide and run a bunch
 of shaders in parallel, and finally we need another bufferand a unit that sorts the results (which we received potentially out-of-orderfrom the shader units) back into API order.

We’ve seen shader units (and shader execution) and we’ve seen dispatch; and infact, now that we’ve seen Pixel Shaders (which have some peculiarities likederivative computation, helper pixels, discard and attribute interpolation),we’re not gonna see any big
 additions to shader unit functionality until we getto Compute Shaders, with their specialized buffer types and atomics. So for thenext few parts, I won’t be talking about the shader units; what’s reallydifferent about the various shader types is the shape
 and interpretation ofdata that goes in and comes out. The shader parts that don’t deal with IO(arithmetic, texture sampling) stay the same, so I won’t be talking about them.

**The Shape of Tris to Shade**

So let’s have a look at how our IO buffers for Geometry Shaders look. Let’sstart with input. Well, that’s reasonably easy – it’s just what we wrote fromthe Vertex Shader! Or well, not quite; the Geometry Shader looks at primitives,not individual vertices, so
 what we really need is the output from PrimitiveAssembly (PA). Note that there’s multiple ways to deal with this; PA couldexpand primitives out (duplicating vertices if they’re referenced multipletimes), or it could just hand us one block of vertices (I’ll
 stick with the 32vertices I used earlier) with an associated small “index buffer” (since we’reindexing into a block of 32 vertices, we just need 5 bits per index). Eitherway works fine; the former is the natural input format for the clip/cull Idiscussed after
 PA, but the latter needs far less buffer space when running GS,so I’ll use that model here.

One reason you need to worry about amount of buffer space with GS is that itcan work on some pretty large primitives, because it doesn’t just support plainlines or triangles (2 and 3 vertices per primitive respectively), but alsolines/triangles with adjacency
 information (4/6 vertices per primitive). AndD3D11 adds input primitives that are much fatter still – a GS can also consumespatches with up to 32 control points as input. Duplicating the vertices of e.g.a 16-control point patch, which could each have up to
 16 vector attributes (32with D3D11)? That’d be some serious memory waste. So I’m assumingnon-duplicated, indexed vertices for this path. Which makes the input for abatch of primitives: the VS output, plus a (relatively small) index buffer.

Now, the geometry shader runs per primitive. For vertex shaders, we needed togather a batch of vertices, and we chose our batch size with a simple greedyalgorithm that tries to pack as many vertices into a batch as possible withoutsplitting a primitive across
 multiple batches – fair enough. And for pixelshading, we get plenty of quads from the rasterizer and pack them all intobatches. Geometry Shaders are a bit more inconvenient – our input block isguaranteed to contain at least one full primitive, and possibly
 several – butother than that, the number of primitives in that block completely depends onthe vertex cache hit rate. If it’s high and we’re using triangles, we might getsomething like 40-43; if we’re using triangles with adjacency information wecould have
 as little as 5 if we’re unlucky.

Of course, we could try to collect primitives from several input blocks here,but that’s kind of awkward too. Now we need to keep multiple input blocks andindex buffers around for a single GS batch, and if a single batch can refer tomultiple index buffers that
 means each primitive in that batch now needs toknow where to get the indices and vertex data from – more storage requirements,more management, more overhead. Also ugly. And of course even with two inputblocks you’re still at crappy utilization if you hit two
 input batches with lowvertex cache hit rate. You can support more input blocks, but that eats away atmemory – and remember, you need space for the output geometry too (I’ll get tothat in a bit).

So this is our first snag: with VS, we could basically pick our target batchsize, and we chose to not always generate full batches so as to make our livesin PA (and here in the GS, and later in the HS too) a bit easier. With PS, wealways shade quads, and even
 fairly small tris usually hit multiple quads so weget an okay ratio of number of quads to number of tris. But with GS, we don’thave full control over either ends of the pipeline (since we’re in themiddle!), and we need multiple input vertices per primitive
 (as opposed tomultiple quads per one input triangle), so buffering up a lot of input isexpensive (both in terms of memory and in the amount of management overhead weget).

At this stage, you can basically pick how many input blocks you’re willing tomerge to get one block of primitives to geometry shade; that number is going tobe low because of the memory requirements (I’d be very surprised to see morethan 4), and depending on
 how important you judge GS to be, you might even pick1, i.e. don’t merge across input blocks at all and live with crappy utilizationon GS shading blocks/Warps/Wavefronts! That’s not great with triangles andreally bad with the primitives that have even more
 vertices, but not much of anissue when your main use case for GS in practice is expanding points to quads(point sprites) and maybe rendering the occasional cube shadow map (using theViewport Array Index/Rendertarget Index – I’ll get to that in a bit).

**GS output: no rose garden over here, either**

So how’s it looking on the output side? Again, this is more complicated thanthe plain VS data flow. Much more complicated in fact; while a VS only outputsone thing (shaded vertices) with a 1:1 correspondence between unshaded andshaded vertices, a GS outputs
 a variable number of vertices (up to a maximumthat’s specified at compile time), and as of D3D11 it can also have multipleoutput streams – however, a maximum of one stream can be sent on down the restof the pipeline, which is the path I’m talking about now.
 The other destinationfor GS data (Stream-Out) will be covered in the next part.

A GS produces variable-sized output, but it needs to run with bounded memoryrequirements (among other things, the amount of memory available for buffersdetermines how many primitives can be Geometry Shaded in parallel), which iswhy the maximum number of output
 vertices is fixed at compile-time. This(together with the number of written output attributes) determines how muchbuffer space is allocated, and thus indirectly the maximum number of parallelGS invocations; if that number is too low, latency can’t be fully
 hidden, andthe GS will stall for some percentage of the time.

Also note that the GS inputs primitives (e.g. points, lines, triangles orpatches, optionally with adjacency information), but outputs vertices – eventhough we send primitives down to the rasterizer! If the output primitive typeis points, this is trivial. For
 lines and triangles however, we need toreassemble those vertices back into primitives again. This is handled by makingthe output vertices form a line or triangle strip, respectively. This handleswhat are perhaps the 3 most important cases well: single lines,
 triangles, orquads. It’s not so convenient if the GS tries to do some actual extrusion orgenerate otherwise “complicated” geometry, which often needs several “restartstrip” markers (which boils down to a single bit per vertex that denoteswhether the current
 strip is continued or a new strip is started). So why thelimitation? At the API level, it seems fairly arbitrary – why can’t the GS justoutput a vertex list together with a small index buffer?

The answer boils down to two words: Primitive Assembly. This is what we’redoing here – taking a number of vertices and assembling them into a fullprimitive to send down the pipeline. But we already use that functional blockin this data path, just in front of
 the GS. So for GS, we need a secondprimitive assembly stage, which we’d like to keep simple, and assemblingtriangle strips is very simple indeed: a triangle is always 3 vertices from theoutput buffer in sequential order, with only a bit of glue logic to keep
 trackof the current winding order. In other words, strips are not significantly morecomplex to support than what is arguably the simplest primitive of all(non-indexed lines/triangles), but they still save output buffer space (andhence give us more potential
 for parallelism) for typical primitives likequads.

**API order again**

There’s a few problems here, however: in the regular vertex shading path, weknow exactly how many primitives there are in a batch and where they are, evenbefore the shaded vertices arrive at the PA buffer – all this is fixed from thepoint where we set up the
 batches to shade. If we, for example, have multipleunits for cull/clip/triangle setup, they can all start in parallel; they knowwhere to get their vertex data from, and they can know ahead of time which“sequence number” their triangle will have so it can all
 be put into order.

For GS, we don’t generally know how many primitives we’re gonna generate beforewe get the outputs back – in fact, we might not have produced any! But we stillneed to respect API order: it’s first all primitives generated from GSinvocation 0, then all primitives
 from invocation 1, and so on, through to theend of the batch (and of course the batches need to be processed in order too,same as with VS). So for GS, once we get results back, we first need to scanover the output data to determine the locations where complete
 primitivesstart. Only then can we start doing cull, clip and triangle setup (potentiallyin parallel). More extra work!

**VPAI and RTAI**

These are two features added with GS that don’t actually affect Geometry Shaderexecution, but do have some effect on the processing further downstream, so Ithought I’d mention them here: The Viewport Array Index (here, VPAI for short)and Render-target Array
 Index (RTAI). RTAI first, since it’s a bit easier toexplain: as you hopefully know, D3D10 adds support for texture arrays. Well,the RTAI gives you render-to-texture-array support: you set a texture array asrender target, and then in the GS you can select per-primitive
 to which arrayindex the primitive should go. Note that because the GS is writing vertices notprimitives, we need to pick a single vertex that selects the RTAI (and alsoVPAI) per primitive; this is always the “leading vertex”, i.e. the firstspecified vertex
 that belongs to a primitive. One example use case for RTAI isrendering cubemaps in one pass: the GS decides per primitive to which of thecube faces it should be sent (potentially several of them). VPAI is anorthogonal feature which allows you to set multiple
 viewports and scissor rects(up to 15), and then decide per primitive which viewport to use. This can beused to render multiple cascades in a Cascaded Shadow Map in a single pass, forexample, and it can also be combined with RTAI.

As said, both features don’t affect GS processing significantly – they’re justextra data that gets tacked onto the primitive and then used later: the VPAIgets consumed during the viewport transform, while the RTAI makes it all theway down to the pixel pipeline.

**Summary so far**

Okay, so there’s some amount of trouble on the input end – we don’t fully getto pick our input data format, so we need extra buffering on the input data,and even then we have a variable amount of input primitives which we’re notnecessarily going to be able
 to partition into nice big batches. And on theoutput end, we’re again assembling a variable number of primitives, don’tnecessarily know which GS will produce how many primitives in advance (thoughfor some GSs we’ll be able to determine this statically from
 the compiled code,for example because all vertex emits are outside of flow control or insideloops with a known iteration count and no early-outs), and have to spend sometime parsing the output before we can send it on to triangle setup.

If that sounds more involved than what we had in the VS-only case, that’sbecause it is. This is why I mentioned above that it’s a mistake to think ofthe GS as something that always runs – even a very simple GS that does nothingexcept pass the current triangle
 through goes through two more bufferingstages, an extra round of primitive assembly, and might execute on the shaderunits with poor utilization. All of this has a cost, and it tends to add up: Ichecked it when D3D10 hardware was fairly new, and on both AMD
 and NVidiahardware, even a pure pass-through GS was between 3x and 7x slower than no GSat all (in a geometry-limited scenario, that is). I haven’t re-run thisexperiment on more recent hardware; I would assume that it’s gotten better bynow (this was the first
 generation to implement GS, and features don’t usuallyhave good performance in the first GPU generation that implements them), butthe point still stands: just sending something through the GS pipe, even ifnothing at all happens there, has a very visible cost.

And it doesn’t help that GSs produce primitives as strips, sequentially; for aVertex Shader, we get one invocation per vertex, which reads one vertex andwrites one vertex (nice). For a GS, though, we might end up having only a batchof 11 GSs running (because
 there wasn’t enough primitives in the input buffer),with each of them running fairly long and producing something like 8 outputvertices. That’s a long time to be running at low utilization! (Remember weneed somewhere between 16 and 64 independent jobs per
 batch we dispatch to theshader units). It’s even more annoying if the GS mainly consists of a loop –for example, in the “render to cube map” case I mentioned for RTAI, we loopover the 6 faces in a cube, check if a triangle is visible on that face, andoutput
 a triangle if that’s the case. The computations for the 6 faces arereally independent; if possible, we’d like to run them in parallel!

**Bonus: GS Instancing**

Well, enter GS Instancing, another feature new in D3D11 – poorly documented,sadly (and I’m not sure if there’s any good examples for it in the SDK). It’sfairly simple to explain, though: for each input primitive, the GS gets run notjust once but multiple times
 (this is a static count selected at compile time).It’s basically equivalent to wrapping the whole shader in a

for (int i = 0; i &lt; N; i&#43;&#43;)

{

&nbsp; &nbsp; // ...

}

block, only the loop is handled outside the shader by actually generatingmultiple GS invocations per input primitive, which helps us get larger batchsizes and thus better utilization. The i is exported to the shader as asystem-generated value (in D3D11, with
 Semantic SV_GSInstanceID). So if youhave a GS like that, just get rid of the outer loop, add a [instances(N)]declaration and declare i as input with the right semantic and it’ll probablyrun faster for very little work on your part – the magic of giving moreindependent
 jobs to a massively parallel machine!

Anyway, that’s it on Geometry Shaders. I’ve skipped Stream-Out, but this postis already long enough, and besides SO is a big enough topic (and independentenough of GS!) to warrant its own post. Next post, to be more precise. Untilthen!

&nbsp;

A trip through the GraphicsPipeline 2011, part 11

August 14, 2011

Welcome back! This time, the focus is going to be on Stream-Out (SO). This is afacility for storing the Output of the Geometry Shader stage to memory, insteadof sending it down the rest of the pipeline. This can be used to e.g. cacheskinned vertex data, or
 as a sort of poor man’s Compute Shader on D3D10-levelhardware using the D3D10 API (note that with D3D11, you can just use CS 4.0,even on D3D10 hardware). And just like the GS Instancing I mentioned last time,some of this is very poorly described in the API
 docs, so I’ll have a fewcomments about API usage even though it’s technically out of the intended scopeof this series.

**Vertex Shader Stream-Out (i.e. SO with NULL GS)**

This is one of the features that’s not properly explained in the D3D10 (orD3D11, for that matter) docs; in fact, it’s not mentioned there at all exceptfor a small throwaway remark in “Getting Started with the Stream-Output Stage(Direct3D 10)”. You’re supposed
 to figure it out from the examples – whichthemselves don’t exactly go out of their way to make it clear what’s going on.That’s a pity – VS Stream-Out is easier than GS SO, and has some pretty usefulapplications by itself (e.g. caching skinned vertices).

So here’s how it’s done in D3D10 and 11: You simply pass Vertex Shader bytecode(instead of GS bytecode) to CreateGeometryShaderWithStreamOutput. Yes, the docsmention something about “Size of the compiled geometry shader” here – ignoreit. What you get back is
 a Geometry Shader object that you can then pass toGSSetShader. This is, in effect, a NULL Geometry Shader – it doesn’t actuallygo through GS processing. It’s just some wrapper (more like duct tape really)to make it fit into the API model, where all rendering
 passes through the GSstage and SO comes right after GS – though as I’ve explained last time, actualHW tends to skip the GS stage completely when there’s no GS set.

So the shaded vertices get assembled into primitives as before, but instead ofgetting sent down the rest of the pipeline as already described, they getforwarded to Stream-Out, where they arrive – as always – in a buffer. Whatexactly happens with them then depends
 on the Stream-Out declaration (which ispassed at creation time). In the Stream-Out declaration, the app gets tospecify where it wants each output vector to end up in the Stream-Out targets(or SO targets for short). If the SO declaration “matches” the Vertex
 ShaderOutput Declaration (i.e. the same attributes in the same order), data from theinput buffers can be streamed more or less unprocessed into memory. If itdoesn’t match the declaration exactly – it might skip some attributes writtenby the shader, or write
 them in a different order – either way, there’s someextra reordering involved. This might involve a dedicated reordering unit(which basically implements a gather-type operation from the SO input buffers),or it might involve generating lots of small memory
 writes instead of largeburst writes, or something similar. Either way, it’s extra effort and generallyslower; the details of what exactly triggers a slow path depend on the hardwarespecifics, but really, it doesn’t matter that much. If you want optimal SOperformance,
 just make sure the SO declaration and Output declarations agree.

Another point is that SO usually doesn’t have access to a very high-performancepath to the memory subsystem. Unlike e.g. the ROPs, SO isn’t really (yet?) afull citizen in current GPU designs, so it often only has access to one memorychannel or something of
 the sort. That’s something to keep in mind if you’reproducing a lot of data via SO. This is compounded by SO outputs always beingfull floats, so there’s no way to conserve bandwidth by using one of the packedvertex data types.

Final remark on VS SO: As I mentioned earlier, SO operates on assembledprimitives, not individual vertices. Note that Primitive Assembly discardsadjacency information if it makes it that far down the pipeline, and since thishappens before SO, vertices corresponding
 to adjacency info won’t make it intoSO buffers either. SO working on primitives not individual vertices is relevantfor use cases like instancing a single skinned mesh (in a single pose) severaltimes. If you were to draw your triangle mesh as you usually would
 and then useSO on that, this results in a data explosion – you get 3 unpacked, unsharedvertices per input primitive. This works, but isn’t exactly an efficient use ofbandwidth, both on the SO and the later vertex input side. Instead, you shoulddraw your triangle
 mesh as a (non-indexed) point list in the first pass,thereby shading each vertex exactly once. The SO buffer then ends up in 1:1correspondence to your original vertex buffer, only with skinned instead of non-skinnedvertices. You can then use that vertex buffer
 with your original primitivetopology and index buffer.

**Geometry Shader SO: Multiple streams**

This basically works like SO with a NULL GS, except there’s a Geometry Shaderinvolved, which adds some new capabilities (and complications). In the VS case,we just had one output stream (note that streams are a D3D11&#43; feature – theydon’t exist on D3D10-level
 HW). That stream could be sent to SO or not, and itcould also be sent to down the pipeline to viewport/clip/cull or not, butthat’s it. But Geometry Shaders allow multiple streams, which makes outputrouting a bit more difficult.

Basically, every GS can write to (as of D3D11) up to 4 streams. Each stream maybe sent on to SO targets – yes, plural: a single stream can write to multipleSO targets, but a single SO target can receive values from only one stream,i.e. this is a one-to-many
 relationship, not a fully general many-to-many one.The presence of streams has some implications for SO buffering – instead of asingle input buffer like I described in the NULL GS case, we now may havemultiple input buffers, one per stream. In addition to
 SO targets, up to onestream may be sent down the pipe – i.e. the regular rendering pipeline and SOmay be used simultaneously.

As in the NULL GS case, SO works on primitives, not individual vertices – thatis, the strips you output in the GS get expanded out to full lines or trianglesbefore they get into SO.

**Tracking output size**

There’s another issue here: we don’t necessarily know how much output data isgoing to be produced from SO. For GS, this comes about because each GSinvocation may produce a variable number of output primitives; but even in thesimpler VS case, as soon as indexed
 primitives are involved, the app might slipsome “primitive cut” indices in there that influence how many primitivesactually get written. This is a problem if we then want to draw from that SObuffer later, because we don’t know how many vertices are actually
 in there! Wedo have an upper bound – the maximum capacity of the buffer as created – butthat’s it. Now, this could be resolved using some kind of query mechanism, butonce you think it through, that seems fairly backwards: at the point we’reusing the SO buffer
 for drawing, we obviously do know how many primitives weactually wrote – the SO unit needs to keep track of its current outputposition, after all! If we employed some query mechanism, we would end uptransporting that single 32-bit value back over the bus to
 the driver, whichpasses it on to the API, which passes it on to the app – which then immediatelydispatches another draw, going through all the layers again in the oppositedirection.

So that’s now how it’s solved. Instead, there’s DrawAuto. The idea is verysimple – the GPU already knows how many valid vertices it actually wrote to theoutput buffer; the SO unit keeps track of that while it’s writing, and thefinal counter is also kept in
 memory (along with the buffer) since the app mayrender to a SO buffer in multiple passes. This counter is then used forDrawAuto, instead of having the app submit an explicit count itself –simplifying things considerably and avoiding the costly round-trip completely.Note
 that this query mechanism does exist – both for checking the number ofvertices written and to determine whether an overflow occurred. But it’s not onthe critical path for rendering from SO buffers, which makes things a lotsimpler for driver developers.

And that’s it for SO, really. Not really a lot of HW info in this one, and notreally a super-interesting topic from a pipeline perspective, which is why ittook me so long to finish; sorry about that. Next up is Tessellation – thisshould be a lot quicker, since
 it’s a fun topic :)

&nbsp;

&nbsp;

&nbsp;

&nbsp;

A trip through the Graphics Pipeline 2011, part 12

September 6, 2011

Welcome back! This time, we’ll look into what is perhaps the “poster boy”feature introduced with the D3D11 / Shader 5.x hardware generation:Tessellation. This one is interesting both because it’s a fun topic, andbecause it marks the first time in a long while
 that a significant user-visiblecomponent has been added to the graphics pipeline that’s not programmable.

Unlike Geometry Shaders, which are conceptually quite easy (it’s just a shaderthat sees whole primitives as opposed to individual vertices), the topic of“Tessellation” requires some more explanation. There’s tons of ways totessellate geometry – to name just
 the most popular ones, there’s SplinePatches in dozens of flavors, various types of Subdivision Surfaces, andDisplacement Mapping – so from the bullet point “Tessellation” alone it’s notat all obvious what services the GPU provides us with, and how they areimplemented.

To describe how hardware tessellation works, it’s probably easiest to start inthe middle – with the actual primitive tessellation step, and the variousrequirements that apply to it. I’ll get to the new shader types (Hull Shadersand Domain Shaders in D3D11 parlance,
 Tessellation Control Shader andTessellation Evaluation Shader in OpenGL 4.0 lingo) later.

**Tessellation – not quite like you’d expect**

Tessellation as implemented by Shader 5.x class HW is of the “patch-based”variety. Patch types in the CG literature are mostly named by what kind offunction is used to construct the tessellated points from the control points(B-spline patches, Bézier triangles,
 etc.). But we’ll ignore that part for now,since it’s handled in the new shader types. The actual fixed-functiontessellation unit deals only with the topology of the output mesh (i.e. howmany vertices there are and how they’re connected to each other); and
 it turnsout that from this perspective, there’s basically only two different types ofpatches: quad-based patches, which are defined on a parameter domain with twoorthogonal coordinate axes (which I’ll call u and v here, both are in [0,1])and usually constructed
 as a tensor product of two one-parameter basisfunctions, and triangle-based patches, which use a redundant representationwith three coordinates (u, v, w) based on barycentric coordinates (i.e. u, v, w\ge 0, u &#43; v &#43; w = 1). In D3D11 parlance, these are the
 “quad” and “tri”domains, respectively. There’s also an “isoline” domain which instead of a 2Dsurface produces one or multiple 1D curves; I’ll treat it the same way as I didlines and point primitives throughout this series: I acknowledge its existencebut won’t
 go into further detail.

Tessellated primitives can be drawn naturally in their respective domaincoordinate systems. For quads, the obvious choice of drawing the domain is as aunit square, so that’s what I’ll use; for triangles, I’ll use an equilateraltriangle to visualize things.
 Here’s the coordinate systems I’ll be using inthis post with both the vertices and edges labeled:

Anyway, both triangles and quads have what I would consider a “natural” way totessellate them, depicted below. But it turns out that’s not actually the meshtopology you get.

Here’s the&nbsp;_actual_&nbsp;meshes that the tessellator will produce forthe given input parameters:

For quads, this is (roughly) what we’re expecting – except for some flippeddiagonals, which I’ll get to in a minute. But the triangle is a completelydifferent beast. It’s got a very different topology from the “natural”tessellation I showed above, including
 a different number of vertices (12instead of 10). Clearly, there’s something funny going on here – and thatsomething happens to be related to the way transitions between differenttessellation levels are handled.

**Making ends meet**The elephant in the room is handling transitions betweenpatches. Tessellating a single triangle (or quad) is easy, but we want to beable to determine tessellation factors per-patch, because we only want to spendtriangles where
 we need them – and not waste tons of triangles on some distant(and possibly backface-culled) parts of the mesh. Additionally, we want to beable to do this quickly and ideally without extra memory usage; that means aglobal fix-up post-pass or something of that
 caliber is out of the question.

The solution – which you’ve already encountered if you’ve written a Hull orDomain shader – is to make all of the actual tessellation work purely local andpush the burden of ensuring watertightness for the resulting mesh down to theshaders. This is a topic all
 by itself and requires, among other things,&nbsp;[<span style="color:#336699">great care in the Domain Shader code</span>](http://www.ludicon.com/castano/blog/2010/09/precise/); I’ll skip all the detailsabout expression
 evaluation in shaders and stick with the basics. The basicmechanism is that each patch has multiple tessellation factors (TFs), which arecomputed in the Hull Shader: one or two for the actual inside of the patch,plus one for each edge. The TFs for the inside
 of the patch can be chosenfreely; but if two patches share an edge, they’d better compute the exact sameTFs along that edge, or there will be cracks. The hardware doesn’t care – itwill process each patch by itself. If you do everything correctly, you’ll get
 anice watertight mesh, otherwise – well, that’s your problem. All the HW needsto make sure is that it’s&nbsp;_possible_&nbsp;to get watertight meshes,preferably with reasonable efficiency. That by itself turns out to be tricky insome places; I’ll get to that
 later.

So, here are some new reference patches – this time with different TFs alongeach edge so we can see how that works:

I’ve colored the areas influenced by the different edge tessellation factors;the uncolored center part in the middle only depends on the inside TFs. Inthese images, the u=0 (yellow) edge has a TF of 2, the v=0 (green) edge has aTF of 3, the u=1 / w=0 (pink)
 edge has a TF of 4, and the v=1 (quad only, cyan)edge has a TF of 5 – exactly the number of vertices along the correspondingouter edge. As should be obvious from these two images, the basic buildingblock for topology is just a nice way to stitch two subdivided
 edges withdifferent number of vertices to each other. The details of this are somewhattricky, but not particularly interesting, so I won’t go into it.

As for the inside TFs, quads are fairly easy: The quad above has an inside TFof 3 along u and 4 along v. The geometry is basically that of a regular grid ofthat size, except with the first and last rows/columns replaced by the respectivestitching triangles
 (if any edge has a TF of 1, the resulting mesh will havethe same structure as if the inside TFs for u/v were both 2, even if they’resmaller than that). Triangles are a bit more complicated. Odd TFs we’ve alreadyseen – for a TF of&nbsp;, they produce a mesh consisting
 of&nbsp;&nbsp;concentric rings, the innermostof which is a single triangle. For even TFs, we get&nbsp;concentric rings with a center vertexinstead of a center triangle. Below is an image of the simplest even case,&nbsp;, which consists just of edge stitchesplus the center vertex.

Finally, when triangulating quads, the diagonal is generally chosen to pointaway from the center of the patch (in the domain coordinate space), with aconsistent tie-breaking rule. This is simply to ensure maximum rotationalsymmetry of the resulting meshes –
 if there’s extra degrees of freedom, mightas well use them!

**Fractional tessellation factors and overall pipeline flow**So far, I’veonly talked about integer TFs. In two of the so-called “partitioning types”,namely “Integer” and “Pow2″, that’s all the Tessellator sees. If the shadergenerates a non-integer
 (or, respectively, non-power-of-2) TF, it will simplyget rounded up to the next acceptable value. More interesting are the remainingtwo partitioning types: Fractional-odd and Fractional-even tessellation.Instead of jumping from tessellation factor to tessellation
 factor (which wouldcause visible pops), new vertices start out at the same position as an existingvertex in the mesh and then gradually move to their new positions as the TFincreases.

For example, with fractional-odd tessellation, if you were to use an inner TFof 3.001 for the above triangle, the resulting mesh would look very much likethe mesh for a TF of 3 – but topologically, it’d be the same as if the TF was5, i.e. it’s a patch with
 3 concentric rings, even though the middle ring isvery narrow. Then as the TF gets closer to 5, the middle ring expands until itis eventually at its final position for TF 5\. Once you raise the TF past 5, themesh will be topologically the same as is the TF
 was 7, but again with a numberof almost-degenerate triangles in the middle, and so forth. Fractional-eventessellation uses the same principle, just with even TFs.

The output of the tessellator then consists of two things: First, the positionsof the tessellated vertices in domain coordinates, and second, thecorresponding connectivity information – basically an index buffer.

Now, with the basic function of the fixed-function tessellator unit explained,let’s step back and see what we need to do to actually churn out primitives:First, we need to input a bunch of input control points comprising a patch intothe Hull Shader. The HS
 then computes output control points and “patchconstants” (both of which get passed down to the Domain Shader), plus allTessellation Factors (which are essentially just more patch constants). Then werun the fixed-function tessellator, which gives us a bunch
 of Domain Positionsto run the Domain Shader at, plus the associated indices. After we’ve run theDS, we then do another round of primitive assembly, and then send theprimitives either down to the GS pipeline (if it’s active) or Viewport transform,Clip and Cull
 (if not).

So let’s look a bit into the HS stage.

**Hull Shader execution**Like&nbsp;[<span style="color:#336699">Geometry Shaders</span>](http://fgiesen.wordpress.com/2011/07/20/a-trip-through-the-graphics-pipeline-2011-part-10/), Hull Shaders work on full
 (patch) primitives as input –with all the input buffering headaches that causes. How much of a headacheentirely depends on the type of input patch. If the patch type is somethinglike a cubic Bézier patch, we need 4×4 = 16 input points&nbsp;_per patch_&nbsp;andmight
 just produce a single quad of output (or even none at all, if the patchis culled); clearly, that’s a somewhat awkward amount of data to work with, anddoesn’t lend itself to very efficient shading. On the other hand, iftessellation takes plain triangles as
 input (which a lot of people do), inputbuffering is pretty tame and not likely to be a source of problems orbottlenecks.

More importantly, unlike Geometry Shaders (which run for every primitive), HullShaders don’t run all that often – they run once&nbsp;_per patch_, and aslong as there’s any actual tessellation going on (even at modest TFs), we haveway less patches than we
 have output triangles. In other words, even when HSinput is somewhat inefficient, it’s less of an issue than in the GS case simplybecause we don’t hit it that often.

The other nice attribute of Hull Shaders is that, unlike Geometry Shaders, theydon’t have a variable amount of output data; they produce a fixed amount ofcontrol points, each which a fixed amount of associated attributes, plus afixed amount of patch constants.
 All of this is statically known at compiletime; no dynamic run-time buffer management necessary. If we Hull Shade 16hulls at a time, we know exactly where the data for each hull will end upbefore we even start executing the shader. That’s definitely an advantage
 overGeometry Shaders; for lots of Geometry Shaders, it’s possible to knowstatically how many output vertices will be generated (for example because allthe control flow leading to emit / cut instructions can be statically evaluatedat compile time), and for
 all of them, there’s a guaranteed maximum number ofoutput vertices, but for HS, we have a guaranteed fixed amount of output data,no additional analysis required. In short, there’s no problems with outputbuffer management, other than the fact that, again depending
 on the primitivetype, we might need lots of output buffer space which limits the amount ofparallelism we can achieve (due to memory/register constraints).

Finally, Hull Shaders are somewhat special in the way they are compiled inD3D11; all other shader types basically consist of one block of code (with somesubroutines maybe), but Hull Shaders are generated factored into multiplephases, each of which can consist
 of multiple (independent) threads ofexecution. The details are mainly of interest to driver and shader compilerprogrammers, but suffice it to say that your average HS comes packaged in aform that exposes lots of latent parallelism, if there is any. It certainlyseems
 like Microsoft was really keen to avoid the bottlenecks that plagueGeometry Shaders this time around.

Anyway, Hull Shaders produce a bunch of output per patch; most of it is justkept around until the corresponding Domain Shaders run, except for the TFs,which get sent to the tessellator unit. If any of the TFs are less than orequal to zero (or NaN), the patch
 is culled, and the corresponding controlpoints and patch constants silently get thrown away. Otherwise, the Tessellator(which implements the functionality described above) kicks in, reads thejust-shaded patches, and starts churning out domain point positions
 andtriangle indices, and we need to get ready for DS execution.

**Domain Shaders**Just like for&nbsp;[<span style="color:#336699">Vertex Shading</span>](http://fgiesen.wordpress.com/2011/07/03/a-trip-through-the-graphics-pipeline-2011-part-3/)&nbsp;way back, we want to gather
 multiple domainvertices into one batch that we shade together and then pass on the PA. Thefixed-function tessellator can take care of this: “just” handle it along withproducing vertex positions and indices (I put the “just” in quotes here becausethis does
 involve some amount of bookkeeping).

In terms of input and output, Domain Shaders are very simple indeed: the onlyinput they get that actually varies per vertex is the domain point u and vcoordinates (w, when used, doesn’t need to be computed or passed in by thetesselator; since&nbsp;, it can be computed
 as&nbsp;). Everything else is either patchconstants, control points (all of which are the same across a patch) orconstant buffers. And output is basically the same as for Vertex Shaders.

In short, once we get to the DS, life is good; the data flow is almost assimple as for VS, which is a path we know how to run efficiently. This isperhaps the biggest advantage of the D3D11 tessellation pipeline over GeometryShaders: the actual triangle amplification
 doesn’t happen in a shader, where wewaste precious ALU cycles and need to keep buffer space for a worst-caseestimate of vertices, but in a localized element (the tessellator) that isbasically a state machine, gets very little input (a few TFs) and produces
 verycompact output (effectively an index buffer, plus a 2D coordinate per outputvertex). Because of this, we need way less memory for buffering, and can keepour Shader Units busy with actual shading work instead of housekeeping.

And that’s it for this post – next up: Compute Shaders, aka the final part inmy original outline for this series! Until then.

**Final remarks**As usual, I cut a few corners. There’s the “isoline” patchtype, which I didn’t go into at all (if there’s any demand for this, I canwrite it up). The Tessellator has all kinds of symmetry and precisionrequirements; as far as vertex
 domain positions are concerned, you canbasically expect bit-exact results between the different HW vendors, becausethe D3D11 spec really nails this bit down. What’s intentionally not nailed downis the order in which vertices or triangles are produced – an
 implementationcan do what it wants there, provided it does so consistently (i.e. the sameinput has to produce the same output, always). There’s a bunch of subtleconstraints that go into this too – for example, all domain positions writtenby the Tessellator
 need to have both u and 1-u (and also v and 1-v) exactlyrepresentable as float; there’s a bunch of necessary conditions like this sothat Domain Shaders can then produce watertight meshes (this rule in particularis important so that a shared edge AB between
 two patches, which is AB to onepatch and BA to the other, can get tessellated the same way for both patches).

Writing Domain Shaders so they actually can’t produce cracks is tricky andrequires great care; I intentionally sidestep the topic because it’s outsidethe scope of this series. Another much more trivial issue that I didn’t mentionis the winding order of triangles
 generated by the Tessellator (answer: it’s upto the App – both clockwise and counterclockwise are supported).

The description of Input/Output buffering for Hull and Domain shaders issomewhat terse, but it’s very similar to stages we’ve already seen, so I’drather keep it short and avoid extra clutter; re-read the posts on VertexShaders and Geometry Shaders if this was
 too fast.

Finally, because the Tesselation pipeline can feed into the GS, there’s thequestion of whether it can generate adjacency information. For the “inside” ofpatches this would be conceivable (just more indices for the Tessellator unitto write), but it gets ugly
 fast once you reach patch edges, since cross-patchadjacency needs exactly the kind of global “mesh awareness” that theTessellation pipeline design tries so hard to avoid. So, long story short, no,the tessellator will not produce adjacency information for the
 GS, just plaintriangles.

**A trip through the GraphicsPipeline 2011, part 13**&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; October 9, 2011&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; Welcome back to what’s going to be the last “official” part of this series– I’ll do more GPU-related posts in the future, but this series is long enoughalready.
 We’ve been touring all the regular parts of the graphics pipeline,down to different levels of detail. Which leaves one major new featureintroduced in DX11 out: Compute Shaders. So that’s gonna be my topic this timearound.

**Execution environment**For this series, the emphasis has been on overalldataflow at the architectural level, not shader execution (which is explainedwell elsewhere). For the stages so far, that meant focusing on the input pipedinto and output
 produced by each stage; the way the internals work was usuallydictated by the shape of the data. Compute shaders are different – they’rerunning by themselves, not as part of the graphics pipeline, so the surfacearea of their interface is much smaller.

In fact, on the input side, there’s not really any buffers for input data atall. The only input Compute Shaders get, aside from API state such as the boundConstant Buffers and resources, is their thread index. There’s a tremendouspotential for confusion here,
 so here’s the most important thing to keep inmind: a “thread” is the atomic unit of dispatch in the CS environment, and it’sa substantially different beast from the threads provided by the OS that youprobably associate with the term. CS threads have their
 own identity andregisters, but they don’t have their own Program Counter (Instruction Pointer)or stack, nor are they scheduled individually.

In fact, “threads” in CS take the place that individual vertices had during&nbsp;[<span style="color:#336699">Vertex Shading</span>](http://fgiesen.wordpress.com/2011/07/03/a-trip-through-the-graphics-pipeline-2011-part-3/),
 or individual pixels during&nbsp;[<span style="color:#336699">Pixel Shading</span>](http://fgiesen.wordpress.com/2011/07/10/a-trip-through-the-graphics-pipeline-2011-part-8/). And they get treated the same way: assemble
 a bunch ofthem (usually, somewhere between 16 and 64) into a “Warp” or “Wavefront” andlet them run the same code in lockstep. CS threads don’t get scheduled – Warpsand Wavefronts do (I’ll stick with “Warp” for the rest of this article; mentallysubstitute “Wavefront”
 for AMD). To hide latency, we don’t switch to adifferent “thread” (in CS parlance), but to a different Warp, i.e. a differentbundle of threads. Single threads inside a Warp can’t take branchesindividually; if at least one thread in such a bundle wants to execute
 acertain piece of code, it gets processed by all the threads in the bundle –even if most threads then end up throwing the results away. In short, CS“threads” are more like SIMD lanes than like the threads you see elsewhere inprogramming; keep that in mind.

That explains the “thread” and “warp” levels. Above that is the “thread group”level, which deals with – who would’ve thought? – groups of threads. The sizeof a thread group is specified during shader compilation. In DX11, a threadgroup can contain anywhere
 between 1 and 1024 threads, and the thread groupsize is specified not as a single number but as a 3-tuple giving thread x, y,and z coordinates. This numbering scheme is mostly for the convenience of shadercode that addresses 2D or 3D resources, though it also
 allows for traversaloptimizations. At the macro level, CS execution is dispatched in multiples ofthread groups; thread group IDs in D3D11 again use 3D group IDs, same as threadIDs, and for pretty much the same reasons.

Thread IDs – which can be passed in in various forms, depending on what theshader prefers – are the only input to Compute Shaders that’s not the same forall threads; quite different from the other shader types we’ve seen before. Thisis just the tip of the iceberg,
 though.

**Thread Groups**The above description makes it sound like thread groups area fairly arbitrary middle level in this hierarchy. However, there’s oneimportant bit missing that makes thread groups very special indeed: ThreadGroup Shared Memory (TGSM).
 On DX11 level hardware, compute shaders have accessto 32k of TGSM, which is basically a scratchpad for communication betweenthreads in the same group. This is the primary (and fastest) way by whichdifferent CS threads can communicate.

So how is this implemented in hardware? It’s quite simple: all threads (well,Warps really) within a thread group get executed by the same shader unit. Theshader unit then simply has at least 32k (usually a bit more) of local memory.And because all grouped threads
 share the same shader unit (and hence the sameset of ALUs etc.), there’s no need to include complicated arbitration orsynchronization mechanisms for shared memory access: only one Warp can accessmemory in any given cycle, because only one Warp gets to issue
 instructions inany cycle! Now, of course this process will usually be pipelined, but thatdoesn’t change the basic invariant: per shader unit, we have exactly one pieceof TGSM; accessing TGSM might require multiple pipeline stages, but actualreads from (or
 writes to) TGSM will only happen inside one pipeline stage, andthe memory accesses during that cycle all come from within the same Warp.

However, this is not yet enough for actual shared-memory communication. Theproblem is simple: The above invariant guarantees that there’s only one set ofaccesses to TGSM per cycle even when we don’t add any interlocks to preventconcurrent access. This is nice
 since it makes the hardware simpler and faster.It does not guarantee that memory accesses happen in any particular order fromthe perspective of the shader program, however, since Warps can be scheduledmore or less randomly; it all depends on who is runnable
 (not waiting formemory access / texture read completion) at certain points in time. Somewhatmore subtle, precisely because the whole process is pipelined, it might takesome cycles for writes to TGSM to become “visible” to reads; this happens whenthe actual
 read and write operations to TGSM occur in different pipeline stages(or different phases of the same stage). So we still need some kind ofsynchronization mechanism. Enter barriers. There’s different types of barriers,but they’re composed of just three fundamental
 components:

1.&nbsp;&nbsp;&nbsp;&nbsp; _Group Synchronization_. A Group SynchronizationBarrier forces all threads inside the current group to reach the barrier beforeany of them may consume past it. Once a Warp reaches such a barrier, it will beflagged as non-runnable, same as if
 it was waiting for a memory or textureaccess to complete. Once the last Warp reaches the barrier, the remaining Warpswill be reactivated. This all happens at the Warp scheduling level; it addsadditional scheduling constraints, which may cause stalls, but there’s
 no needfor atomic memory transactions or anything like that; other than lostutilization at the micro level, this is a reasonably cheap operation.

2.&nbsp;&nbsp;&nbsp;&nbsp; _Group Memory Barriers_. Since all threads within agroup run on the same shader unit, this basically amounts to a pipeline flush,to ensure that all pending shared memory operations are completed. There’s noneed to synchronize with resources
 external to the current shader unit, whichmeans it’s again reasonably cheap.

3.&nbsp;&nbsp;&nbsp;&nbsp; _Device Memory Barriers_. This blocks all threadswithin a group until all memory accesses have completed – either direct orindirect (e.g. via texture samples). As explained earlier in this series,memory accesses and texture samples on GPUs
 have long latencies – think morethan 600, and often above 1000 cycles – so this kind of barrier will reallyhurt.

DX11 offers different types ofbarriers that combine several of the above components into one atomic unit; thesemantics should be obvious.

**Unordered Access Views**We’ve now dealt with CS input and learned a bitabout CS execution. But where do we put our output data? The answer has theunwieldy name “unordered access views”, or UAVs for short. An UAV seemssomewhat similar to render
 targets in Pixel Shaders (and UAVs can in fact beused in addition to render targets in Pixel Shaders), but there’s some veryimportant semantic differences:

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Most importantly, as the same suggests, access to UAVs is“unordered”, in the sense that the API does not guarantee accesses to becomevisible in any particular order. When rendering primitives, quads are guaranteedto be Z-tested, blended and written
 back in API order (as discussed in detailin&nbsp;[<span style="color:#336699">part 9 of this series</span>](http://fgiesen.wordpress.com/2011/07/12/a-trip-through-the-graphics-pipeline-2011-part-9/)), or at least produce
 the same results as if theywere – which takes substantial effort. UAVs make no such effort – UAV accesseshappen immediately as they’re encountered in the shader, which may be verydifferent from API order. They’re not_completely_&nbsp;unordered, though;while
 there’s no guaranteed order of operations within an API call, the API anddriver will still collaborate to make sure that perceived sequential orderingis preserved across API calls. Thus, if you have a complex Compute Shader (orPixel Shader) writing to an UAV
 immediately followed by a second (simpler) CSthat reads from the same underlying resource, the second CS will see thefinished results, never some partially-written output.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;UAVs support random access. A Pixel Shader can only writeto one location per render target – its corresponding pixel. The same PixelShader can write to arbitrary locations in whatever UAVs it has bound.

·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;UAVs support atomic operations. In the classic PixelPipeline, there’s no need; we guarantee there’s never any collisions anyway.But with the free-form execution provided by UAVs, different threads might betrying to access a piece of memory at the
 same time, and we needsynchronization mechanisms to deal with this.

So from a “CPU programmer”‘s point of view, UAVscorrespond to regular RAM in a shared-memory multiprocessing system; they’rewindows into memory. More interesting is the issue of atomic operations; thisis one area where current GPUs diverge considerably from
 CPU designs.

**Atomics**In current CPUs, most of the magic for shared memory processingis handled by the memory hierarchy (i.e. caches). To write to a piece ofmemory, the active core must first assert exclusive ownership of thecorresponding cache line. This
 is accomplished using what’s called a “cachecoherency protocol”, usually&nbsp;[<span style="color:#336699">MESI</span>](http://en.wikipedia.org/wiki/MESI_protocol)&nbsp;and descendants. Thedetails are tangential to this article;
 what matters is that because writing tomemory entails acquiring exclusive ownership, there’s never a risk of two coressimultaneously trying to write to the some location. In such a model, atomicoperations can be implemented by holding exclusive ownership for
 the durationof the operation; if we had exclusive ownership for the whole time, there’s nochance that someone else was trying to write to the same location while we wereperforming the atomic operation. Again, the actual details of this get hairypretty fast
 (especially as soon as things like paging, interrupts andexceptions get involved), but the 30000-feet-view will suffice for the purposesof this article.

In this type of model, atomic operations are performed using the regular CoreALUs and load/store units, and most of the “interesting” work happens in thecaches. The advantage is that atomic operations are (more or less) regularmemory accesses, albeit with some
 extra requirements. There’s a couple ofproblems, though: most importantly, the standard implementation of cachecoherency, “snooping”, requires that all agents in the protocol talk to eachother, which has serious scalability issues. There are ways around thisrestriction
 (mainly using so-called Directory-based Coherency protocols), butthey add additional complexity and latency to memory accesses. Another issue isthat all locks and memory transactions really happen at the cache line level;if two unrelated but frequently-updated
 variables share the same cache line, itcan end up “ping-ponging” between multiple cores, causing tons of coherencytransactions (and associated slowdown). This problem is called “false sharing”.Software can avoid it by making sure unrelated fields don’t fall
 into the samecache line; but on GPUs, neither the cache line size nor the memory layoutduring execution is known or controlled by the application, so this problemwould be more serious.

Current GPUs avoid this problem by structuring their memory hierarchydifferently. Instead of handling atomic operations inside the shader units(which again raises the “who owns which memory” issue), there’s dedicatedatomic units that directly talk to a shared
 lowest-level cache hierarchy.There’s only one such cache, so the issue of coherency doesn’t come up; eitherthe cache line is present in the cache (which means it’s current) or it isn’t(which means the copy in memory is current). Atomic operations consist of
 firstbringing the respective memory location into the cache (if it isn’t therealready), then performing the required read-modify-write operation directly onthe cache contents using a dedicated integer ALU on the atomic units. While anatomic unit is busy on
 a memory location, all other accesses to that locationwill stall. Since there’s multiple atomic units, it’s necessary to make surethey never try to access the same memory location at the same time; one easyway to accomplish this is to make each atomic unit
 “own” a certain set ofaddresses (statically – not dynamically as with cache line ownership). This isdone by computing the index of the responsible atomic unit as some hashfunction of the memory address to be accessed. (Note that I can’t confirm thisis how
 current GPUs do; I’ve found little detail on how the atomic units workin official docs).

If a shader unit wants to perform an atomic operation to a given memoryaddress, it first needs to determine which atomic unit is responsible, waituntil it is ready to accept new commands, and then submit the operation (andpotentially wait until it is finished
 if the result of the atomic operation isrequired). The atomic unit might only be processing one command at a time, orit might have a small FIFO of outstanding requests; and of course there’s allkinds of allocation and queuing details to get right so that atomic
 operationprocessing is reasonably fair so that shader units will always make progress.Again, I won’t go into further detail here.

One final remark is that, of course, outstanding atomic operations count as“device memory” accesses, same as memory/texture reads and UAV writes; shaderunits need to keep track of their outstanding atomic operations and make surethey’re finished when they hit
 device memory access barriers.

**Structured buffers and append/consume buffers**Unless I missed something,these two buffer types are the last CS-related features I haven’t talked aboutyet. And, well, from a hardware perspective, there’s not that much to talkabout, really. Structured
 buffers are more of a hint to the driver-internalshader compiler than anything else; they give the driver some hint as to howthey’re going to be used – namely, they consist of elements with a fixed stridethat are likely going to be accessed together – but
 they still compile down toregular memory accesses in the end. The structured buffer part may bias thedriver’s decision of their position and layout in memory, but it does not addany fundamentally new functionality to the model.

Append/consume buffers are similar; they could be implemented using theexisting atomic instructions. In fact, they kind of are, except theappend/consume pointers aren’t at an explicit location in the resource, they’reside-band data outside the resource that
 are accessed using special atomic instructions.(And similarly to structured buffers, the fact that their usage is declared asappend/consume buffer allows the driver to pick their location in memoryappropriately).

**Wrap-up.**And… that’s it. No more previews for the next part, this seriesis done :), though that doesn’t mean I’m done with it. I have somerestructuring and partial rewriting to do – these blog posts are raw andunproofed, and I intend to go over
 them and turn it into a single document. Inthe meantime, I’ll be writing about other stuff here. I’ll try to incorporatethe feedback I got so far – if there’s any other questions, corrections orcomments, now’s the time to tell me! I don’t want to nail down
 the ETA for thefinal cleaned-up version of this series, but I’ll try to get it down wellbefore the end of the year. We’ll see. Until then, thanks for reading!

&nbsp;

&nbsp;

            <div>
                作者：qwertyu1234 发表于2015/9/29 16:56:02 [原文链接](http://blog.csdn.net/qwertyu1234/article/details/48808653)
            </div>
            <div>
            阅读：38 评论：0 [查看评论](http://blog.csdn.net/qwertyu1234/article/details/48808653#comments)
            </div>