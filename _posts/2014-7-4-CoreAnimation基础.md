---
layout: post
title: CoreAnimation基础
author: Gump Wang
---

Core Animation为你的应用程序中动画视图和其它视觉元素提供了一个通用的系统。Core Animation不是你的应用程序中视图的替代品，它是一个与你的视图相结合，以提供更好的性能和支持内容动画的技术。它通过把视图内容超高速缓存到能够直接被图形硬件操作的位图来实现它的行为。在某些情况下，它的超高速缓存行为可能需要你重新思考怎样呈现和管理你应用程序中的内容，但是在很多时候，你在不知道这些东西的情况下使用Core Animation。除了超高速缓存，Core Animation还定义了一种方式来指定任意的视觉内容，使它与你的视图内容相结合，并且使它随着其他的一切动起来。

你可以使用Core Animation来更改你的应用程序的视图和可视化对象的动画。大多数改变与修改你的视觉对象的属性有关。举例来说，你可能使用Core Animation来改变一个视图的位置，大小或者透明度。当你做了这样的修改，Core Animation使它的值从一个值变到另一个你指定的新值。你通常不使用Core Animation来一分钟替换60次一个视图的内容，就像卡通漫画一样。取而代之，你会使用Core Animation在屏幕上移动视图的内容，渐入或者渐出，为视图指定任意的图形变化，或者改变视图的其它视觉属性。

### Layers提供绘图和动画的基础
**Layer对象**是组织在一个三维空间内的二维表面，是你做使用Core Animation做任何动画的核心。像视图那样，layers管理几何结构，内容和表面视觉属性的信息。与视图不同的是，layers没有定义自己的外表。一个层只管理位图有关的状态信息。位图本身可以是一个视图绘画本身的结果，或者是一个你自己定义的固定图像。由于这个原因，你用在你的应用程序中的主要层被认为是模型对象，因为它们主要用来管理数据。这个概念是很重要的，要记住，因为它会影响动画的行为。

### 基于层的绘画模型
大多数层在你的应用程序中没有做实际的绘画。取而代之，layer会捉取你的应用程序提供的内容并将其缓存到内存中的位图，有时也被称为**后备存储**。当你随后改变层的一个属性的时候，你正在做的是改变与layer对象相关联的状态信息。当一个改变触发一个动画的时候，Core Animation传递层的位图和状态信息到能够使用新信息的位图的图形硬件，如图1-1所示。在硬件上操作位图比在软件上产生更快的动画。

**图1-1** Core Animation如何绘制内容

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic1.jpg" | relative_url }})

因为它操作一个静态的位图，基于层的绘画与其它传统的基于视图的绘画技术有很显著的不同。基于视图的绘画，改变视图本身通常导致调用视图的**drawRect:**方法使用新的参数来重新绘制内容。但是用这种方法绘图开销是很大的，因为它是在主线程上通过CPU来绘图。Core Animation通过在任何情况下操作缓存在硬件上的位图来达到同样或者相似的效果来避免这样的开销。

尽管Core Animation尽可能使用缓存的内容，但是你的应用程序仍然必须提供初始化内容并且不时的更新。这里有许多不同的方式为你的应用程序提供包含内容的layer对象，这里是详细内容“[Providing a Layer's Contents](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/SettingUpLayerObjects/SettingUpLayerObjects.html#//apple_ref/doc/uid/TP40004514-CH13-SW4)”。

###基于层的动画
一个层对象的数据和状态信息是与层在屏幕上的视觉表现是分离的。这种分离给Core Animation一个干预自身的方式并且使它从一个状态值变化到一个新的状态值。举例来说，改变一个层的位置属性引发Core Animation来移动层从它当前位置到新的指定位置。类似的改变其它属性的值也能引起引发相应的动画。如图1-2举例说明了你可以在层上执行不同类型的动画。对于出发层动画的属性列表，参见“[Animatable Properties](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW1)”


**图1-2** 在层上执行的动画的例子

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic2.png" | relative_url }})

在动画的过程中，Core Animation在硬件上一帧一帧为你绘图。你要做的事指定动画的开始点和结束点，剩下的就交给Core Animation去做就行了。你也可以根据需要指定自定义的定时信息和动画参数；当然，如果你不那样做的话，Core Animation提供了适当的默认值。


对于更多的关于怎样开始动画并且配置动画的参数的信息，参见“[Animating Layer Content](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/CreatingBasicAnimations/CreatingBasicAnimations.html#//apple_ref/doc/uid/TP40004514-CH3-SW1)”

### 层对象定义自己的几何
一个层的其中一项工作是管理它的内容的视觉几何。视觉几何包含关于内容的边界信息，它在屏幕上的位置和层层是否别旋转，缩放或者任何方式的改变。就像视图那样，一个层有它的框架和矩形边界，你可以用它来定位层及其位置。层还有视图没有的其它属性，像锚点，能定义围绕操作发生的点。你可以指定层的几何形状的某些方面的方式也不同于你如何指定视图的信息。

### 层使用两种类型的坐标系统
层利用**点坐标系统**和**单元坐标**来指定内容的位置。使用哪个坐标取决于被表达的类型。点坐标被用于当指定的值直接对应到屏幕的坐标或者必须被相对于另一层指定的时候，比如层的*positon*属性。单位坐标不能被用于屏幕坐标的值，因为它是相对于其它值而言的。例如，该层的*anchorPoint*属性指定相对于所述层本身，它可以改变的边界的点。

一个层的边界和矩阵框的方向始终与底层平台的默认方向适应。如图1-3展示了在iOS和OS X上矩阵框的默认方向。在iOS中，矩阵框的起点默认在layer的左上角，但是在OS X中默认在左下角。如果你在iOS和OS X之间共享Core Animation的代码，那么你必须把这样的不同点考虑在内。

**图1-3** iOS和OS X上layer的默认几何形状

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic3.png" | relative_url }})

有一点要注意在[图1-3](a3)是*position*属性是位于一层的中间。这个属性是描述基于层的*anchorPoint*属性值的变化之一。锚点代表从确定的坐标原点开始的点，更多详细信息在“[Anchor Points Affect Genometric Manipulations](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/CoreAnimationBasics/CoreAnimationBasics.html#//apple_ref/doc/uid/TP40004514-CH2-SW17)”

锚点是你指定使用的单元坐标系统的几个属性之一。Core Animation使用单元坐标来表示那些当层的大小变化的时候值可能会变化属性。你可以认为单元坐标时指定所有可能值的百分比。在单元坐标中的每一个坐标空间有一个*0.0到－*1.0的范围。例如，x坐标上，左边界坐标在0.0，右边界坐标在1.0。在y坐标上，单元坐标值的变化取决于平台，如图1-4所示。

**图1-4** iOS和OS X上默认的单元坐标系统

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic4.png" | relative_url }})

-----

**注意：** 到OS X10.8，在需要的时候，geometryFlipped属性是改变一个层的y轴的默认方向的一种方式。当翻转变换被调用的时候使用整个属性来改变一个层的方向有时候是必要的。例如，如果父视图使用了翻转变换，它的子视图的内容（和它们对应的layer）往往会被翻转。在这种情况下，设置子layer的geometryFlipped为YES是来解决这个问题很简单的方式。在OS X 10.8以后AppKit为你管理这个属性并且你可以修改它。在iOS应用程序中，建议您不要使用geometryFlipped属性。

--------

所有坐标的值，无论他们是点还是坐标单元都被为浮点数。使用浮点数允许你指定可能正常的坐标值下降的精确位置。使用浮点数是很方便的，特别是在一个点可能代表多个像素的Retina屏幕上打印或者绘画的时候。浮点数允许你忽略底层设备的分辨率，只在你需要的时候精确地指定值。

### 锚点影响几何操作
一个层的几何形状有关的操作发生相对于该层的锚点，你可以使用该层的anchorPoint属性访问。当操作层的*position*或*transform*属性的时候，锚点的影响是显而易见的。位置属性总是相对于层的锚点来指定的，并且你在层上任何变换的发生都是相对于锚点的。

图1-5展示了改变锚点从一个值到另一个不同的值怎样影响层的*position*属性。尽管层相对于父bounds并没有移动，但是移动锚点从层的中心到层的起点改变了层的*position*属性。

**图1-5** 锚点如何影响层的位置属性

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic5.png" | relative_url }})


图1-6展示了改变锚点怎样影响层的旋转。当你讲层的角度旋转，那么旋转围绕着锚点。因为锚点默认被设置在层的中间，这通常能建立你期望的旋转行为。不管怎样，如果你改变锚点，那么旋转的的结果是不同的。

**图1-6** 锚点如何影响层的变换

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic6.jpg" | relative_url }})

### 层能在三维空间进行操作
每个层有两个变换矩阵，你可以用它来操作层及其内容。*CALayer*的*transform*指定了你想要的层的两种变换并且包含他们的子层。当你想要修改层本身通常可以使用此属性。例如，你可能需要使此属性来暂时缩放、旋转层或者改变层的位置。*sublayerTransform*属性定义了只针对子层的额外的变换，它最常用于为场景的内容添加透视效果。

通过操作坐标值变换的工作通过数字矩阵取得代表的原始点变换后的版本新坐标。因为Core Animation的值能在三维空间内被指定，每个坐标点有必需通过一个4*4矩阵相乘的4个值，如图1-7所示。在Core Animation中，在该图中的变换通过CATransform3D来表示。幸运的是，你不需要修直接改这种结构的字段来执行标准转换。Core Animation提供了一全套的用于创建缩放，平移和旋转矩阵并且比较矩阵的功能。除了使用功能操作变换，Core Animation扩展了对key-value的支持，允许你通过一个key paths来修改一个变换。有关可以修改key paths的属性列表，参见“[CATransform3D Key Paths](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Key-ValueCodingExtensions/Key-ValueCodingExtensions.html#//apple_ref/doc/uid/TP40004514-CH12-SW1)”

**图1-7** 用矩阵数学矩阵转换坐标

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic7.png" | relative_url }})

图1-8展示了一些较常见的转换的矩阵结构。任何坐标乘以恒等矩阵都能得到完全一样的坐标。关于其它变换，坐标怎样被改完全决定于其基质成分的改变。

举例来说，改变x轴，你会为平移矩阵的*tx*组成部分提供一个非零值，*ty*和*tz*的值设置为0。关于旋转，你将提供给目标旋转对象适当的正弦值和余弦值。

**图1-8** 常见的举证变换结构

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic8.png" | relative_url }})

关于你用来创建和操作转换的功能的信息，参见*Core Animation Function Reference*。

### 层的树形结构反应了动画状态的不同的方面
使用Core Animation的一个应用程序有三套层的对象。每个层对象的拥有使您的应用程序的内容出现在屏幕上不同的角色：
* **model layer tree**(或直接叫“layer tree”)中的对象是与你的应用程序交流最多的。这些在树形结构里的对象是能存储任何动画目标值的模型对象。无论何时你改变一个层的属性，都会用到这些对象中的一个。

* **presentation tree**中的对象为正在运行的动画包含在动态的值。鉴于这个树形层对象包含动画目标值，这些对象在presentation tree上反映了当前在屏幕上显示的值。你不应该修改这个树的对象。取而代之，你应该使用这些对象来读取当前动画的值，可能用那些值来创建新的动画。

* **render tree**中的对象执行实际的动画，它是Core Animation私有的。

每种层对象的集合被分层次地组织就像你应用中的视图。事实上，对于一个应用程序，所有视图的层，其初始化的树形结构和视图的组织结构完全匹配。然而，一个应用程序能添加额外的层对象，就是说，可以使用一个与视图没有关联的层。你可以在某种情况下这样做，以优化不需要一个视图的所有开销内容，来完善你的应用程序。如图1-9所示在一个简单的iOS应用程序中发现层的故障。这个示例的窗口包含一个内容视图，它本身包含一个button视图和两个独立的层对象。每个视图有一个与之对应的层对象，形成有层次的层结构的一部分。

**图1-9** 与窗口关联的层

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic9.png" | relative_url }})

对于layer tree上的对象，有一个在presentation tree和render tree上对应的对象。就像在图1-10所示的那样。像之前提到过的，应用程序主要工作在layer tree，但是有时可能会访问presentation tree中的对象。特别说明的是，访问layer tree中的对象的*presentationLayer*属性返回的雨在presentation tree中的对象一致。你可能想要访问那个对象来读取当前在动画中间一个属性的值。

图1-10 对于窗口的层结构

![Core Animation]({{ "/assets/img/pexels/CoreAnimationBasic10.png" | relative_url }})

--------

**重要提醒：** 在动画运行的时候你应该存取presentation tree中的对象。当动画在进程中的时候，presentation tree包含在屏幕上呈现的瞬间的值。这种行为与能够反映你在你的代码中设置的最后得值和动画的最后的状态的layer tree有区别。

--------


### 层和视图的关系
层不是你程序中视图的替代品－就是说，你不能创建一个仅仅基于层的可见的视图。层提供视图的基础。需要特别说明的是，层使绘画变的更容易和更有效率，当你这样做的时候，它能使你的视图的内容动起来并且能维持很高的绘画速度。然而，有很多层做不到的事。层不能处理事件，绘制内容，参与相应链，或者做其他事情。由于这个原因，每一个应用程序必须有一个或多个视图来处理这些类型的交互作用。

在iOS里，每一个视图都依赖一个层对象，但是在OS X里，你必须决定哪个视图应该有层。在OS X10.8以后，在你的所有视图上添加层可能是有意义的。然而，你不是必须要这样做，你可以在你开销不够或者不允许的情况下禁用层。层稍微增加了你内存的开销，但是好处比坏处多，所以在你禁用你的层之前它始终是测试你的应用程序的表现最好的选择。

当你启用了一个视图的层支持，你创建了一个**layer-backed view（层支持的视图）**。在一个层支持的视图中，系统为创建的层对象和保持层与视图的同步是负责。所有iOS视图和大多数OS X视图都是层支持的。然而，你也可以创建**layer-hosting view（层托管视图）**，它是一个你可以自己提供层对象的视图。对于一个层托管视图，AppKit带来了不干涉的方式来管理层，并且在视图改变的时候不修改它。

----------

**注意：**对于层支持视图，无论何时，操作视图是被推荐的，而不是他的层。在iOS中，视图只是层对象的很薄的包装，所以你对层的任何操作通常工作得很好。但是iOS和OS X有许多情况下，操作层而不是视图可能并不是期望的结果。只要有可能，这个文档指出了容易犯的错误并且视图提供帮助你围绕它们工作的方式。

---------

除层与视图的关联外，你也可以创建没有视图关联的层对象。你可以插入独立的层对象代替你的应用程序中任何层对象，包括哪些与视图有关联的。您通常使用独立的层对象作为一个具体的优化路径的一部分。例如，如果你想要在多个地方使用同样的图片，你可以加载一次图片，然后与多个独立的层对象联系在一起并且把那些对象加入到layer tree。然后每一个层引用这个图片资源而不是在内存中创建它自己的拷贝。

关于怎样在你的应用程序中启用层支持的有关信息，参见“[Enabling Core Animation Support in Your App](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/SettingUpLayerObjects/SettingUpLayerObjects.html#//apple_ref/doc/uid/TP40004514-CH13-SW5)”。怎样创建层的等级结构的有关信息，和你使用的时候的一些技巧，参见“[Building a Layer Hierarchy](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/BuildingaLayerHierarchy/BuildingaLayerHierarchy.html#//apple_ref/doc/uid/TP40004514-CH6-SW2)”






