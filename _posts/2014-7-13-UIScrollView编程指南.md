---
layout: post
title: UIScrollView编程指南
---
{{ page.title }}
================

<p class="meta">13 July 2014 - wanggang</p>

## 关于Scroll View设计
在iOS程序中当内容需要被显示并且在屏幕上操作不合适的时候，滑动视图被创建。滑动视图有两个主要的目标：

* 让用户拖拽他们想要在屏幕上显示的内容
* 让用户使用手势放大或者缩小想在屏幕上显示的内容

下面的图形是*UIScrollView*的典型应用。子视图是一个包含有一个男孩图片的的*UIImageView*。当用户在屏幕上拖动他／她的手指，视图窗口上的图片会跟着移动，正如你在示意图中看到的一样，滑动指示器（scroll indicators）随着移动被显示出来。当用户的手指离开，滑动指示器消失。

### 看一眼
*UIScrollView*类提供给以下功能：
* 滑动的内容不完全适合屏幕
* 缩放内容，允许你的应用程序支持标准的缩放手势来放大或缩小
* 限制内容在一个屏幕上依次滑动

*UIScrollView*类没有为他所显示的内容特别定义视图；而是简单地滚动它的字视图。这个简单的模型是合适的，因为iOS中的滑动视图并没有用于初始化滚动的额外的控制器。

###	 基础的视图滚动很容易实现
通过拖动或者轻击手势不要求有基类或者委托。除此可以在*UIScrollView实例中设置内容尺寸，整个界面的的创建和设计也可以在Interface Builder中完成。

----

**关联章节：**“[Creating and Configuring Scroll Views]()”

---

### 为了支持缩放手势，使用委托
在滑动视图中添加基本的放大缩小手势的支持是使用代理。代理类必须遵从*UIScrollViewDelegate* 协议并实现代理方法，代理方法详细说明了那个滑动视图的子类应该被缩放。您还必须指定一个最小和最大放大倍率因素，或两者兼而有之。

如果你的应用程序需要支持双击缩放，两个手指放大，或者单一手触摸滑动，你需要在你的内容视图中实现来操作这些功能。

---

**关联章节：**“[Basic Zooming Using the Pinch Gestures]()”

----

### 为了支持手指开合来缩放或者点击放大，实现代码写在内容视图里
如果你的应用程序支持双击放大，两个手指放大，或者单一手触摸滑动，你需要在你的内容视图中来实现。

----

**关联章节：**“[Zooming by Tapping]()”

---

### 为为了支持分页模式，你只需要三个子类
为了支持分页模式，没有子类和代理是必须的。你只需指定内容尺寸并开启分页模式。你可以只使用三个字视图来实现大多数的分页程序，从而节省内存空间和提高效率。

------

**关联章节：**“[Scrolling Using Paging Mode]()”

------

### 要求
在阅读这个指南之前，阅读*iOS App Programming Guide*来iOS应用程序的基本开发流程。也可以考虑阅读* View Controller Programming Guide for iOS*来了解关于view controllers的基本信息，这些是经常与滑动视图结合使用的。

### 怎样使用这个文档
本指南中剩余的章节会知道你完成月来越复杂的任务，比如处理点击缩放技术，理解代理的作用和它的消息队列，并在你的应用程序中嵌套滚动视图。

### 另请参考
你会发现下面的实例工程对你的表视图的实现是有益的：

* *Scrolling*演示基本的滚动
* *PageControl*演示使用分页模式的滑动视图
*  *ScrollViewSuite*的实例工程。这里有一些用于演示点击滑动技术的高级实例，除此之外，其它一些典型的高级实例，包括切片来允许更大的，更详细的图片以在内存中以高效的方式被显示。

## 创建并配置滑动视图
滑动视图像其它任何视图一样被创建，要么通过编程的方式要么通过Interface Builder。只有少数的额外是必须的以实现基本的滑动。

### 创建滑动视图
一个滑动视图像其它任何视图一样被创建并插入到一个控制器或视图层次结构。要完成滚动视图的配置，只有两个额外的步骤是必须的。

1. 你必须通过*contentSize*属性设置滚动内容的大小。这个指定了可滚动区域的大小。
2. 你必须同样添加被显示的一个或多个视图，并且通过滚动视图滚动。这些视图提供显示内容。

你可以随意配置应用程序要求的视觉提示－垂直和水平滚动指示器，弹性拖拽，弹性缩放或者弹性定向滚动。

### 在Interface Builder中创建滚动视图
为了在Interface Builder中创建滚动视图，拖拽Library->Cocoa Touch->Data Views单元内的*UIScrollView*图标到你的“窗口”内。然后把*UIViewController*的子类的视图输出口连接到滚动视图。如图1-1展示了此连接，假设File’s Owner是*UIViewController*的子类（常见的设计模式）。

如图1-1 怎样把*UIViewController*的子类连接到滚动视图




尽管在Interface Builder中*UIScrollView*的检查器允许你许多滚动视图实例的属性，你仍然要负责在你的应用程序代码中设置*contentSize*属性，它在定义了滚动区域的大小。如果你已经把滚动视图连接到控制器的实例的视图，在控制器的*viewDidLoad*方法初始化*contentSize*属性，如清单1-1所示。

清单1-1 设置滚动视图的尺寸

    - (void)viewDidLoad {
	    [super viewDidLoad];
	    UIScrollView *tempScrollView = (UIScrollView *)self.view;
	    tempScrollView.contentSize = CGSizeMake(1280, 960);
    }


在滚动视图的尺寸被设置后，你的应用程序就能够添加提供内容的视图，可以用编程的方式或者Interface Builder的方式添加。

### 通过编程的方式创建滚动视图
完全用代码创建滚动视图也是可以的。这通常是在控制器类中完成的，特别指出，在*loadView*方法里实现。实现的例子在清单1-2中展示。

清单1-2 在代码中创建滚动视图


     - (void)loadView {
		CGRect fullScreenRect = [[UIScreen mainScreen] applicationFrame];
		scrollView = [[UIScrollView alloc] initWithFrame:fullScreenRect];
		scrollVIew.contentSize = CGSizeMake(320, 758);

		// do any further configuration to the scroll view
		// add a view, or views, as a subview of the scroll view.
         
		// release scrollView as self view retains it
		self.view = scrollView;
		[scrollView release];
	}

这段代码创建了一个全屏幕尺寸的滚动视图，设置滚动视图对象为控制器的视图，并且设置`contentSize`属性为320*758像素。这个代码创建的滚动视图是垂直滚动。

在这个方法里会有更多的代码被实现，例如，插入字视图或者视图并且配置它们也是必须的。同样的，这段代码假定控制器还没有设置视图。如果已经设置了，在设置滚动视图为控制器的视图之前你有责任释放已经存在的视图。

### 添加子视图
在你已经创建并配置完滚动视图后，你必须添加一个或多个字视图来显示他们的内容。在你的滚动视图中，应该用单一子视图或者多个子视图是一个设计抉择，通常机遇一个需求：滚动视图是否需要放大？

如果你打算在你的滚动视图中支持放大，最常用的技术是使用单一子视图，它能包含整个滚动视图的`contentSize`，然后在这个视图傻姑娘添加额外的子视图。这允许你指定单一的’collection’内容视图作为视图来放大，并且它的字视图会根据它的状态放大。

如果放大不是必须的，那么滚动视图是否使用单一子视图或者多个子视图依赖应用程序的决定。

-----

笔记：同时返回一个子视图是最常见的情况，你的应用程序可能需要在同一滚动视图的内部允许多个视图，以支持放大的能力。在那种情况下，你可以使用代理方法`viewForZoomingInScrollView:`返回适当的子视图，更多的叙述在“[Basic Zooming Using the Pinch Gestures]()”

-----

### 配置滚动视图的Content Size，Content Inset，还有Scroll Indicators
`contentSize`属性是你需要在滚动视图中显示的内容的尺寸。如图1-2中的图片展示了指示了宽度和高度的`contentSize`的滚动视图的内容。

图1-2 标记`contentSize`尺寸的内容






你可能想需要在滚动视图的边界添加填充（padding），通常在顶部或者底部，以便控制器和工具栏（toolbars）不会干扰内容。想要添加填充，使用`contentInset`属性在滚动视图的内容周围指定一个缓冲区。一种思考它的方式是，在不改变子视图或者视图内容尺寸的情况下是滚动视图的内容区域变大。

`contentInset`属性是一个包含`top`，`bottom`，`left`，`right`这几个字段的`UIEdgeInsets`结构体。如图1-3展示了指示出`contntInset`和`contentSize`的内容。

图1-3 指示了`contentSize`和`contentInset`的内容




如图1-3所示为`conentInset`属性指定了`(64, 44, 0, 0)`，使滚动视图的顶部有一个额外的64像素的缓冲区域，底部有44像素的缓冲区域。这样设置`contentInset`的值允许在屏幕上显示导航栏和工具栏，但仍然允许滚动以显示滚动视图的全部内容。

清单1-3 设置`contentInset`属性

    - (void)loadView {
		CGRect fullScreenRect = [[UIScreen mainScreen] applicationFrame];
		scrollView = [[UIScrollView alloc] initWithFrame:fullScreenRect];
		self.view = scrollView;
		scrollView.contentSize = CGSizeMake(320, 758);
		scrollView.contentInset = UIEdgeInsetsMake(64.0, 0.0, 44.0, 0.0);

		// do any further configuration to the scroll view
		// add a view, or views ,as a subview of the scroll view.

		// release scrollView as self.view retains it
		self.view = scrollView;
		[scrollView release];
	}


如图1-4 展示了在设置了`contentInset`的顶部和顶部的参数值的结果。当滑动到顶部时（如左图所示），屏幕上为navigation bar和status bar留有空间。在右边的图片展示了内容滑动到底部为toolbar留有空间。在两种情况下你都能看见当滑动的时候内容穿过透明的navigation bar和toolbar，当内容完全滑动到顶部或者底部的时候，所有的内容是可见的。

图1-4 设置了`contentInset`的顶部和底部值的结果







然而，当滚动视图显示指示器的时候，改变`contentInset`的值会有一个意想不到的副作用。当拖拽内容到屏幕的顶部或者底部的时候，滚动指示器超出了被`contentInset`定义的区域，在navigation control和toolbar之间。

为了更正这个问题，你必须设置`scrollIndicatorInsets`属性。正如`contentInset`属性，`scrollIndicatorInsets`属性被指定为`UIEdgeInsets`结构体。设置垂直方向的插入值来限制垂直方向的指示器，这也导致水平滚动指示器的显示超出了`contentInset`的矩形区域。

只修改`contentInset`属性而不设置`scrollIndicatorInsets`属性使滚动指示器绘制在navigation controller和toolbar之间，这不是想要的结果。然而，设置`scrollIndicatorInsets`属性值与`contentInset`相匹配来更正这种情况。

正确的`loadView`实现在清单1-4，通过添加`scrollIndicatorInsets`初始化配置滚动视图所需要要的额外代码。

清单1-4 设置滚动视图的`contentInset`和`scrollIndicatorInsets`属性

    -  (void)loadView 
		CGRect fullScreenRect = [[UIScreen mainScreen] applicationFrame];
		scrollView = [[UIScrollView alloc] initWithFrame:fullScreenRect];
		scrollView.contentSize = CGSizeMake(320, 758);
		scrollView.contentInset = UIEdgeInsetsMake(64.0, 0.0, 44.0, 0.0);
		scrollView.scrollIndicatorInsets = UIEdgeInsetsMake(64.0);

		// do any further configuration to the scroll view
		// add a view, or views, as a subview of the scroll view.

		// release scrollVIew as self.view retains it
		self.view = scrollView;
		[self.scrollView release];
	}


## 移动滚动视图

初始化一个滚动视图的滚动最常用的方法是用户直接触摸屏幕，并利用他或她的手指拖动。然后滚动视图响应这个动作滚动内容。这个手势被称为*拖拽手势*。

拖动动作中的一个变化是*轻弹手势*，一个轻弹手势是用户手指与屏幕初次接触后的快速移动，在想要滚动的方向拖动，然后离开屏幕。这个手势不仅仅能引起滑动，它根据用户拖拽的速度给人一种动量，这使得用户的手指完成后任然能继续滑动。滑动在指定的时间内主见减速。轻弹手势允许用户一个单一动作移动很长的距离。在减速的任意时刻，用户能在适当的位置触摸屏幕来停止滚动。所有这些行为都内置在`UIScrollView`内，就开发者而言不需要实现它们。

但是有时候为程序以编程的方式滚动内容是有必要的，举例来说，显示一个文档的特定部分。在那种情况下，`UIScrollView`提供了必要的方法。

### 以编程方式滚动
滚动滚动视图的内容不总是响应用户拖动或者轻弹屏幕。有些时候，你的应用程序需要滚动到一定的偏移量，使特定的矩形区域暴露，或者滚动到视图的顶部。`UIScrollView`提供了方法来执行所有这些操作。

### 滚刀刀指定的偏移量
滚动到指定的左上角位置（`contentOffset`属性）能通过两种方式实现。`setContentOffset:animated:`方法使内容滚动到指定偏移量。如果animated参数是YES，滚动会从当前位置以恒定的速度以动画的方式到指定的位置。如果animated参数是NO，滚动是即时的，没有发生动画。在这两种情况下，委托发送一个`scrollViewDidScroll:`消息。如果动画被禁用，或者你直接通过`contentOffset`属性设置内容偏移量，代理收到一个`scrollViewDidScroll:`消息。如果动画开启，当动画正在进行时，代理会接收一系列`scrollViewDidScroll:`消息。当动画完成的时候，代理收到一个`scrollViewDidEndScrollingAnimation:`消息。

### 使一个矩形可见
另外，也可以滚动矩形区域使它可见。当一个应用程序需要显示当前可见视图之外的控制区域的时候，这是非常有用的。`scrollRectToVisible:animated:`方法滚动指定的矩形区域，使其在滚动视图内可见。如果animated参数是YES，矩形以恒定的速度滚动到视图内。和`setContentOffset:animated:`一样，如果动画被禁用，委托发送一个`scrollViewDidScroll:`消息。如果动画被启动，在动画正在进行时，委托发送一系列`scrollViewDidScroll:`消息。在`scrollRectToVisible:animated:`的情况下，滚动视图跟踪和拖动性能也没有。

如果为了`scrollRectToVisible:animated:`动画被启用，代理接收一个`scrollViewDidEndScrollingAnimation:`消息，提供了滚动视图已经到达指定位置并且动画已经完成的通知。

### 滚动到顶部
如果状态栏是可见的，当在状态栏上单击的时候，滚动视图也能滚动到内容的顶部。这种做法在应用程序中是很常见的，提供数据在垂直方向上的表现。例如，照片程序无论在是相册选择表视图还是在查看相关照片在相册中的缩略图都支持滚动到顶部，或者在大多数`UITableView`（`UIScrollView`的子类）的实现也支持滚动到顶部。

你的应用程序通过滚动视图的代理方法`scrollViewShouldScrollToTop:`为YES来开启这个行为。如果在屏幕上有多个滚动视图在同一时间通过返回滚动视图来滚动，这个代理方法允许精细的控制那些会滚动到顶部的滚动视图。

当滚动完成，委托发送一个`scrollViewDIdScrollToTop:`消息，详细说明这个滚动视图。

### 在滚动期间发送的代理消息
由于滚动时，滚动视图使用`tracking`（跟踪），`dragging`（拖动），`decelerating`（减速）和`zooming`（缩放）属性跟踪状态。另外，`contentOffset`属性定义了可见内容在滚动视图的左上角边界的点。下面的表描述了每个状态的属性：

| State property        | Description         |
|:------------- |:-------------|
| `tracking`      | 如果用户的手指与设备的屏幕相接处为`YES` |
| `dragging`      | 如果用户的与设备的屏幕相接处并且移动为`YES`  |
| `decelerating` | 如果滚动视图的减速为轻弹手势，或者超出滚动视图框架拖动的反弹的结果为`YES` |
| `zooming` | 如果滚动视图跟踪一个开合手势来改变`zoomScale`属性为`YES` |
| `contentOffset` | 定义滚动视图左上角的一个`CGPoint`值 |

遍历这些属性来判断正在进行的动作是没有必要的，因为滚动视图会给代理发送详细的消息序列。这些方法可以让你的应用程序在必要的时候做出回应。委托方法可以查询这些状态的属性来确定为什么收到消息或者滚动视图目前是什么状态。

### 简单的开始：跟踪滚动操作的开始和完成
如果你的应用程序只关注滚动过程的开始和结束，你只能实现的维多方法只有一小部分。

实现`scrollVIewWiewWillBeginDragging:`方法来接受拖动开始的通知。

为了确定什么时候滚动完成，你必须实现两个代理方法：`scrollViewDidEndDragging:willDecelerate:`和`scrollViewDidEndDecelerating:`。当委托收到decelerate参数为NO的`scrollViewDidEndDragging:willDecelerate:`消息，或者委托收到`scrollViewDidEndDecelerating:`方法。在这两种情况下，滚动完成。

### 完整的委托消息序列
当用户接触屏幕，tracking sequence开始。`tracking`属性立刻被设置为`YES`，只要用户的手指与屏幕相接触它保持值为YES，不管怎样移动手指。

如果用户的手指保持静止并且内容视图响应触摸事件，它应该处理这个触摸，并且该序列是完整的。

然而，如果用户移动手指，这个序列继续。

当用户开始移动他或她的手指开始滚动滚动视图第一次尝试（假设滚动视图的默认值）取消任何正在进行的触摸操控，如果试图这样做。

-----

笔记：整个消息队列，跟踪和拖拽属性总会保持NO，放大属性为YES，这是有可能的。在滚动的发生是由于放大操作的时候，无论通过一个手势或者编程滚动。如果代理方法作为放大或者滚动的结果，你的应用程序可能会采取不同的行动。

-----

滚动视图`dragging`属性被设置成`YES`，然后他的委托被发送`scrollVIewWillBeginDragging:`消息。

当用户拖动他或者她的手指，`scrollVIewDidScroll:`消息被发送到委托。这个消息在滚动的时候不断被发送。你的方法实现能查询滚动视图的`contentOffset`属性来确定基于滚动视图左上角的位置。`contentOffset`属性总是当前基于左上角的位置；无论滑动是否在进行。

如果用户执行轻弹手势，`tracking`属性为`NO`，因为为了执行轻弹手势，在最初的手势之后，用户的手指与离开屏幕。此时，代理接收一个`scrollViewDidEndDargging:willDecelerate:`消息。随着滚动减速deceleration参数为`YES`。减速的速度通过`decelerationRate`（减速度）属性控制。在默认的情况下，这个属性为`UIScrollViewDecelerationRateNormal`，这个值允许滚动持续相当长一段时间。你可以设置速率为`UIScrollDecelerationRateFast`使滚动持续更短的时间，在轻弹手势后滚动的距离也更短。当视图减速时，滚动视图的`decelerating`属性为`YES`。

如果用户拖动，停止拖动然后手指从屏幕上离开，委托收到`scrollViewDidEndDragging:willDecelerate:`消息，然而deceleration参数是`NO`。这是因为在scroll view上没有动能。用户的手指离开屏幕`tracking`属性是`NO`。

如果`scrollViewDidEndDragging:willDecelerate:`的decelerate参数是`NO`，然后scroll view的委托不会再收到拖拽动作的委托消息。滚动视图的decelerating属性现在也返回一个`NO`。

还有另外一种情况能使`scrollVIewDidEndDragging:willDecelation:`消息发送给委托，即使用户的手指在静止的时候离开屏幕。如果当用户拖动内容超过滚动区域的边界的时候，滚动视图配置了反弹的视觉效果，`scrollViewDidEndDragging:willDecelerate:`消息被发送到委托，decelation参数是`YES`。当`bounces`属性为`YES`（默认状态）的时候反弹开启。`alwaysBounceVertical`和`alwaysBounceHorizontal`属性当`bounces`属性为`NO`的时候不产生影响。如果`bounces`为`YES`的时候，它允许`contentSize`属性的值小于滚动视图的bounds。

不管导致滚动视图接收`csrollviewdidenddargging:willDecelerate:`消息是什么条件，如果decelerate参数是`YES`，滚动视图就会发送`scrollViewWillBeginDecelerating:`消息。在减速期间，尽管`tranking`和`dragging`属性都为`NO`，委托继续接收`scrollDidEndScroll:`消息。`decelerating`属性仍然为`YES`。

最后，当减速完成，委托被发送`scrollViewDeiEndDecelerating:`消息，`decelerating`属性的值为`NO`，滚动序列完成。


## 使用开合手势实现基本变焦

`UIScrollView`支持很容易的实现开合手势来变焦。你的应用指出变焦因素然后实现一个简单的委托。只需要简单的几步就能使滚动视图支持开合手势的变焦。

### 支持开合手势
捏合和打开手势是iOS应用的标准手势，用户期望在放大缩小的时候使用它。如图3-1展示了开合手势

图3-1 标准的捏合和打开手势







为了支持变焦，你必须为你的滚动视图设置一个委托。这个委托类必须遵守`UIScrollViewdelegate`协议。在很多情况下，这个委托是滚动视图的控制器。这个委托必须实现`viewForZoomingInScrollView:`方法然后返回要变焦的视图。下面展示了委托的实现方法，返回一个`UIImageView`的`imageView`属性。这指出了`imageView`属性会被放大来响应变焦手势，以及任何有计划的变焦。

    - (UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView {
		return self.imageView;
	}

为了指定用户的缩放率，你可以设置`minimumZoomScale`和`maximumZoomScale`属性，他们的初始值都为1.0。这些属性能够在Interface Builder中`UIScrollView`的属性检查器或者编程设置。清单3-1展示了在UIViewController的子类中为了支持变焦要求的代码。假设控制器的子类实例为委托类并且实现了`viewForZoomingInScrollView:`委托方法。

清单3-1 `UIViewController`的子类实现了最低要求的变焦方法

    - (void)viewDidLoad {
		[super viewDidLoad];
		self.scrollView.minimumZoomScale = 0.5;
		self.scrollView.maximumZoomScale = 6.0;
		self.scrollView.contentSize = CGSizeMake(1280, 960);
		self.scrollView.delegate = self;
    }

指出变焦元素和实现了`viewForZoomingInScrollView:`方法的委托对象，是支持开合手势的变焦的最低要求。

### 一编程的方式缩放
一个滚动视图可能需要变焦来响应触摸手势，像双击或者其他点击手势，或者来响应其他用户的开合手势以外的手势。为了允许这样，滚动视图提供了两个实现方法：`setZoomScale:animated:`和`zoomToRect:animated:`。

`setZoomScale:animated:`设置当前缩方率到指定的值。这个值必须在`minimumZoomScale`和`maximumZoomScale`范围内。如果动画参数是`YES`，这个变焦会执行一个恒定的动画直到完成；否则立刻改变比例。也可以直接设置`zoomScale`来实现。这种方法等同于设置`setZoomScale:animated:`，动画参数为`NO`。当用这种方法或者直接通过改变属性的方法变焦，视图会被变焦这样的视图中心保持静止。

`zoomToRect:animated:`方法使内容变焦到指定的矩形。因为`setZoomScroll:animated:`这个方法有一个动画的参数，它决定了是否改变位置和动画变焦。

你的应用程序会经常需要设置变焦比例或者在点击指定位置的时候改变位置。因为`setZoomScale:animated:`放大周围可见内容的中心，你会需要一个能够指定位置让变焦元素转变大一个矩形，这个功能用`zoomToRect:animated:`是合适的。一个实用程序方法，它有一个滚动视图，变焦比例和在变焦矩形中间的一个点，正如清单3-2所展示的那样。

清单3-2 一个转换变焦比例和变焦中心的使用方法

    - (CGRect)zoomRectForScrollView: (UIScrollView *)scrollView withScale:(float)scale withCenter:(CGPoint)center {
		
		CGRect zoomRect;

		// The zoom rect is in the content view’s coordinates.
		// At a zoom scale of 1.0, it would be the size of 
		// the imageScrollView’s bounds
		// As the zoom scale decreases, so more content is visible,
		// the size of the grows.
		zoomRect.size.height = scroll.frame.size.height / scale;
		zoomRect.size.width = scrollView.frame.size.width / scale;

		// choose an origin so as to get the right center.
		zoomRect.origin.x = center.x - (zoomRect.size.width / 2.0);
		zoomRect.origin.y = center.y - (zoomRect.size.height / 2.0);

		return zoomRect;
    }

当在自定义的支持的子类中响应双击手势的时候，这个使用方法是很有用的。通过与滚动视图相关的实例和一个新的比例或者围绕变焦中心的点来使用这个方法是很简单的。当响应一个双击手势时，响应的中心点通常是点击的中心点。这个方法返回的矩形被传到`zoomToRect:animated:`方法。

### 通知委托变焦完成
当用户完成开合手势或者滚动视图有计划的变焦被完成，滚动视图的委托被通知接收一个`scrollViewDidEndZooming:withView:atScale:`消息。

这种方法以滚动视图实例，已滚动的滚动视图的子视图，并在该变焦完成参数的比例因子作为参数。一旦接收到这个消息代表你的应用程序就可以采取适当的行动。



未完待续
















