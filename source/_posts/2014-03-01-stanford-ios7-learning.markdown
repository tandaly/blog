---
layout: post
title: "斯坦福大学iOS7学习笔记"
date: 2014-03-01 18:44
comments: true
categories: 学习笔记
keywords: 斯坦福、iOS7
description: 斯坦福iOS7学习笔记


---

其实这个视频出来也有一段时间了，但是由于各种原因一直拖到现在还没看完。这篇文章是我在观看视频以及自己Demo时的一些笔记记录,有些可能上下文不连贯,如果造成不便请理解,也可以留言指出。
<!--more-->  
![](/images/blogimg/stanford/description.png)

1. [概览](#1)
2. [objective-c](#2)
3. [Foundation And Attributed Strings](#3)
4. [View Controller Lifecycle](#4)
5. [Polymorphism with Controllers, UI](#5)
6. [手势、UIView、CoreGraphics](#6)
7. [OC的一些语法特色&Animation](#7)
8. [AutoLayout](#8)
9. [Multithreading、UIScrollingView、UITableView](#9)
10. [UITableView](#10)
11. [Document&Core Data](#11)
12. [Core Data and Table View](#12)
13. [其他](#13)



#<a id="1"></a>1.概览(第一二集)
---
IBAction其实就是void
#<a id="2"></a>2.objective-c
---

1.在数组中做插入时，应该先判断插入的位置是否越界。给数组发消息时，最好先判断数组是否为空
  
2.做输出口连接时，选择`Outlet Collection`(有三种:Outlet,Action,Outlet Collection)，默认为strong也必须为strong，view对每个输出口都有强引用，但是不会对输出口数组有强引用.当连接时，自动生成。如下图
  
  ![](/images/blogimg/stanford/collection.png)
  
如果设置为weak的话，将没有人对其有强引用，数组会持续为空。检查所有的输出口是否全部连接上，可以将鼠标放在property的前面实心圆点上，来观察所有的是否高亮。其中数组中的顺序是未知的
  
3.当要获取操作的对象在`collection`中的位置时，可以使用`[self.array indexOfObject:sender]`;`array`为`collection`生成的数组
  
4.`[array firstObject]`意义并不等同于 `array[0]`,当array为nil时，若用array[0]则会导致程序崩溃，而firstObject 则不会（此时return nil即返回nil）。
#<a id="3"></a>3.Foundation And Attributed Strings
---


1.如果给NSString *str 发送[str mutable copy]将会产生一个可变的mutable String。

2.像firstObject、lastObject这种并没有产生新的对象，而是给你一个指向他们的指针。

3.给nil发送消息，将不会崩溃，相应得代码也不会被执行，如果该方法有返回值，则返回0或者nil.但是当返回值是C语言的结构体的话，将返回一个不确定的值，比如CGPoint p = [nil getLoction],此时p将是不确定的值。

4.动态绑定，id类型本身就是指针，所以不用加星号，指向一个不确定类型的对象。  

![](/images/blogimg/stanford/objec-pointer.png)
                            
运行时所有的对象指针均被当作id来对待。但是在编译时，如果你用比如NSString *而不是id，编译器会给你相应的提示，它可以帮你找出bug，给出适当的方法.

5.由于我们使用的是静态类型定义语言，并且编译器知道你的指针指向的类型.所以，动态绑定相对来说是安全的.但实际上还是有隐患的，比如：

![](/images/blogimg/stanford/static.png)

6.不要使用id *，因为这是指向指针的指针

7.些情况下，编译时会给出警告，而运行时不会崩溃，比如：

![](/images/blogimg/stanford/warning.png)

8.那么什么时候该用到id(动态绑定)呢？

![](/images/blogimg/stanford/when-to-dynamic.png)

要让他变得安全可以使用两种方式：内省（introspection）[即在运行时判断某个对象属于哪个类或者能给它发送什么样的消息]和协议（protocol）
                
9.所有继承自NSObject的类都有下面的方法

  	isKindOfClass：某对象是否属于某个(包括继承)
   	isMemberOfClass：某对象是否属于某个类，(不包括继承)
   	respondsToSelector:某对象是否相应某个方法
上面三个方法都是在运行时来确定的。
一般的书写格式：

![](/images/blogimg/stanford/run-timemodel.png)

在数组中可以使用 makeObjectsPerformSelector使数组每个值都执行某个方法。
####Foundation Framework


1.给可变的对象发送copy消息，返回的是不可变的对象。比如 *arr = [array copy]假设array为可变的。copy消息返回的不可变的，mutableCopy返回的是可变的(不管是给可变或者不可变的对象发送消息都是如此)
2.NSArray一些有意思的方法 
  
  ![](/images/blogimg/stanford/funnuy-NSArray.png)
  
3.NSNumber的新语法
  
  ![](/images/blogimg/stanford/NSNumber-new-syntax.png)
  
4.NSDictionary（类似哈希表）
  
  ![](/images/blogimg/stanford/NSDictionary.png)
  
5.Property List （集合的集合） ：NSUserDefaults 就是一个典型的property list ，NSUserDefaults 通常只用来存储一些较小的东西，比如账号、密码最好别用来存储一些容量很大的东西，比如图片，NSData  

6.在调用[[NSUserDefaults  standard]setobject:forKey]之后必须使用[[NSUserDefaults  standard] synchronize]是数据同步

7.NSRange   range = （i，j）它的范围是从第i个到i+j-1个。
  
  ![](/images/blogimg/stanford/NSRange.png)
  
8.UIColor：alpha（透明度，0代表全透明，1代表不透明）

9.UIFonts ：ios7种很大的一点改进就是对字体的支持更多元化
  
  ![](/images/blogimg/stanford/UIFont.png)

Attributed Strings

  1. NSAttributedString（类似NSString但是字符串中的每个字符都有自己的属性  并非NSString的子类）是不可变的（immutable）
  2.  NSMutableAttributedString（使用的更多）
  3. 所有的属性添加下划线
  
  ![](/images/blogimg/stanford/underline.png)

#<a id="4"></a>4.View Controller Lifecycle
---
####UITextView
常用属性：
`UIFont *font，NSTextStorage *textStorage``NSTextStorage`是`NSMutableAttributedString`的子类，可以随意修改它，UITextView会自动更新修改后的内容。
在设置font的时候最好是为特定的字符或字符串设置font，不然会影响到性能.

####UIView LifeCycle
不要在viewdidload方法里面设置一些尺寸会发生变化的控件

顺序
awakeFromNib->viewDidload->viewWillLayoutSubviews->viewDidiLayoutSubviews->viewWillAppear->viewDidAppear


1.viewwillappear只需一次性初始化的不要放在该方法中,最好放入viewdidiload.当展示的东西会发生改变时,可以将其放在该方法中,使其同步
一些网络延时等待可以放在这个方法中进而优化,减少等待时间.在该方法中来设置控件尺寸
viewwilldisappear 这个方法一般用来存储当前数据,用作恢复时的重载.

2.viewWill(did)LayoutSubviews (iOS6.0)最好在did里面来改变尺寸

3.awakeFromNib(storyboard)	 在输出口被设置之前调用

想要了解更多情况可以看看[理一理UIViewController的东东](http://blog.csdn.net/u012971918/article/details/16983499)。


####Notification
在接收通知后如无需再监听,就`removeObeserver:self`，`viewWillAppear`里面添加通知监听同时执行监听到通知后应该执行的方法，在`viewWillDisappear`里面移除监听。

`notification center`拥有一个`unsafe retained`指针指向监听者。`unsafe retained` 意味着如果将监听者移除堆栈却没有调用`remove`这个方法，`notification center`仍会向监听者发送notification，这个时候APP将会崩溃。之所以用`unsafe retained`是因为向前（ios 4、5）兼容的问题。


#<a id="5"></a>5.Polymorphism with Controllers, UI
---

抽象类意味着该类不能被实例化，只能作为超类来给其他具体类来继承。在storyboard里面`ctrl+shift+单击`，会显示鼠标所在区域的所有东西。

具体类继承了抽象类的outlet以及Action（即使是private也能继承）

导航条上的button并非UIButton而是`UIBarButtonItems`（轻量级button）放置在`NavigationItem`上

下方为`toolbaritems`

####segues
要为每一个segue设置id以便辨识区分
####UINavigationController
通过self.navigationcontroller来判断当前controller是否为UINavigationController。tabarcontroller同理

判断某个view是否为当前显示，可以用[view.window]是否为nil来判断。当某个view当前并没有显示，但是由于数据变化需要更新其UI，则可将UI更新代码放在viewWillAppear里。UI更新最好作为一个方法updateUI，以方便复用。

####UITabarController
tabaritem的icon是30*30

#<a id="6"></a>6.手势、UIView、CoreGraphics
---
####UIWindow
view层级的顶层view，一般情况下一个APP只有一个UIWindow

`initWithFrame`不能用于storyboard的view，可以用`awakeFromNib`

####View的坐标系
CGFloat:(x,y),CGSize:(width,height),CGRect:(CGFloat,CGSize)

bounds是自身坐标系，frame是父视图坐标系。

可在`drawRect`方法中对View进行自定义draw。不要调用drawRect，它是供系统调用的，但是可以复写。可以通过调用`setneedsdisplay`或者`setneedsdisplayinrect`，这两个方法会调用`drawrect`方法。

CoreGraphics利用UIBezierPath来完成画线 填色 线条粗细等操作

上下文`context`决定了画的位置,UIBezierPath在当前上下文`context`中画因此可以不用去得到上下文.如果要得到上下文可调用`CGContextRef context = UIGraphicsGetCurrentContext()`,这里直接调用了C函数.`CGContextRef`是一个void型指针.

####示例						
	//用UIBezierPath画三角形
	UIBezierPath *path = [[UIBezierPath alloc]init];
	[path moveToPoint:CGPointMake(75,10)];
	[path addLineToPoint:CGPointMake(160,150)];
	[path addLineToPoint:CGPointMake(10,150)];
	[path closePath];
	path.lineWidth = 3.0f;
	//上面所有步骤并没有画出该三角形,仅仅是确定其大小
	//需要加上以下几个方法调用才行
	[[UIColor redColor]setFill];
	[[UIColor blueColor]setStroke];
	[path fill];
	[path stroke];
还可以用贝塞尔曲线画各种形状,方法参见API.`[path addClip]`方法可以剪切出该封闭曲线内所画的内容.

如果要使view有透明度,首先需要将其opaque`@property BOOL opaque`属性设置为NO(但是对系统性能有一定影响).可以通过设置透明度`alpha`来改变整个view的透明度.

通过CGContext函数来生成图片

	UIGraphicsBeginImageContext(CGSize);
	UIImage *image = UIGraphicsGetImageFromCurrentContext();
	UIGraphicsEndInamgeContext();
在UIImage上作图

	[img drawAtPoint:point];
	[img drawInRect:rect];
	[img drawAsPatternInRect:rect];

####手势
首先要使view添加手势才能识别,其次要实现手势识别的方法.

- UITapGestureRecognizer
- UIPinchGestureRecognizer
- UIRotationGestureRecognizer
- UISwipeGestureRecognizer
- UIPanGestureRecognizer //缓慢拖动
- UILongPressGestureRecognizer


####Demo
NSParaGraphStyle来设置段落的一些属性.

字体的翻转
`CGContextTranslateCTM(context,x,y)`(x,y是要移动的距离,相对于当前坐标系)来移动,然后在设定角度来翻转即可`CGContextRotateCTM(context,M_PI)`.

#<a id="7"></a>7.OC的一些语法特色&Animation
---
####协议(Protocol)
内省:有时有效,但不是主要编程方法,它是在运行时发生的.

Attention!在协议中也可以有属性(property)的申明.

*NSObject Protocol*里面的方法和*NSObject*种方法是一样的,那为什么要有*NSObject protoclo*呢?这是作为给那些非*NSObject*的类在运行时来实现自省.(可以自定义某个类并非继承自NSobjec.如`@inteface MyObject`,并没有父类,但是经常不这么做,通常所有类都继承自*NSObject*).
####闭包(Block、closure)
通常为 `^ int (int a,int b)`,^为block标识符,紧接着为返回类型,然后是变量.如果可以从其返回值推测其返回类型,可以省去返回值.(如省去int)

对于闭包外的局部变量,在闭包内使用时是只读的.如果想要在闭包内部改变闭包外变量,可将其变量加上`__block`标识符(两个下划线),这样编译器会将存于栈中的该变量转存到堆中.当闭包执行完之后,会将其信息复制到堆中的该变量然后复制到栈中.对于实例变量,通过setter和getter方法来改变它的值.因为它是存于堆中,所以不必加__block.

给block中的对象发送消息就会使得该变量引用计数次数加1,即会有一个strong pointer指向它.

某种程度上(存储block时)block可以看作对象,也有相应的引用计数(retaincount).但是它不能执行任何除`copy`方法外的方法.如下面示例
	
	//.h
	@property (nonatmic,strong) NSArray *myBlocks;
	//.m
	//这一句存在着循环引用
	[self.myBlocks addObject: ^{
		[self doit];
	}];
	//block的赋值
	viod (^dosomething)(void) = self.myBlocks[0];
	dosomething();
####循环引用
在某些时候会造成循环引用的问题,因为闭包对闭包内所有的实例变量均为强引用,只要闭包还存留于堆当中.比如上面示例中block对self有强引用,同时self对block也有强引用,这时便产生了循环引用(retain cycle).
####解决方法
	
	//循环引用的解决
	__weak SomeClass *weakSelf = self;
	[self.myBlocks addObject: ^{
		[weakSelf doit];
	}];
这样便实现了循环引用的问题.这个时候block对weakSelf是弱引用而非强引用,所以不会产生类似问题.
####什么时候用到Block
 - 枚举(Enumeration)
 - 动画(View Animation)
 - 排序(Sorting)
 - 通知(Notification)
 - 错误处理(Error handlers)
 - 完成处理程序(Completion handlers)
 - 多线程(Multithreading:Grand Central Dispatch)
 
关于Block的更深层次的东西请看[谈Objective-C Block的实现](http://blog.devtang.com/blog/2013/07/28/a-look-inside-blocks/)
####动画 Animation
UIView的类方法

	+ (void)animateWithDuration:(NSTimeInterval)duration
						  delay:(NSTimeInterval)delay
						 option:(UIViewAnimationOptions)options
					  animation:(void (^)void)animations
					 completion:(void (^)(BOOL finished))completion

####UIViewAnimationOptions
 - BeginFromCurrentState      // interrupt other, in-progress animations 					             of these properties
 - AllowUserInteraction       // allow gestures to get processed while 									 animation is in progress
 - LayoutSubviews             // animate the relayout of subviews along 								     with a parent’s animation
 - Repeat                     // repeat indefinitely
 - Autoreverse                // play animation forwards, then backwards
 - OverrideInheritedDuration  // if not set, use duration of any in-										 progress animation
 - OverrideInheritedCurve     // if not set, use curve (e.g. ease-in/out) 								 of in-progress animation
 - AllowAnimatedContent       // if not set, just interpolate between 									 current and end state image
 - CurveEaseInEaseOut         // slower at the beginning, normal 										 throughout, then slow at end
 - CurveEaseIn                // slower at the beginning, but then 									     constant through the rest
 - CurveLinear                // same speed throughout
####力学性动画
 - 重力
 - 撞击
 - 外力作用
 - ......

步骤:

> 1 创建UIDynamicAnimator  `UIDynamicAnimator *animator = [[UIDynamicAnimator alloc] initWithReferenceView:aView]; `
 
> 2 为其添加UIDynamicBehaviors(如重力加速度,冲撞等)  `UIGravityBehavior *gravity = [[UIGravityBehavior alloc] init];`
>`[animator addBehavior:gravity];`

> 3 为UIDynamicBehaviors添加UIDynamicItems(通常是UIView)
`id <UIDynamicItem> item1 = ... ; ` 

>`[gravity addItem:item1]; `

	//UIDynamicItem protocol的内容
	@protocol UIDynamicItem  
	@property (readonly) CGRect bounds; 
	@property (readwrite) CGPoint center; 
	@property (readwrite) CGAffineTransform transform; 
	@end 
如果想要改变`center`或者`transform`必须调用这个方法`- (void)updateItemUsingCurrentState:(id <UIDynamicItem>)item`,否则改变是无效的.

####一些力学行为的属性
UIGravityBehavior  

	@property CGFloat angle; 	
	@property CGFloat magnitude;  // 1.0 is 1000 points/s/s
UICollisionBehavior 

	@property UICollisionBehaviorMode collisionMode;   //  Items,  Boundaries,  Everything (default)
	-(void)addBoundaryWithIdentifier:(NSString *)identifier forPath:(UIBezierPath *)path;  
	@property BOOL translatesReferenceBoundsIntoBoundary; 

其他的动态力学行为参见官方文档.
####UIDynamicItemBehavior  
	@property BOOL allowsRotation; 
	@property BOOL friction; 
	@property BOOL elasticity;  
	@property CGFloat density;   
	- (CGPoint)linearVelocityForItem:(id <UIDynamicItem>)item;  
	- (CGFloat)angularVelocityForItem:(id <UIDynamicItem>)item; 
####UIDynamicBehavior
它是所有动态力学行为的父类,自定义行为可继承自UIDynamicBehavior.
所有的Behavior都知道自己属于哪个UIDynamicAnimator,因为它们只能同时属于一个Animator.

很重要的一个属性`@property (copy) void(^action)(void)`在action里面不要执行耗时的操作,以免影响动画的进行.

####Demo
继承UIDynamicBehavior,自定义力学行为

	//复写init方法,为子DynamicBehavior添加各种力学行为,如重力,冲撞等等
	- (instancetype)init
	  {
	    self = [super init];
		[self addChildBehavior:self.gravity];
		...		
	    return self;
	  }
#<a id="8"></a>8.AutoLayout
---
通过约束条件而非数字来控制视图的尺寸.

在Xcode中预览旋转后视图的变化(无需启动模拟器):*选中某个Controller-->Attribute Inspector-->Oreintation*.

添加约束的三种方法:1.通过blue guideline 2.用界面右下角的菜单栏 3.ctrl+鼠标点击

#<a id="9"></a>9.Multithreading、UIScrollingView、UITableView
---
####在其它队列上(queue)执行block

	dispatch_queue_t queue = ...;//创建block将要执行的队列
	dispatch_async(queue,^{});
####获取main queue

	dispatch_queue_t mainQ = dispatch_get_main_queue();    
	NSOperationQueue *mainQ = [NSOperationQueue mainQueue]; // for object-oriented APIs
####创建队列(非mainqueue)

	dispatch_queue_t otherQ = dispatch_queue_create(“name”, NULL); //  name a const char *!name是一个常量指针非NSString
####主线程上调用某方法

	//NSObject  method … !
	- (void)performSelectorOnMainThread:(SEL)aMethod  
    	                     withObject:(id)obj  
    	                  waitUntilDone:(BOOL)waitUntilDone;
	//for non-object-oriented APIs,NULL意味着serial queue即顺
	//序队列,非并发队列(concurrent),该函数是线程安全的
	dispatch_async(dispatch_get_main_queue(), ^{ /*  call  aMethod */  });
####下载

	NSURLSession *session = [NSURLSession sessionWithConfiguration:configuration    
					delegate:nil   
			   delegateQueue:[NSOperationQueue mainQueue]];     
	NSURLSessionDownloadTask *task;   
	task = [session downloadTaskWithRequest:request     
                          completionHandler:^(NSURL *localfile, NSURLResponse *response, NSErr or *error) {    
    /*  yes, can do UI things directly because this is called on the main queue */    
	}];   
	//记得给task加上resume,因为上面并不会马上进行,调用resume方法会使得下载
	//任务马上进行
	[task resume]; 
####UIScrollView
几个重要的属性`contentSize`,`contentOffset`contentOffset这个属性表示当前视图左上角与content的左上角的x,y的偏移量.`scrollView.bounds`表示当前所见区域.

将当前可见区域转化成content所在坐标系的坐标
`CGRect visibleRect = [scrollView convertRect:scrollView.bounds toView:subview];`
在创建UIScrollView时不要忘记设置contentSize,在需要reset的时候也不要忘记reset.最好将设置contentSize的方法发在,随时要改变view大小的方法中.

	//滑至指定区域
	- (void)scrollRectToVisible:(CGRect)aRect animated:(BOOL)animated;
####缩放(Zooming)
要想缩放生效,必须*1.设置最小/最大缩放比例.2.实现代理方法`
- (UIView *)viewForZoomingInScrollView:(UIScrollView *)sender;`*

	//关于缩放的一些属性和方法
	@property (nonatomic) float zoomScale;  !
	- (void)setZoomScale:(float)scale animated:(BOOL)animated;  
	- (void)zoomToRect:(CGRect)zoomRect animated:(BOOL)animated;   
	- (void)scrollViewDidEndZooming:(UIScrollView *)sender  	
	                       withView:(UIView *)zoomView 
	 						atScale:(CGFloat)scale;
####Demo
让imageView自适应image可调用`[imageView sizeToFit]`方法。

如果自己复写了某属性的getter和setter方法就不要在用_someThing而是使用self.someThing.

prepareForSegue方法发生在oulet被设置之前.

NSURLSessionConfiguration有三个类构造函数，这很好地说明了NSURLSession是为不同的用例而设计的。相关介绍可以看这里[iOS 7系列译文：忘记NSURLConnection，拥抱NSURLSession吧！](http://jishu.zol.com.cn/201091.html)

	//返回标准配置，这实际上与NSURLConnection
	//的网络协议栈是一样的，具有相同的共享NSHTTPCookieStorage，共享
	//NSURLCache和共享NSURLCredentialStorage。
	+ defaultSessionConfiguration

	//返回一个预设配置，没有持久性存储的缓存，Cookie或证书。
	//这对于实现像秘密浏览功能的功能来说，是很理想的。
	+ ephemeralSessionConfiguration

	//独特之处在于，它会创建一个后台会话。
	//后台会话不同于常规的，普通的会话，它甚至可以在应用程序挂起，退出，崩溃的
	//情况下运行上传和下载任务。初始化时指定的标识符，被用于向任何可能在进程外
	//恢复后台传输的守护进程提供上下文。
	+ backgroundSessionConfiguration:(NSString *)name
在下载时,最好判断request.URL和imgurl是否一致.
#<a id="10"></a>10.UITableView
####TableView的种类
 - *Plain or Grouped*
 - *Static or Dynamic*:静态一般用作设置之类的,动态是用来加载数据之类的
 - *Devided into sections or not*
 - *Different formats for each row in the table*

####tableView的分布

![](/images/blogimg/stanford/header_footer.png)

####Cell的样式 
 - *Subtitle(UITableViewCellStyleSubtitle)*
 - *Basic(UITableViewCellStyleDefault)*
 - *Right/Left Detail(UITableViewCellStyleValue1/Value2)*

一定要记得为不同的cell设置标识符(`identifier`)。 
####两个协议UITableViewDelegate、UITableViewDataSource
`delegate`用来控制`tableview`如何被显示的,`datasource`为`cell`的各种显示细节提供数据。如果给一个非`TableviewController`添加`tableview`,一定要记住给这个tableview添加`delegate`和`datasource`即

	self.tableview.delegate = self;
	self.tableview.dataSource = self;
dataSource的几个除返回row和section外的重要的方法

	//only for dynamic ones
	- (UITableViewCell *)tableView:(UITableView *)sender
	cellForRowAtIndexPath:(NSIndexPath *)indexPath
	{
		cell = [dequeueReusableCellWithIdentifier: forIndexPath:];
	}
	在prepareForSegue方法中调用indexPathForCell:方法,
	可以获取当前的cell  index
	- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender     
	{      
    	NSIndexPath *indexPath = [self.tableView indexPathForCell:sender];     
    //  prepare  segue.destinationController to display based on information  
    //   about my Model corresponding to  indexPath.row  in  indexPath.section 
	}

#<a id="11"></a>11.Document&Core Data


`NSManagedObject`是`CoreData`所有类的根类

获取`NSManagedObjectContext`,有两种方法

	1.创建UIManagedDocument,然后可以取得其属性managedObjectContext
	2.创建工程时,勾选"use core data"

创建`UIManagedDocument` 

	NSFileManager *fileManager = [NSFileManager defaultManager]; 
	NSURL *documentsDirectory = [[fileManager URLsForDirectory:NSDocumentDirectory 
                                               	inDomains:NSUserDomainMask] firstObject]; 
	NSString *documentName = @“MyDocument”; 
	NSURL *url = [documentsDirectory URLByAppendingPathComponent:documentName]; 
	UIManagedDocument * document = [[UIManagedDocument alloc] initWithFileURL:url]; 
上面代码段创建了`UIManagedDocument` 实例,但没有打开也没有生成相应文件

####怎么打开或者创建`UIManagedDocument` ?

	//1.先检测UIManagedDocument 的底层文件是否存在
	BOOL fileExists =  [[NSFileManager defaultManager] fileExistsAtPath:[url path]];,
	//如果存在,则打开
	[document openWithCompletionHandler:
	^(BOOL success) { /* block to execute when open */  }];
	//如果不存在,则创建,url应该用document.fileURL属性作参数
	[document saveToURL:url 
	   forSaveOperation:UIDocumentSaveForCreating
	   competionHandler:^(BOOL success) { /* block to execute when create is done */  }];  

	//下面是一段完整的代码,documentIsReady是一个自定义方法,在打
	//开或创建成功后才执行的代码
	self.document = [[UIManagedDocument alloc]initWithFileURL: (URL *)url];   
	if ([[NSFileManager defaultManager]  fileExistsAtPath:[url path]]) {   
    [document openWithCompletionHandler: ^(BOOL success) {    
        if (success) [self documentIsReady];   
        if (!success) NSLog(@“couldn’t open document at %@”, url);  
    }];  
	} else {  
    	[document saveToURL:url forSaveOperation:UIDocumentSaveForCreating  
       completionHandler: ^(BOOL success) {    
        if (success) [self documentIsReady];   
        if (!success) NSLog(@“couldn’t create document at %@”, url);  
    	}];  
	}  
	// can’t do anything with the document yet (do it in  documentIsReady).
	//打开或创建成功后,就可使用了,最好对document的状态做个判断
	- (void) documentIsReady  
	{    
   		 if (self.document.documentState ==  UIDocumentStateNormal ) {    
      		 // start using document  
    	}     
	}
####ducument的状态还有下面几个

- UIDocumentStateClosed  
- UIDocumentStateSavingError  
- UIDocumentStateEditingDisabled 
- UIDocumentStateInConflict 
####document的关闭与保存
- #####关闭

    在没有被强引用时就会自动关闭,也可以用下面方法来关闭它,它是异步的
	`[self.document closeWithCompletionHandler:^(BOOL success) {    
    if (!success) NSLog(@“failed to close document %@”,  self.document.localizedName); }];`
- #####保存

	document会自动保存,如果需要手动保存(也是异步的),可以使用下面方法(将会在主线程上被调用)
	`[document saveToURL:document.fileURL   
   forSaveOperation:UIDocumentSaveForOverwriting   
   competionHandler:^(BOOL success) { /* block to execute when save is done */  }];`  
####监听document的managedObjectContext
	//center为[NSNotificationCenter defaultCenter]
	//开启监听
	- (void)viewDidAppear:(BOOL)animated    
	{    
	    [super viewDidAppear:animated];      
	    [center addObserver:self     
                   selector:@selector(contextChanged:)
			           name:NSManagedObjectContextDidSaveNotification
                     object:document.managedObjectContext];  //   不要传空值
	}  
	//移除监听
	- (void)viewWillDisappear:(BOOL)animated     
	{    
  	  	[center removeObserver:self    
                      	  name:NSManagedObjectContextDidSaveNotification    
                        object:document.managedObjectContext];    
   		[super viewWillDisappear:animated];    
	} 
	- (void)contextChanged:(NSNotification *)notification    
	{ 
		//在这里处理context发生改变时需要进行的操作   
    	// notification.userInfo拥有以下key(均为数组)   
    	//NSInsertedObjectsKey 
    	//NSUpdatedObjectsKey 
   	 	//NSDeletedObjectsKey 
	}  
	//在接收到通知时,可以通过调用下面方法来合并数据库的变化
	//- (void)mergeChangesFromContextDidSaveNotification:(NSNotification   *)notification; 
####插入
	NSManagedObjectContext *context = aDocument.managedObjectContext;  
	NSManagedObject *photo =  
    [NSEntityDescription insertNewObjectForEntityForName:@“Photo” inManagedObjectContext: context];  
新插入对象的属性均为nil(除非在Xcode中指定一个默认值)
####删除
调用`[aDocument.managedObjectContext deleteObject:photo];` 即可.在删除该属性后,需要将指向它的强引用置nil.删除之后实体间的关系会自动更新.

也可以写一个分类,添加- (void)prepareForDeletion方法,在该方法中进行一些相应的操作.
####查询(Query)
创建`NSFetchRequest`四要素

- 想要获取的实体
- 一次性获取实体的数目(默认为全部实体)
- 用来指定返回顺序的NSSortDescriptor
- 用来指定获取哪些实体的NSPredicate

创建的代码

	NSFetchRequest *request = [NSFetchRequest fetchRequestWithEntityName:@“Photo”]; 
	request.fetchBatchSize = 20; 
	request.fetchLimit = 100;  
	request.sortDescriptors = @[sortDescriptor]; 
	request.predicate = ...;
#####NSSortDescriptor
`NSSortDescriptor *sortDescriptor = 
 [NSSortDescriptor sortDescriptorWithKey:@“title”  
 							   ascending:YES  
                                selector:@selector`(localizedStandardCompare :)]; 
#####NSPredicate(谓词)
明确指定我们需要从数据库得到的实例.当其为nil即意味着所有的.

	NSString *serverName = @“flickr-5”;  
	NSPredicate *predicate =   
    [NSPredicate predicateWithFormat:@“thumbnailURL contains %@”, serverName]; 
#####NSCompoundPredicate(复合谓词)
	NSArray *array = @ [predicate1, predicate2];  
	NSPredicate *predicate = [NSCompoundPredicate andPredicateWithSubpredicates:array];
#####高级查询
1. 使用KVC(key value coding)

可以使用谓词,比如`@“photos.@count > 5”`,意味着所有大于5的Photographers.`@count`是database自执行的一个函数,还有一些其他类似的.

查看更多请点击[这里](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/KeyValueCoding/Articles/CollectionOperators.html)
####实例属性的获取
	- (id)valueForKey:(NSString *)key; 
	- (void)setValue:(id)value forKey:(NSString *)key;
	//同样也可以用
	valueForKeyPath: /setValue:forKeyPath:这两个方法会自动遵循实体之间的关系
同样我们可以通过Xcode来同时生成头文件(实体属性的property)和实现代码(实现代码里没有对@property进行@synthesize,需要注意)
####分类(Category)
分类并不是子类,分类也有自己的.m和.h文件,同时它不能拥有实例变量,可以有属性变量(@property).分类中最好不要复写本类的原有方法.

####CoreData的线程安全
 `NSManagedObjectContext`是非线程安全的,通常coredata使用 `NSManagedObjectContext`非常迅速,所以多线程操作是没有必要的.

	[context performBlock:^{  // or performBlockAndWait:
    // do stuff with context in its safe queue (the queue it was created on) !
	}];  
#<a id="12"></a>12. Core Data and Table View
#### NSFetchedResultsController 
 `NSFetchedResultsController`和`NSFetchRequest`建立了相应联系,所以只要在`UITableViewDataSource`中使用即可(在UITableviewController中申明`NSFetchedResultsController`属性).比如

	- (NSUInteger)numberOfSectionsInTableView:(UITableView *)sender  
	{  
    	return [[self.fetchedResultsController sections] count];  
	} 
  
	- (NSUInteger)tableView:(UITableView *)sender numberOfRowsInSection:(NSUInteger)section  
	{  
   		return [[[self.fetchedResultsController sections] objectAtIndex:section] numberOfObjects];  
	}  
`UITableviewController`有一个比较常用的方法就是`objectAtIndexPath:`比如

	- (UITableViewCell *)tableView:(UITableView *)sender  
         cellForRowAtIndexPath:(NSIndexPath *)indexPath  
	{ 
  		UITableViewCell *cell = ...; 
   		NSManagedObject *managedObject =   // or, e.g.,  Photo *photo = (Photo *) … 
        [self.fetchedResultsController objectAtIndexPath:indexPath]; 
    	// load up the cell based on the properties of the  managedObject !
    	// of course, if you had a custom subclass, you’d be using dot notation to get them
    	return cell; 
	}
创建`UITableviewController`

	NSFetchedResultsController *frc = [[NSFetchedResultsController alloc] 
    initWithFetchRequest:(NSFetchRequest *)request 
    managedObjectContext:(NSManagedObjectContext *)context 
      sectionNameKeyPath:(NSString *)keyThatSaysWhichSectionEachManagedObjectIsIn 
               cacheName:@“MyPhotoCache”];//cacheName可为nil,这里的cache必须为同一个NSFetchRequest下的cache
`UITableviewController`会"监听"coredata的变化,然后自动更新列表.使用的是KVO(key-value observing)机制,当"监听"到变化,它会发送像下面的消息给它的delegate.

	- (void)controller:(NSFetchedResultsController *)controller 
  	didChangeObject:(id)anObject 
        atIndexPath:(NSIndexPath *)indexPath 
      forChangeType:(NSFetchedResultsChangeType)type 
       newIndexPath:(NSIndexPath *)newIndexPath 
	{ 
    	//这里可以调用tablevie的一些方法来更新列表
	}


#<a id="13"></a>13.其他

 >- 关于视频:我已经打包好了,包括视频,源码,以及每节课的slides以及Assignments。下载请戳[这里](http://pan.baidu.com/s/1gdqTYPd)
 >-本文将不断更新,如果发现文中的问题,请在评论中指出,谢谢。