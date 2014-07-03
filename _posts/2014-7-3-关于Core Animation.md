---
layout: post
title: 关于Core Animation
---

{{ page.title }}
================

<p class="meta">3 July 2014 - wanggang</p>

> 最近在研究iOS动画，可是却找不到好的资料来学习，索性找来Core Animation的官方文档，一边翻译一边学习，如有不准确的地方，请参见[Core Animation Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html "Core Animation Programming Guide")。

###关于Core Animation
Core Animation是IOS和OS X平台上用来制作你的应用中的动画和其他视觉元素的图形渲染和动画库。Core Animation能帮你绘制每一帧动画。你要做的只是配置一些参数（比如开始点和结束点）并且告诉Core Animation开始。其余的工作就交给Core Animation，它会把实际的绘制工作交给主板上的显卡来加速渲染。这种自动的图形加速的结果是帧率较高和流畅的动画并且并不会给你的CPU带来负担或者减慢应用的速度。

如果您正在编写iOS应用程序，那么你正在使用Core Animation不管你知道与否。同样，如果你正在编写OS X应用程序，你可以毫不费力地使用Core Animation。Core Animation在AppKit和UIKit的下层并且紧密的与Cocoa和Cocoa Touch中的视图相结合。当然，Core Animation也能通过你的视图来暴露接口，让你能更细致的控制你的动画。


[a1]: ../images/2014-07-03.jpg  "Core Animation"

![Alt text][a1]

