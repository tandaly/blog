---
layout: post
title: "VM10装Mac OS X 10.9.3及更新到Mac OS X 10.10"
date: 2014-06-06 23:30
comments: true
categories: Mac OS X
keywords: Mac OS X,虚拟机,VM,10.9.3
description: VM10装Mac OS X 10.9.3,更新Mac OS X 10.10
---
最近WWDC放出终极大招——新的编程语言Swift(雨燕),导致一大波程序员的围观和跃跃欲试。当然了,工欲善其事,必先利其器,所以对于那些没有Mac又想要尝鲜的小伙伴肯定很为难。但是，请放心，本文教你如何在Windows下也能体验Mac的滋味，当然咯，最主要的还是体验新的语言Swift。好了话不多说，直接开始，贴图比较多，耐心等待图的刷出。
<!--more-->  

1. [所需软件](#1)
2. [基本步骤](#2)

	2.1. [虚拟机的安装](#2.1)

	2.2. [插件安装](#2.2)

	2.3.  [Mac OS X 10.9.3的安装](#2.3)

	2.4.  [VMTool以及Darwin6.0.3的安装](#2.4)

	2.5.  [设置共享文件夹](#2.5)
	
	2.6 . [升级Mac OS X Yosemite](#2.6)
	
	2.6.1.[升级方法1](#2.6.1)
	
	2.6.2.[升级方法2](#2.6.2)
3. [其他的一些tips](#3)
4. [后续](#4)

##<a id="1"></a>一、所需软件
- [VMware Workstation 10 及注册机](http://pan.baidu.com/s/1gdwyIcj)

- [装Mac OS X插件](http://pan.baidu.com/s/1EPPh0)

- [Mac OS X 10.9.3镜像](http://pan.baidu.com/s/1ntJTd49)

- [10.10镜像 iso版](http://yun.baidu.com/s/1sj6THFJ)

- [10.10镜像 dmg版](http://yun.baidu.com/s/1bniZGmz)

- [Xcode5.1.1](http://pan.baidu.com/s/1ntyozI5)

- [Xcode6 Beta](http://pan.baidu.com/s/1c0pdrOW)


基本上所用到的软件都在上面了，点击即可下载，下面开始进入正题。

##<a id="2"></a>二、基本步骤
###<a id="2.1"></a>1.虚拟机的安装
下载上面的Vmare Workstation，以管理员身份运行，安装，一直下一步即可，别放C盘以免拖慢系统速度。VM安装好后，开始用注册机来激活，输入激活码即可。
###<a id="2.2"></a>2.插件安装
步骤一完成后，打开任务管理器，找到服务项，选择按名称排序，将框中四项全部停止运行。

![](/images/blogimg/vm/01.png)

然后打开下载的插件，解压`unlock-all-v120.zip`文件，找到`
unlock-all-v120\windows\install.cmd `，右键以管理员身份运行，等待运行完即可。
###<a id="2.3"></a>3.Mac OS X 10.9.3的安装
相信你以上步骤已做好了，开启下面旅程吧。基本都是图片，所以很容易，just follow。

![](/images/blogimg/vm/02.png)
![](/images/blogimg/vm/03.png)
![](/images/blogimg/vm/04.png)
![](/images/blogimg/vm/05.png)
![](/images/blogimg/vm/06.png)
![](/images/blogimg/vm/07.png)
![](/images/blogimg/vm/08.png)
![](/images/blogimg/vm/09.png)
![](/images/blogimg/vm/10.png)
![](/images/blogimg/vm/11.png)
![](/images/blogimg/vm/12.png)
![](/images/blogimg/vm/13.png)
![](/images/blogimg/vm/14.png)
![](/images/blogimg/vm/15.png)
![](/images/blogimg/vm/16.png)
![](/images/blogimg/vm/17.png)
![](/images/blogimg/vm/18.png)
![](/images/blogimg/vm/19.png)
![](/images/blogimg/vm/20.png)

![](/images/blogimg/vm/21.png)

然后出现苹果标志，稍等片刻，会出现语言选择，选择简体中文再下一步

![](/images/blogimg/vm/22.png)

![](/images/blogimg/vm/23.png)

![](/images/blogimg/vm/24.png)

然后点击左上角的`磁盘工具`，选择`退出磁盘工具`。然后在安装界面点击`继续`,再点击`安装`

![](/images/blogimg/vm/25.png)

![](/images/blogimg/vm/26.png)

![](/images/blogimg/vm/27.png)

![](/images/blogimg/vm/28.png)

![](/images/blogimg/vm/29.png)

![](/images/blogimg/vm/30.png)

![](/images/blogimg/vm/31.png)

安装完成，直接重启即可。至此，所有的安装步骤都完成了。
###<a id="2.4"></a>4.VMTool以及Darwin6.0.3的安装

![](/images/blogimg/vm/32.png)
![](/images/blogimg/vm/33.png)
![](/images/blogimg/vm/34.png)
![](/images/blogimg/vm/35.png)
![](/images/blogimg/vm/36.png)
![](/images/blogimg/vm/37.png)
![](/images/blogimg/vm/38.png)

###<a id="2.5"></a>5.设置共享文件夹

![](/images/blogimg/vm/39.png)
![](/images/blogimg/vm/40.png)

共享文件夹的说明：由于虚拟机无法访问本机的硬盘，所以需要设置共享文件夹来方便虚拟机读取电脑的物理内存。

###<a id="2.6"></a>6.升级Mac OS X Yosemite
关于10.10 iso版需要说明一下,需将文件夹中两个文件都下载,两个都下载完后才能解压,解压`OS X 10.10 DP1 14A238x.zip`即可,`OS X 10.10 DP1 14A238x.z01`可以不用管,这是由于百度网盘上传文件大小设置,所以我将其切割成了两份。
####<a id="2.6.1"></a>升级方法1
下载10.10的iso镜像文件，将CD弹出，设置为10.10的镜像文件，然后连接CD即可，类似VMTool的安装。直接点击安装或下一步即可。
####<a id="2.6.2"></a>升级方法2
下载10.10的dmg镜像文件,将其拷入之前设置的共享文件夹内,然后在虚拟机中找到该dmg,双击运行即可。

##<a id="3"></a>三、其他的一些tips
当然你也可以直接装10.10(没有亲测),按照装10.9.3得步骤,应该也是可以的.可以尝试下,如果不能安装,可以再从10.9.3升级.如果直接安装,一定要用iso文件,很多人反馈说开启虚拟机就直接黑屏,主要是因为用的dmg镜像文件所导致。


当再次打开虚拟机时，发现进入的是下面界面

![](/images/blogimg/vm/41.png)

之前装好的虚拟机不见了，不要着急,按照下图设置即可

![](/images/blogimg/vm/42.png)

是不是又出现了,接下来就尽情的玩耍吧，Xcode 6 是必不可少的啦。在安装的过程中,由于电脑配置等原因,安装的时间不一,所以大家要耐心等待下。

##<a id="4"></a>四、后续
我的电脑是win8，但是win7步骤基本一致，没有太大区别。还有一个就是AMD的貌似安装不了，只能intel的，如果你有好的方法，也可以分享下。如果你有疑问可在下面留言，也可以Email我。Have fun！
