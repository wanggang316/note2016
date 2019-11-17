---
layout: post
title: CoreAnimation介绍
author: Gump Wang
---

最近在研究iOS动画，可是却找不到好的资料来学习，索性找来Core Animation的官方文档，一边翻译一边学习，如有不准确的地方，请参见[Core Animation Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html "Core Animation Programming Guide")。

###关于Core Animation
Core Animation是IOS和OS X平台上用来制作你的应用中的动画和其他视觉元素的图形渲染和动画库。Core Animation能帮你绘制每一帧动画。你要做的只是配置一些参数（比如开始点和结束点）并且告诉Core Animation开始。其余的工作就交给Core Animation，它会把实际的绘制工作交给主板上的显卡来加速渲染。这种自动的图形加速的结果是帧率较高和流畅的动画并且并不会给你的CPU带来负担或者减慢应用的速度。

如果您正在编写iOS应用程序，那么你正在使用Core Animation不管你知道与否。同样，如果你正在编写OS X应用程序，你可以毫不费力地使用Core Animation。Core Animation在AppKit和UIKit的下层并且紧密的与Cocoa和Cocoa Touch中的视图相结合。当然，Core Animation也能通过你的视图来暴露接口，让你能更细致的控制你的动画。


![Core Animation]({{ "/assets/img/pexels/CoreAnimation.jpg" | relative_url }})


你可能从来都不需要直接使用Core Animation，但是当你操作时你就会发现Core Animation在你的应用中扮演的是很基础的角色。

### Core Animation管理你应用的内容

Core Animation本身并不是一个绘画系统。它是在硬件上合成和操作你的应用的基础。这一基础的核心是能管理和操作你的内容的__layer对象__。一个layer能把你的内容转换成很容易被硬件操作的位图。在大多数应用中，layers被用于操作视图的一种方式，当然，你也可以根据你的需要单独创建layers。

-------

关联章节：“[Core Animation Basic](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/CoreAnimationBasics/CoreAnimationBasics.html#//apple_ref/doc/uid/TP40004514-CH2-SW3)”，“[Setting Up Layer Ojbect](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/SettingUpLayerObjects/SettingUpLayerObjects.html#//apple_ref/doc/uid/TP40004514-CH13-SW12)”

------

### Layer的改变触发动画

大多数通过Core Animation创建的动画都涉及修改layer的属性。像视图，layer对象都有能被修改的形状（bounds rectangle），位置(position)，透明度(opacity)，旋转(transform)和其他一些视觉导向的属性。对于这些属性中的大多数，改变属性从一个值到另一个值能创建一个隐式动画。你也可以明确的控制这些属性的值来达到你想要的动画效果。

-------

关联章节：“[Animating Layer Content](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/CreatingBasicAnimations/CreatingBasicAnimations.html#//apple_ref/doc/uid/TP40004514-CH3-SW1)”,“[Advanced Animation Tricks](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/AdvancedAnimationTricks/AdvancedAnimationTricks.html#//apple_ref/doc/uid/TP40004514-CH8-SW1)”，“[Layer Style Property Animation](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html)”，“[Animatable Properties](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW1)”

-------

### Layers能被组织成层次结构

Layers能被整理成层次机构来创建父子关系。layers的组织结构影响它们所管理的视觉内容，这在某种程度上类似于视图。一组layers的层次结构能作用在视图上从而反映出视图的层次结构。你同样可以在你的应用中添加单独添加有层次结构的layers来扩展能超出你的视图之外的视觉内容。

-------

关联章节：“[Building a Layer Hierarchy](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/BuildingaLayerHierarchy/BuildingaLayerHierarchy.html#//apple_ref/doc/uid/TP40004514-CH6-SW2)”

-----

### Action使你能改变Layer的默认行为

隐式Layer动画通过**action对象**来实现，action对象是一个实现了一个预定义接口的一般对象。Core Animation一般通过action对象结合layer来实现默认的动画。你可以创建自己的action对象来实现自定义动画或者其它行为，然后把你的action对象分配给layer属性。当layer的属性值改变的时候，Core Animation会通知action对象来执行它的动作。

--------

关联章节：“[Changing a Layer's Default Behavior](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/ReactingtoLayerChanges/ReactingtoLayerChanges.html#//apple_ref/doc/uid/TP40004514-CH7-SW1)”

--------

### 如何使用文档

这个文档值特意为那些需要更好的控制它们程序中的动画或者想要通过layers来提高绘画表现的开发者准备的。它同样提供了iOS和OS X中关于layers与views之间的整合知识。iOS和OS X中layers和views的知识是不同的，在你创建高效的动画之前，理解这一点是很重要的。

### 前提

你应该已经理解你的目标平台的视图结构，并且理解如何创建基于视图的动画。如果没有，你应该阅读一下文档之一：

* 对于iOS应用程序，你应该了解*View Programming Guide for iOS *中所描述的视图结构。

* 对于OS X应用程序，你应该了解*View Programming Guide*中所描述的视图结构。

### 另请参阅

通过Core Animation实现具体类型的动画的实例，请看*Core Animation Cookbook *。














