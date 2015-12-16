title: '[原]AMD Mantle API 学习笔记 -- Mantle初始化'
date: 2015-09-09 21:50:24
tags: [Mantle,Graphics]
---

本系列文章是笔者研究mantle的一些心得；其目的是为了学习新的图形API标准Vulkan。因为Vulkan还没有正式发布，而它事实上是基于mantle的，所以研究mantle可以让我们对新一代图形标准（Vulkan和D3D12）有一个提前认识。在Vulkan正式发布后，笔者也会写一系列的文章来介绍Vulkan。事实上，mantle的API函数都是以gr开头的；而Vulkan很多API只是简单的替换为vk开头而已。这进一步说明学习mantle的价值。

要学习一个新的API，最好就是用它来写个简单的demo。国外有位牛人已经写了个mantle版的Hello World：[Implementing Hello Triangle with Mantle](https://medium.com/@Overv/implementing-hello-triangle-in-mantle-4302450fbcd2)。这个例子的代码在：[MantleHelloTriangle](https://github.com/Overv/MantleHelloTriangle)。笔者接下来的文章都是基于这个例子，试着把mantle的一系列基本概念剖析一下。

* * *

初始化Mantle
---------

使用mantle首先要初始化；主要是调用“grInitAndEnumerateGpus()”来获取系统中所有GPU的handle，然后调用”grGetGpuInfo()”得到每个物理GPU的属性。另外还可以获取`GPU_PERFORMANCE`的特性，这样Apps可以根据这些信息来选择使用哪个GPU。

    GR_CHAR appName[] = "HelloWorld";
    GR_APPLICATION_INFO appInfo = {};
    appInfo.pAppName = appName;
    appInfo.pEngineName = appName;
    appInfo.apiVersion = GR_API_VERSION;

    // initialize and enumerate all Mantle enabled GPUs
    GR_RESULT result = grInitAndEnumerateGpus(&appInfo, NULL, &m_gpuCount, &m_gpus[0]);
    MANTLE_ASSERT(result);

    if(result == GR_SUCCESS)
    {
        // retrieve the GPU information for all GPUs
        for(GR_UINT32 gpu = 0; gpu < m_gpuCount; ++gpu)
        {
            GR_SIZE gpuInfoSize = 0;
            GR_PHYSICAL_GPU_PROPERTIES gpuInfo = {};
            // first query the size of the gpuInfo
            result = grGetGpuInfo(m_gpus[gpu], GR_INFO_TYPE_PHYSICAL_GPU_PROPERTIES, &gpuInfoSize, NULL);
            MANTLE_ASSERT(result);
            // retrieve the GPU physical properties
            result = grGetGpuInfo(m_gpus[gpu], GR_INFO_TYPE_PHYSICAL_GPU_PROPERTIES, &gpuInfoSize, &gpuInfo);
            MANTLE_ASSERT(result);
        }
    }

Device和Queue
------------

选取GPU后就可以在此GPU上创建Device了。Device代表了一个在某GPU上运行的上下文（execution context）；这个概念与D3D里的ID3DDevice对象类似。

    if(result == GR_SUCCESS)
    {
        // use the WSI_WINDOWS extension
        // required to output results in a window on Windows
        const GR_CHAR *extensions[] =
        {
            "GR_WSI_WINDOWS"
        };

        // 先检查extension是否支持
        for(int ext = 0; ext < sizeof(extensions) / sizeof(extensions[0]); ++ext)
        {
            result = grGetExtensionSupport(gpus[0], extensions[ext]);
        }

        // only use one universal queue
        GR_DEVICE_QUEUE_CREATE_INFO dqinfo = {};
        dqinfo.queueCount = 1;
        dqinfo.queueType = GR_QUEUE_UNIVERSAL;

        //device creation info
        GR_DEVICE_CREATE_INFO info = {};
        info.queueRecordCount = 1;
        info.pRequestedQueues = &dqinfo;
        info.extensionCount = sizeof(extensions) / sizeof(GR_CHAR *);
        info.ppEnabledExtensionNames = extensions;

        // 正式版本默认无需任何validation
        // 调试时可以打开validation并且设置为break on error模式
    #ifdef _DEBUG
        info.flags = GR_DEVICE_CREATE_VALIDATION;
        info.maxValidationLevel = GR_VALIDATION_LEVEL_4;
        GR_BOOL grTrue = GR_TRUE;
        result = grDbgSetGlobalOption(GR_DBG_OPTION_BREAK_ON_ERROR, sizeof(grTrue), &grTrue);
    #else
        info.maxValidationLevel = GR_VALIDATION_LEVEL_0;
    #endif
        // create the device
        result = grCreateDevice(gpus[0], &info, &device);
    }

在这里我们在默认gpu0上创建了一个device；其主要需要的参数是要支持的extension，需要的queue以及validation的级别。 
在此我们有必要理一下物理显卡（adaptor），GPU，Screen/Display，Device和Queue之间的关系；这种基本概念之间的联系在以后的标准Vulkan里也是适用的。
![Mantle-Display-Screen][1]
这里我们只用到了一个extension：WSI_WINDOWS。这是为了获取相应的Display对象，从而将渲染结果Swap到屏幕上去显示。 
有了Device后，我们就可以得到Queue了。Mantle的设计模式是应用程序生成并填写Cmd Buffers；然后再将Cmd Buffers submit到相应的Queue中去（Queue和Engine对应）。后面笔者将专门写一编关于Queue的文章。目前我们只要关心大致上Mantle支持三种Queue：Universal（Gfx和Compute）、Compute（通用计算，可以异步与渲染Queue）和DMA。

    // create the universal queue
    result = grGetDeviceQueue(m_device, GR_QUEUE_UNIVERSAL, 0, &m_universalQueue);
   
到此为止，mantle初始化完成，device和queue也都准备好了。

  [1]: http://img.blog.csdn.net/20150913173840443