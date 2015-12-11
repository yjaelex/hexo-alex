title: '[转][转] A Brief Overview Of Vulkan API'
tags: []
date: 2015-09-24 11:15:32
---

转一篇关于Vulkan的介绍性文章。

Vulkan据说标准快要出来了；根据目前笔者了解，其实很多很AMD mantle很像。等正式发布后，笔者准备再写一系列文章研究新标准。毕竟这个才是未来的方向！

另外这个网站有很多SIGGRAPH2015上关于Vulkan的文档：[&nbsp;http://nextgenapis.realtimerendering.com/](http://nextgenapis.realtimerendering.com/)

特别是AMD大牛<span style="font-family:Calibri,sans-serif; font-size:14.6667px">Graham&nbsp;</span><span class="GramE" style="font-family:Calibri,sans-serif; font-size:14.6667px">Sellers的PPT：</span>&nbsp;[&nbsp;<span class="GramE" style="font-family:Calibri,sans-serif; font-size:11pt; color:purple">_A_</span><span style="font-family:Calibri,sans-serif; font-size:11pt; color:purple">_&nbsp;Whirlwind
 Tour of&nbsp;<span class="SpellE">Vulkan</span>_</span>](http://nextgenapis.realtimerendering.com/presentations/2_Sellers_Vulkan.pptx)

# A Brief Overview Of Vulkan API

<div class="post_meta-wrapper post_header" style="margin:0px 0px 40px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; display:flex; color:rgb(48,48,48); font-family:'Proxima Nova',Arial,sans-serif; font-size:15px; line-height:20px">
<div class="post_avatar post_meta-avatar" style="margin:0px 15px 0px 0px; padding:0px; border:4px solid white; vertical-align:baseline; min-height:0px; min-width:0px; overflow:hidden; width:70px; height:70px">
![Headshot.jpg](http://assets.toptal.io/uploads/avatar/external_author_photo/90228/headshot.jpg-ab38402da7731dc317775eb08505bf43.jpg)</div>
<div class="post_meta is-full" style="">
<div class="post_meta-author" style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; letter-spacing:0.01em; font-size:14px; font-weight:500; text-transform:uppercase; line-height:19px">

BY&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; font-weight:600; color:rgb(57,118,203)">[NERMIN
 HAJDARBEGOVIC](http://www.toptal.com/blog)</span>&nbsp;- TECHNICAL EDITOR @&nbsp;[TOPTAL](http://www.toptal.com/)

</div>
<div class="post_meta-tags" style="margin:10px 0px 0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; font-size:15px">
[#Vulkan](http://www.toptal.com/blog#vulkan-posts)&nbsp;[#VulkanAPI](http://www.toptal.com/blog#vulkanapi-posts)&nbsp;[#API](http://www.toptal.com/blog#api-posts)&nbsp;[#Khronos](http://www.toptal.com/blog#khronos-posts)&nbsp;[#OpenGL](http://www.toptal.com/blog#opengl-posts)</div>
</div>
<div class="post_meta-extra" style="margin:10px 0px 0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; color:rgb(146,146,146)">
<div class="post_meta-extra_row" style="margin:0px 0px 0px 85px; padding:10px; border:1px solid rgb(242,242,242); vertical-align:baseline; min-height:0px; min-width:0px; position:relative">
<div class="post_meta-extra_icon" style="margin:0px 0px 0px -72px; padding:0px; border:1px solid rgb(242,242,242); vertical-align:baseline; min-height:0px; min-width:0px; left:0px; top:0px; width:42px; height:42px; line-height:42px; position:absolute; text-align:center">
<div class="post_meta-extra_icon_image is-share" style="margin:0px -1px 0px 0px; padding:0px; border:0px; vertical-align:middle; min-height:0px; min-width:0px; display:inline-block; width:20px; height:20px">
</div>
</div>
<div class="post_share" style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">

*   <iframe src="http://hn-button.herokuapp.com/?title=Introduction%20To%20Vulkan%20API%20%7C%20Toptal&amp;url=http%3A%2F%2Fwww.toptal.com%2Fapi-developers%2Fa-brief-overview-of-vulkan-api&amp;style=twitter&amp;count=horizontal" name="hn-button-ve3uiwe" id="hn-button-ve3uiwe" class="hn-button" data-title="Introduction To Vulkan API | Toptal" data-url="http://www.toptal.com/api-developers/a-brief-overview-of-vulkan-api" data-style="twitter" data-count="horizontal" title="Hacker News Button" height="20" width="67" frameborder="0" style="margin: 0px; padding: 0px; border-width: 0px; border-style: initial; vertical-align: baseline; min-height: 0px; min-width: 0px; box-sizing: border-box;"></iframe>
*   <div class="fb-like fb_iframe_widget" data-href="http://www.toptal.com/api-developers/a-brief-overview-of-vulkan-api" data-send="false" data-layout="button_count" data-width="450" data-show-faces="false" fb-xfbml-state="rendered" fb-iframe-plugin-query="app_id=565054136848482&amp;container_width=0&amp;href=http%3A%2F%2Fwww.toptal.com%2Fapi-developers%2Fa-brief-overview-of-vulkan-api&amp;layout=button_count&amp;locale=en_US&amp;sdk=joey&amp;send=false&amp;show_faces=false&amp;width=450" style="margin: 0px; padding: 0px; border: 0px; vertical-align: baseline; min-height: 0px; min-width: 0px; box-sizing: border-box; display: inline-block; position: inherit !important;"><span style="margin: 0px; padding: 0px; border: 0px; vertical-align: top; min-height: 0px; min-width: 0px; box-sizing: border-box; display: inline-block; position: inherit !important; text-align: justify; width: 0px; height: 0px; overflow: hidden;"></span><iframe name="f2b007cf4" width="450px" height="1000px" frameborder="0" allowtransparency="true" allowfullscreen="true" scrolling="no" title="fb:like Facebook Social Plugin" src="http://www.facebook.com/v2.0/plugins/like.php?app_id=565054136848482&amp;channel=http%3A%2F%2Fstatic.ak.facebook.com%2Fconnect%2Fxd_arbiter%2F6brUqVNoWO3.js%3Fversion%3D41%23cb%3Df195558b84%26domain%3Dwww.toptal.com%26origin%3Dhttp%253A%252F%252Fwww.toptal.com%252Ff346d82f4c%26relation%3Dparent.parent&amp;container_width=0&amp;href=http%3A%2F%2Fwww.toptal.com%2Fapi-developers%2Fa-brief-overview-of-vulkan-api&amp;layout=button_count&amp;locale=en_US&amp;sdk=joey&amp;send=false&amp;show_faces=false&amp;width=450" style="margin: 0px; padding: 0px; border-style: none; border-width: initial; vertical-align: baseline; min-height: 0px; min-width: 0px; box-sizing: border-box; position: absolute; visibility: visible; width: 0px; height: 0px;"></iframe>
</div>
<li class="social_share-item is-google_plus" style="margin:0px 10px 5px 0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; height:21px; max-width:130px; position:relative; top:2px">
<div id="___plus_0" style="margin:0px; padding:0px; border:0px none; vertical-align:baseline; min-height:0px; min-width:0px; float:none; line-height:normal; font-size:1px; display:inline-block; width:82px; height:20px; background:transparent">
<iframe frameborder="0" hspace="0" marginheight="0" marginwidth="0" scrolling="no" tabindex="0" vspace="0" width="100%" id="I0_1443064619698" name="I0_1443064619698" src="https://apis.google.com/u/0/se/0/_/+1/sharebutton?plusShare=true&amp;usegapi=1&amp;action=share&amp;annotation=bubble&amp;origin=http%3A%2F%2Fwww.toptal.com&amp;url=http%3A%2F%2Fwww.toptal.com%2Fapi-developers%2Fa-brief-overview-of-vulkan-api&amp;gsrc=3p&amp;ic=1&amp;jsh=m%3B%2F_%2Fscs%2Fapps-static%2F_%2Fjs%2Fk%3Doz.gapi.zh_CN.uui3cOgRPWE.O%2Fm%3D__features__%2Fam%3DAQ%2Frt%3Dj%2Fd%3D1%2Ft%3Dzcms%2Frs%3DAGLTcCOaJFyOPDf6PdAT0jE48ciuPJJ0LQ#_methods=onPlusOne%2C_ready%2C_close%2C_open%2C_resizeMe%2C_renderstart%2Concircled%2Cdrefresh%2Cerefresh%2Conload&amp;id=I0_1443064619698&amp;parent=http%3A%2F%2Fwww.toptal.com&amp;pfname=&amp;rpctoken=14208841" data-gapiattached="true" title="+分享" style="margin: 0px; padding: 0px; border-width: 0px; border-style: none; vertical-align: baseline; min-height: 0px; min-width: 0px; box-sizing: border-box; position: static; top: 0px; width: 82px; left: 0px; visibility: visible; height: 20px;"></iframe></div>
</li><li class="social_share-item is-linkedin" style="margin:0px 10px 5px 0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; height:21px; max-width:130px; position:relative; top:2px">
<span class="IN-widget" style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; line-height:1; display:inline-block; text-align:center"><span style="border:0px; min-height:0px; min-width:0px; margin:0px!important; padding:0px!important; vertical-align:baseline!important; display:inline-block!important; font-size:1px!important"><span id="li_ui_li_gen_1443064621938_0" style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; position:relative!important; overflow:visible!important; display:block!important"><a target="_blank" id="li_ui_li_gen_1443064621938_0-link" style="vertical-align:baseline; min-height:0px; min-width:0px; margin:0px!important; padding:0px!important; border:0px!important; height:20px!important; display:inline-block!important"><span id="li_ui_li_gen_1443064621938_0-logo" style="margin:0px!important; padding:0px!important; border-width:0px 1px 0px 0px!important; border-right-style:solid!important; border-right-color:rgb(6,96,148)!important; vertical-align:baseline; min-height:0px; min-width:0px; text-indent:-9999em!important; overflow:hidden!important; position:absolute!important; left:0px!important; top:0px!important; display:block!important; width:20px!important; height:20px!important; float:right!important; background-color:rgb(0,119,181)!important">in</span><span id="li_ui_li_gen_1443064621938_0-title" style="margin-top:0px; margin-right:0px; margin-bottom:0px; min-height:0px; min-width:0px; margin-left:1px!important; padding:0px 4px 0px 23px!important; border:1px solid rgb(0,119,181)!important; vertical-align:top!important; color:rgb(255,255,255)!important; display:block!important; white-space:nowrap!important; float:left!important; overflow:hidden!important; height:18px!important; line-height:20px!important; background-color:rgb(0,119,181)!important"><span id="li_ui_li_gen_1443064621938_0-mark" style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; display:inline-block!important; width:0px!important; overflow:hidden!important"></span><span id="li_ui_li_gen_1443064621938_0-title-text" style="">**Share**</span></span></a></span></span><span style="border:0px; min-height:0px; min-width:0px; margin:0px!important; padding:0px!important; vertical-align:baseline!important; display:inline-block!important; font-size:1px!important"><span id="li_ui_li_gen_1443064621955_1-container" class="IN-right" style="margin:0px; padding-top:0px; padding-right:0px; padding-bottom:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; padding-left:2px!important; display:inline-block!important; float:left!important; overflow:visible!important; position:relative!important; height:20px!important"><span id="li_ui_li_gen_1443064621955_1" class="IN-right" style="margin-top:0px; margin-right:4px!important; margin-bottom:0px; margin-left:0px; padding-top:0px; padding-right:4px!important; padding-bottom:0px; padding-left:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; display:block!important; float:left!important; height:20px!important; background-color:transparent!important"><span id="li_ui_li_gen_1443064621955_1-inner" class="IN-right" style="margin:0px; padding-top:0px; padding-right:0px; padding-bottom:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; padding-left:8px!important; display:block!important; float:left!important; background-color:transparent!important"><span id="li_ui_li_gen_1443064621955_1-content" class="IN-right" style="margin:0px; padding:0px 5px!important; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; display:inline!important; font-size:11px!important; color:rgb(78,78,78)!important; font-weight:bold!important; font-family:Arial,sans-serif!important; line-height:20px!important">37</span></span></span></span></span></span></li><li class="social_share-item is-twitter" style="margin:0px 10px 5px 0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; height:21px; max-width:130px; position:relative; top:2px; overflow:hidden; width:90px">
[](https://twitter.com/share)

</li></div>
</div>
</div>
<div></div>
<div class="content_body" style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; font-size:15px; color:rgb(48,48,48); font-family:'Proxima Nova',Arial,sans-serif; line-height:20px">

So, what’s the big deal with this new Vulkan API anyway, and why should we care?

Here’s the Vulkan API in a hundred words or less: It’s a low-overhead, close-to-metal API for 3D graphics and compute applications. Vulkan is basically a follow-on to OpenGL. It was originally referred to as the “next generation OpenGL initiative,” and it includes
 a few bits and pieces from AMD’s Mantle API. Vulkan is supposed to provide numerous advantages over other GPU APIs, enabling superior cross-platform support, better support for multithreaded processors, lower CPU load, and a pinch of OS agnosticism. It should
 also make driver development easier, and allow the pre-compilation of drivers, including the use of shaders written in various languages.

![Meet the new Vulkan API: Live Long And Render!](http://assets.toptal.io/uploads/blog/image/91572/toptal-blog-image-1439883331110-f300346b5fd77fec4b3bab653e40faf2.jpg)

<div class="pop_out_box is-full_width is-big" style="margin:0px 30px 15px 0px; padding:1em 1.5em; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; color:rgb(80,80,80); width:864px; line-height:1.5em; float:none; font-size:1.3em; overflow:hidden; background:rgb(250,250,250)">
Meet the new Vulkan API: Live Long And Render!</div>
<div class="tweet_this" style="margin:-10px 0px 1em; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; text-align:center">
[Tweet](https://twitter.com/share)</div>

That’s 93 words, so if you’re not interested, you can skip the next 3,500\. If, on the other hand, you want to know more about the upcoming graphics API that will be with us for years to come, I’ll get started on the basics.

## 
How Vulkan API Came Into Being

Vulkan was&nbsp;[originally
 announced](https://www.khronos.org/news/press/khronos-reveals-vulkan-api-for-high-efficiency-graphics-and-compute-on-gpus)&nbsp;by the Khronos Group in March 2015, with a tentative launch date set for late 2015\. In case you are not familiar with&nbsp;[Khronos](https://www.khronos.org/),
 it’s a non-profit industry consortium founded fifteen years ago by some of the biggest names in the graphics industry, including ATI (now a part of AMD), Nvidia, Intel, Silicon Graphics, Discrete, and Sun Microsystems. Even if you haven’t heard of Khronos,
 you’ve probably heard of some of their standards, such as: OpenGL, OpenGL ES, WebGL, OpenCL, SPIR, SYCL, WebCL, OpenVX, EGL, OpenMAX, OpenVG, OpenSL ES, StreamInput, COLLADA, and glTF.

By now, you’re probably thinking “Ah, it’s those guys,” so I can skip the rest of the intro and focus on the API itself.

Unlike its predecessor, or predecessors, Vulkan is designed from the ground up to run on diverse platforms, ranging from mobiles and tablets, to gaming consoles and high-end desktops. The underlying design of the API is layered, or should we say modular, so
 it enables the creation of a common, yet extensible architecture for code validation, debugging, and profiling, without impacting performance. Krhonos claims the layered approach will deliver a lot more flexibility, catalyse strong innovation in cross-vendor
 GPU tools, and provide more direct GPU control demanded by sophisticated game engines.

Now, I understand that a lot of technophiles have their reservations about marketing terms like “flexible,” “extensible,” and “modular,” but this time around we are dealing with the real McCoy. As a matter of fact, that’s the basic idea behind Vulkan: It’s
 envisioned as an API for the masses, from kids gaming on smartphones, to their parents designing buildings and games on workstations.

In theory, Vulkan could be used in parallel computing hardware, to control tens of billions of GPU cores, in tiny wearables and toy drones, in 3D printers, cars, VR kit, and just about anything else with a compatible GPU inside.

For more details, I suggest you take a look at the&nbsp;[official
 Vulkan overview in PDF](https://www.khronos.org/assets/uploads/developers/library/overview/vulkan-overview.pdf).

## 
AMD Mantle DNA

If the close-to-metal approach sounds eerily familiar, you may have been following AMD’s GPU announcements over the past two years or so. AMD surprised industry observers when it announced its Mantle API in 2013, and it surprised them once again when it decided
 to pull the plug on the API, announcing in March 2015 that it would not release Mantle 1.0 as a public SDK. In a nutshell, Mantle promised to deliver significant performance and efficiency improvements in some situations, especially on the CPU front since
 it would reduce processing overhead. It sounded like a good idea, as gamers could put together custom PCs with somewhat slower processors and invest more money in top notch graphics cards. It sounded very convenient for AMD, too, because the company hasn’t
 had a competitive high-end CPU in years, although it still has good GPU products.

As weeping AMD fanboys gathered to mourn the passing of their saviour, Mantle was miraculously resurrected. The good news came in the form of&nbsp;[a
 blog post](https://community.amd.com/community/gaming/blog/2015/05/12/on-apis-and-the-future-of-mantle), penned by AMD VP of Visual and Perceptual Computing, Raja Koduri. Coincidentally, in keeping with the religious theme, on one occasion Koduri held a sermon on a mount, during AMD’s Hawaii launch event in 2013, but I digress.

Joking aside, Koduri’s team did a good job. While Mantle didn’t become a new industry standard, it did become a foundation for Vulkan. The biggest difference is that Vulkan will not be restricted to AMD GCN hardware; it will work on a lot more GPUs from different
 vendors. You can probably see where I am going with this; it’s a bit better to have a single low-overhead graphics API that works on different operating systems and hardware platforms than to have proprietary APIs for different GPU architectures, OSes and
 so on.

![It sounds like a pun, but AMD](http://assets.toptal.io/uploads/blog/image/91570/toptal-blog-image-1439883331122-9edaae03955f378386cb965e42de8415.jpg)

<div class="pop_out_box is-full_width is-big" style="margin:0px 30px 15px 0px; padding:1em 1.5em; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; color:rgb(80,80,80); width:864px; line-height:1.5em; float:none; font-size:1.3em; overflow:hidden; background:rgb(250,250,250)">
It sounds like a pun, but AMD's Mantle is actually at the core of the new Vulkan API.</div>
<div class="tweet_this" style="margin:-10px 0px 1em; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; text-align:center">
[Tweet](https://twitter.com/share)</div>

Vulkan API simply takes a good chunk of the Mantle pie and shares it with everyone, regardless of OS, hardware, race or religion.

Oh, and there’s one more thing: Mantle eventually forced Microsoft and Khronos finally to do something about DirectX and OpenGL bloat and inefficiency. It was a gentle, friendly kick in the posterior, or “badonkadonk,” as one fellow Toptaler likes to put it.

## 
How Does Vulkan Compare To OpenGL?

Obviously, I need to outline the basic differences between Vulkan and OpenGL. Khronos came up with a simple illustration, showing how much driver bloat could be eliminated with the new API.

![Vulkan is a unified API for all platforms, and it enables simpler drivers as well.](http://assets.toptal.io/uploads/blog/image/91574/toptal-blog-image-1439883331118-79b1ee834f18ba761799a09c60c1278f.jpg)

<div class="pop_out_box is-full_width is-big" style="margin:0px 30px 15px 0px; padding:1em 1.5em; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; color:rgb(80,80,80); width:864px; line-height:1.5em; float:none; font-size:1.3em; overflow:hidden; background:rgb(250,250,250)">
Vulkan is a unified API for all platforms, and it enables simpler drivers as well.</div>
<div class="tweet_this" style="margin:-10px 0px 1em; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; text-align:center">
[Tweet](https://twitter.com/share)</div>

Vulkan allows applications to get closer to metal, thus eliminating the need for a lot of memory and error management, as well as a lot of shading language source. Drivers will be lighter, leaner and meaner. Vulkan will only rely on the SPIR-V intermediate
 language, and since it has a unified API for mobile, desktop and console markets, it should also get more tender, loving care from developers.

But wait, doesn’t this simply offload more work to&nbsp;[game developers](http://www.toptal.com/game)? Sure, they
 will be able to use hardware more efficiently, but what about their own man-hours? This is where the layered ecosystem approach enters the fray.

Developers will be able to choose three different levels, or tiers, of the Vulkan ecosystem.

*   Use Vulkan&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">directly</span>&nbsp;for maximum flexibility and control.
*   Use Vulkan&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">libraries and layers</span>&nbsp;to speed up development.
*   Use Vulkan via&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">off-the-shelf game engines</span>&nbsp;fully optimised over the new API.

The first option clearly won’t be for everyone, but I suspect it would make for some nice benchmarking software. Khronos expects the second option to be a “rich area for innovation” because many utilities and layers will be in open source, and will ease transition
 from OpenGL. If a publisher has some OpenGL titles that need tweaking and updating, this is what they would use.

The last option is, perhaps, the most tempting one because the heavy lifting has been done by industry heavyweights such as Unity, Oxide, Blizzard, Epic, EA, Valve and others.

Here is a quick OpenGL vs. Vulkan table:

<table class="tg  " style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; border-collapse:collapse; border-spacing:0px">
<tbody style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">
<tr style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">
<th class="tg-u227" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(187,218,255)">
OpenGL</th>
<th class="tg-mdm8" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(253,104,100)">
Vulkan</th>
</tr>
<tr style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">
<td class="tg-ci37" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(236,244,255)">
Originally created for graphics workstations with direct renderers, split memory.</td>
<td class="tg-oxek" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(253,232,231)">
A better match for modern platforms, including mobile platforms with unified memory and tiled rendering support.</td>
</tr>
<tr style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">
<td class="tg-ci37" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(236,244,255)">
Driver handles state validation, dependency tracking, error checking. This may limit and randomise performance.</td>
<td class="tg-oxek" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(253,232,231)">
The application has direct and predictable control over the GPU via an explicit API.</td>
</tr>
<tr style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">
<td class="tg-ci37" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(236,244,255)">
Obsolete threading model does not allow generation of graphics commands in parallel to command execution.</td>
<td class="tg-oxek" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(253,232,231)">
API designed for multi-core, multi-thread platforms. Multiple command buffers can be created in parallel.</td>
</tr>
<tr style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">
<td class="tg-ci37" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(236,244,255)">
API choices can be complex, syntax evolved over twenty years.</td>
<td class="tg-oxek" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(253,232,231)">
Removal of legacy requirements simplifies API design, simplifies usage guidance, reduces specification size.</td>
</tr>
<tr style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">
<td class="tg-ci37" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(236,244,255)">
Shader language compiler is a part of the driver, and it only supports GLSL. The shader source has to be shipped.</td>
<td class="tg-oxek" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(253,232,231)">
SPIR-V is the new compiler target, enabling front-end language flexibility and reliability.</td>
</tr>
<tr style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">
<td class="tg-ci37" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(236,244,255)">
Developers have to take into account implementation variability between vendors.</td>
<td class="tg-oxek" style="margin:0px; padding:10px 5px; border:1px solid; vertical-align:baseline; min-height:0px; min-width:0px; font-family:Arial,sans-serif; font-size:14px; overflow:hidden; word-break:normal; text-align:center; background-color:rgb(253,232,231)">
Due to the simpler API and common language front-ends, more rigorous testing will increase cross-vendor compatibility.</td>
</tr>
</tbody>
</table>

To be honest, I don’t think it is even fair to compare the two. Vulkan is a Mantle derivative, while OpenGL is a mastodon with 20 years’ worth of baggage. Vulkan is supposed to ditch loads of legacy stuff; that’s the whole point. Vulkan is supposed to streamline
 testing and implementation, make drivers leaner, and improve shader program portability via the SPIR-V intermediate language.

This brings us to the next question. What does Vulkan really mean for developers?

## 
SPIR-V Is Expected To Transform The Language Ecosystem

So where does SPIR-V come into play, and what happens to good old GLSL?

GSLS will stay alive for now, and it will be the first shading language supported by Vulkan. A GLSL to SPIR-V translator will do the heavy lifting, and voila!, you’ll get SPIR-V ready to feed the hungry Vulkan runtime. Game developers will be able to use SPIR-V
 and Vulkan back-ends, probably relying on open-sourced compiler front-ends. In addition to GLSL, Vulkan can support OpenCL C kernels, while work on adding support for C&#43;&#43; is progressing. Future domain-specific languages, frameworks and tools are another option.
 Khronos even mentions the possibility of developing new experimental languages.

![The SPIR-V language is the glue that will bind different platforms in Vulkan API.](http://assets.toptal.io/uploads/blog/image/91573/toptal-blog-image-1439883331119-10b2f295015537cb59608acc44a27a51.jpg)

<div class="pop_out_box is-full_width is-big" style="margin:0px 30px 15px 0px; padding:1em 1.5em; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; color:rgb(80,80,80); width:864px; line-height:1.5em; float:none; font-size:1.3em; overflow:hidden; background:rgb(250,250,250)">
The SPIR-V language is the glue that will bind different platforms in Vulkan API.</div>
<div class="tweet_this" style="margin:-10px 0px 1em; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; text-align:center">
[Tweet](https://twitter.com/share)</div>

Whatever developers choose to do, all roads lead to Vulkan, via SPIR-V, and then to a multitude of different devices.

SPIR-V is supposed to improve portability in three ways:

*   Shared tools
*   Single tool set for a single ISV
*   Simplicity

Since there will be no need for every hardware platform to feature a high-level language translator, developers will deal with less of them.

An individual ISV can generate SPIR-V using a single tool set, thus eliminating portability issues of the high-level language.

SPIR-V is simpler than a typical high-level language, making implementation and processing easier.

Performance will be improved in a number of ways, depending on how Vulkan is implemented:

*   No more compiler front-end, a lot of processing can be done offline
*   Optimisation passes can settle faster, optimisations executed offline
*   Multiple source shaders reduce to the same intermediate language instruction stream

Khronos does not specify any performance numbers and notes that “mileage will definitely vary.” It will all depend on how Vulkan is used. If you want to check out the gritty details, be sure to check out the&nbsp;[SPIR-V
 white paper](https://www.khronos.org/registry/spir-v/papers/WhitePaper.pdf).

<form >
<div class="embeddable_form-row is-label" style="margin:0px 0px 10px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; position:relative">
<div class="embeddable_form-label_title" style="margin:0px 0.5em 0px 0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; color:rgb(37,87,161); font-size:17px; font-weight:bold; display:inline-block; line-height:23px">
Like what you're reading?</div>
<div class="embeddable_form-label" style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; color:rgb(49,49,49); font-size:17px; line-height:23px; display:inline-block; font-style:italic">
Get the latest updates first.</div>
</div>
<div class="embeddable_form-row" style="margin:0px 0px 10px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; position:relative">
<input class="input is-medium" name="email" type="text" style=""></div>
<div class="embeddable_form-row_wrapper is-footer" style="">
<div class="embeddable_form-row is-submit" style=""><input class="button is-green_candy is-default is-full_width" type="submit" value="Get Exclusive Updates" style=""></div>
<div class="embeddable_form-row is-privacy" style="">
<div class="embeddable_form-privacy" style="">
<div class="embeddable_form-privacy_icon" style="margin:0px 5px 0px 0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; display:inline-block; width:9.5px; height:11.5px; position:relative; top:-2px">
</div>
<div class="embeddable_form-privacy_text" style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">
No spam. Just great engineering and design posts.</div>
</div>
</div>
</div>
</form>

## 
Vulkan Looks Promising From A Developer Perspective

I have outlined a number of features that should make Vulkan and SPIR-V popular in the dev community, and Khronos is keen to get this point across as well. The prospect of using the same tools and skills to develop for multiple platforms appears intriguing,
 especially now that the performance gap between various platforms is closing.

Of course, developing a big-budget AAA game for PCs will remain an extremely complex and time-consuming process, involving heaps of cash and human resources, but mobile platforms and integrated GPUs employed in the latest Intel and AMD processors already deliver
 a lot of GPU performance for the casual gamer. Besides, small, independent developers, or freelancers, are more likely to work on cross-platform casual games than AAA titles churned out by major publishers.

Khronos outlines the following advantages made possible by SPIR-V:

*   Developers can use the same front-end compiler across multiple platforms to eliminate cross-vendor portability issues
*   Runtime shader/kernel compilation time will be reduced since the driver only has to process SPIR-V
*   Developers do not have to distribute shader/kernel source code, so they enjoy an added level of IP protection
*   Drivers are simpler and more reliable since there is no need to include front-end compilers
*   Developers have a better picture of memory allocation and can tweak their memory allocation approach accordingly

I am sure you’ll agree that this sounds good, but there is still a long way to go.

## 
Vulkan: It Works, But It’s A Work In Progress

As I said, Vulkan is still pretty much a work in progress, and we should have the full spec by the end of the year. However, from what we have seen so far, the new API can unlock&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">a
 lot of</span>&nbsp;performance even with current-generation hardware.

The best illustration of Vulkan I’ve seen so far comes from Imagination Technologies, one of the leading mobile GPU outfits out there. Imagination Technologies GPU IP is used in all iOS gadgets, along with numerous other ARM-based System-on-Chip designs, and
 even in some low-voltage Intel x86 chips.

Last week Imagination published a&nbsp;[blog
 post](http://blog.imgtec.com/powervr/gnomes-per-second-in-vulkan-and-opengl-es)&nbsp;detailing the performance gains made possible by Vulkan. Its choice of hardware was somewhat unusual: a Google Nexus Player, based on a rarely used Intel quad-core processor with PowerVR G6430 GPU. The device was tested with the latest Vulkan API
 driver for PowerVR GPUs, while the reference run was performed on OpenGL ES 3.0\. The performance gap was nothing short of staggering.

[![Check out this Vulkan API demo: smooth gnomes vs. choppy gnomes](http://assets.toptal.io/uploads/blog/image/91575/toptal-blog-image-1439892627291-c39ec7004e4c20bb44601084e4b12152.jpg)](https://youtu.be/P_I8an8jXuM)

<div class="pop_out_box is-full_width is-big" style="margin:0px 30px 15px 0px; padding:1em 1.5em; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; color:rgb(80,80,80); width:864px; line-height:1.5em; float:none; font-size:1.3em; overflow:hidden; background:rgb(250,250,250)">
Check out this Vulkan API demo: smooth gnomes vs. choppy gnomes</div>
<div class="tweet_this" style="margin:-10px 0px 1em; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; text-align:center">
[Tweet](https://twitter.com/share)</div>

The scene includes a total of 400,000 objects, with different levels of detail, ranging from 13,000 to 300 vertices. The wide shot shows an estimated one million triangles, some alpha on the plants and about ten different textures for the gnomes and plants.
 Each object type uses a different shader and the gnomes are not instanced, each draw call could be an entirely different object, with different materials, but the end-result would be similar.

Still, there’s a big caveat: This is not the sort of performance boost you can expect in real life. The Imagination Technologies team used an exaggerated scenario to highlight Vulkan’s superiority, to push it to its limits, and in this particular scenario the
 limit is in favour of Vulkan vs. OpenGL ES. Also, keep in mind that&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">this test is GPU-bound</span>, but it is still a good illustration of Vulkan’s superior
 CPU utilisation.

## 
How Does Vulkan Reduce CPU Utilisation?

Remember that OpenGL vs. Vulkan table we had earlier, or to be more specific, that&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">tiled rendering</span>&nbsp;bit? Probably not, so here it is, in a nutshell:
 Imagination used Vulkan to batch draw calls into tiles and render a tile at a time. Depending on where the tile is at the moment the frame is rendered, it can come into or go out of view, change its level of detail, and so on. In OpenGL ES, all draw calls
 are dynamic, they are submitted with each frame, according to what is in the field of view. Draw calls that have already been executed cannot be cached.

As a result, OpenGL ES needs many calls into kernel mode to change the state of the driver and validate it. Vulkan does not because it relies on pre-generated commands (command buffers) to reduce CPU overhead and eliminate the need to validate or compile during
 the render loop. The Imagination team described the ability to re-use command buffers as “useful in some circumstances,” and possible to use “to a great extent” in many games and applications.

The second game changer is&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">parallel buffer generation</span>, which enables Vulkan to harness the power of all CPU cores. OpenGL ES was designed before
 the advent of multi-core mobile chips, but over the past three years, the industry has gone from two, through four, to eight and ten CPU cores, with Apple’s A-series SoCs and Denver-based Nvidia Tegra chips as the only notable exceptions. I talked about mobile
 SoC trends in one of my previous blog pieces, covering the upcoming&nbsp;[<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px">Optimizing</span>&nbsp;Android
 compiler](http://www.toptal.com/android/brace-yourselves-new-android-compiler-is-coming), so you can check it out for additional info.

Let’s try an analogy: If Vulkan was an internal combustion engine, it would be storing and reusing part of its power, in much the same way a turbocharger and intercooler would (command buffers), and it would be able to use four, six, eight or even ten cylinders
 with no loss in efficiency (parallel buffer generation). Comparing Vulkan to OpenGL ES sounds a bit like comparing a new, downsized turbo engine to an old, single-cylinder engine on your granddad’s Triumph Trophy.

Well, at least granddad was a proper rocker, not a mod.

The end result is a vastly more efficient environment, capable of putting all available hardware to good use, unlike OpenGL ES, which is CPU bound in most scenarios. This means Vulkan can deliver similar levels of performance while keeping the CPU at lower
 clocks, thus reducing power consumption and throttling.

## 
Potential Vulkan API Downsides (Spoiler Alert: There Aren’t That Many)

I am not nit-picking; I feel it’s also important to list the pros and cons of Vulkan API . Fortunately, there aren’t that many cons other than a few minor ones and, potentially, one or two big ones. If you think Vulkan is the best thing since sliced bread,
 and you’re eager to give it a go in your next project, you may want to consider a few of these points:

*   Added code complexity in certain scenarios
*   Time-to-Market
*   Level of industry support
*   Vulkan may not be as relevant or effective on some platforms (desktops)
*   Convincing developers to use Vulkan on some platforms
*   Limited compatibility with legacy hardware

If a&nbsp;[developer](http://www.toptal.com/game)&nbsp;wants to implement some of the neat features outlined in this
 post, it will involve a fair amount of work. Each will have to be implemented in code, but the good news is that industry leaders will make the process easier with new driver updates.

![There aren](http://assets.toptal.io/uploads/blog/image/91571/toptal-blog-image-1439883331121-f31a49359e772b648a949bc56c22d47f.jpg)

<div class="pop_out_box is-full_width is-big" style="margin:0px 30px 15px 0px; padding:1em 1.5em; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; color:rgb(80,80,80); width:864px; line-height:1.5em; float:none; font-size:1.3em; overflow:hidden; background:rgb(250,250,250)">
There aren't that many downsides to the Vulkan API, but it will take a while before we see it in action.</div>
<div class="tweet_this" style="margin:-10px 0px 1em; padding:0px; border:0px; vertical-align:baseline; min-height:0px; min-width:0px; text-align:center">
[Tweet](https://twitter.com/share)</div>

Time-to-market is another concern, as is the implementation of Vulkan in older apps and games. Vulkan is still a technical preview; initial specifications and implementations are expected by the end of 2015, so, realistically, we probably won’t see many real-world
 applications before mid-2016.

Industry support should not be an issue; After all, this is a Khronos standard, but it may take a while. That’s one of the reasons I focused this post on the mobile segment; Mobile software and hardware evolve more quickly, and it may take a few more quarters
 before we see Vulkan making an impact on desktop platforms. That’s just how the industry works, there is a lot more stuff to worry about in the desktop niche: support for professional applications, hordes of pitchfork-wielding gamers going ape over every torn
 frame, and so on. However, the fact that Vulkan is derived from AMD’s Mantle is encouraging.

While Vulkan can do wonders in a CPU-bound setting, especially with multi-core mobile SoCs, these performance gains will be limited on desktop platforms. Desktops handle multi-core processors with a greater level of efficiency, and most graphically demanding
 applications are GPU-bound.

Until all pieces of the puzzle fall into place, some developers may be reluctant to take the plunge and mess around with Vulkan. A lot of people simply don’t have time to experiment, and they learn new skills only when absolutely necessary. Burning a lot of
 money and wasting man-hours to tweak existing mobile games to use Vulkan at this early stage won’t be an option for many developers and publishers.

Compatibility with older hardware could be another source of concern. Vulkan will need OpenGL ES 3.1 or OpenGL 4.1 hardware, accompanied by new drivers. For example, Imagination Technologies’ PowerVR series 6 GPUs can support it, but series 5 cannot. Qualcomm’s
 Adreno 400 series supports OpenGL ES 3.1, but the 300 series does not. ARM’s Mali T600- and T700-series support OpenGL ES 3.1, but support is lacking on older T400-series designs. Luckily, by the time Vulkan becomes relevant, most devices with unsupported
 GPUs will be out of the picture. These include the iPhone 5/5C, fourth generation iPad and Samsung devices based on certain 5000-series Exynos chips. Qualcomm-based devices may not be as lucky since Adreno 300-series GPUs are used on relatively recent and
 prolific designs such as the Snapdragon 410, Snapdragon 600, Snapdragon 800 and 801\. However, I suspect most of them will be gone by the time Vulkan becomes truly relevant.

## 
Live Long And Render

It is still too early to say whether or not Vulkan will be a game-changer, but I think you will agree that it has plenty of potential. I think it will be a big deal, and I base that assumption on a decade of experience covering the GPU industry. It will take
 time, however, and I suspect Vulkan will make its presence felt in mobile before it starts changing the desktop landscape.

At about the same time Vulkan-optimised drivers, game engines, and games, we will get new hardware to play around with, and I don’t mean just minor hardware tweaks. Mobile SoC development has stalled for a number of reasons I won’t go into now, but 2016 will
 be a big year for the industry, as 14/16nm FinFET nodes become available to more manufacturers, and become economically viable for mainstream hardware rather than flagship chips.

Developers will have vastly more powerful and efficient hardware to play around with, and a new low-overhead graphics API will be the icing on the cake. I sincerely hope hardware vendors will stop using display resolution as a marketing gimmick, as pointlessly
 high resolutions do nothing for visual quality but still waste power. Unfortunately, since the average consumer doesn’t get this, and wants to see bigger numbers on the box, I suspect this won’t happen anytime soon. I intend to examine this weird issue in
 one of my upcoming posts, so if you’re annoyed by it, stay tuned and feel free to vent in the comment section.

</div>

            <div>
                作者：qwertyu1234 发表于2015/9/24 11:15:32 [原文链接](http://blog.csdn.net/qwertyu1234/article/details/48708559)
            </div>
            <div>
            阅读：36 评论：0 [查看评论](http://blog.csdn.net/qwertyu1234/article/details/48708559#comments)
            </div>