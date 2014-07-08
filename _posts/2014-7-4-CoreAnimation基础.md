---
layout: post
title: CoreAnimation基础
---

{{ page.title }}
================

<p class="meta">4 July 2014 - wanggang</p>


Core Animation为你的应用程序中动画视图和其它视觉元素提供了一个通用的系统。Core Animation不是你的应用程序中视图的替代品，它是一个与你的视图相结合，以提供更好的性能和支持内容动画的技术。它通过把视图内容超高速缓存到能够直接被图形硬件操作的位图来实现它的行为。在某些情况下，它的超高速缓存行为可能需要你重新思考怎样呈现和管理你应用程序中的内容，但是在很多时候，你在不知道这些东西的情况下使用Core Animation。除了超高速缓存，Core Animation还定义了一种方式来指定任意的视觉内容，使它与你的视图内容相结合，并且使它随着其他的一切动起来。

你可以使用Core Animation来更改你的应用程序的视图和可视化对象的动画。大多数改变与修改你的视觉对象的属性有关。举例来说，你可能使用Core Animation来改变一个视图的位置，大小或者透明度。当你做了这样的修改，Core Animation使它的值从一个值变到另一个你指定的新值。你通常不使用Core Animation来一分钟替换60次一个视图的内容，就像卡通漫画一样。取而代之，你会使用Core Animation在屏幕上移动视图的内容，渐入或者渐出，为视图指定任意的图形变化，或者改变视图的其它视觉属性。

### Layers提供绘图和动画的基础
**Layer对象**是组织在一个三维空间内的二维表面，是你做使用Core Animation做任何动画的核心。像视图那样，layers管理几何结构，内容和表面视觉属性的信息。与视图不同的是，layers没有定义自己的外表。一个层只管理位图有关的状态信息。位图本身可以是一个视图绘画本身的结果，或者是一个你自己定义的固定图像。由于这个原因，你用在你的应用程序中的主要层被认为是模型对象，因为它们主要用来管理数据。这个概念是很重要的，要记住，因为它会影响动画的行为。

### 基于层的绘画模型
大多数层在你的应用程序中没有做实际的绘画。取而代之，layer会捉取你的应用程序提供的内容并将其缓存到内存中的位图，有时也被称为**后备存储**。当你随后改变层的一个属性的时候，你正在做的是改变与layer对象相关联的状态信息。当一个改变触发一个动画的时候，Core Animation传递层的位图和状态信息到能够使用新信息的位图的图形硬件，如图1-1所示。在硬件上操作位图比在软件上产生更快的动画。

图1-1    Core Animation如何绘制内容

[a1]: /images/CoreAnimationBasic1.jpg "Core Animation"
![Alt text][a1]


因为它操作一个静态的位图，基于层的绘画与其它传统的基于视图的绘画技术有很显著的不同。基于视图的绘画，改变视图本身通常导致调用视图的**drawRect:**方法使用新的参数来重新绘制内容。但是用这种方法绘图开销是很大的，因为它是在主线程上通过CPU来绘图。Core Animation通过在任何情况下操作缓存在硬件上的位图来达到同样或者相似的效果来避免这样的开销。

尽管Core Animation尽可能使用缓存的内容，但是你的应用程序仍然必须提供初始化内容并且不时的更新。这里有许多不同的方式为你的应用程序提供包含内容的layer对象，这里是详细内容“[Providing a Layer's Contents](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/SettingUpLayerObjects/SettingUpLayerObjects.html#//apple_ref/doc/uid/TP40004514-CH13-SW4)”。

###基于层的动画
一个层对象的数据和状态信息是与层在屏幕上的视觉表现是分离的。这种分离给Core Animation一个干预自身的方式并且使它从一个状态值变化到一个新的状态值。举例来说，改变一个层的位置属性引发Core Animation来移动层从它当前位置到新的指定位置。类似的改变其它属性的值也能引起引发相应的动画。如图1-2举例说明了你可以在层上执行不同类型的动画。对于出发层动画的属性列表，参见“[Animatable Properties](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW1)”

**图1-2**    在层上执行的动画的例子
[a2]: /images/CoreAnimationBasic2.png "CoreAnimation"
![Alt text][a2]

在动画的过程中，Core Animation在硬件上一帧一帧为你绘图。你要做的事指定动画的开始点和结束点，剩下的就交给Core Animation去做就行了。你也可以根据需要指定自定义的定时信息和动画参数；当然，如果你不那样做的话，Core Animation提供了适当的默认值。

对于更多的关于怎样开始动画并且配置动画的参数的信息，参见“[Animating Layer Content](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/CreatingBasicAnimations/CreatingBasicAnimations.html#//apple_ref/doc/uid/TP40004514-CH3-SW1)”

### 层对象定义自己的几何
一个层的其中一项工作是管理它的内容的视觉几何。视觉几何包含关于内容的边界信息，它在屏幕上的位置和层层是否别旋转，缩放或者任何方式的改变。就像视图那样，一个层有它的框架和矩形边界，你可以用它来定位层及其位置。层还有视图没有的其它属性，像锚点，能定义围绕操作发生的点。你可以指定层的几何形状的某些方面的方式也不同于你如何指定视图的信息。

### 层使用两种类型的坐标系统

























