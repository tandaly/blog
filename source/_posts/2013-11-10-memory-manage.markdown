---
layout: post
title: "oc内存管理学习"
date: 2013-11-10 15:34
comments: true
categories: 内存管理
keywords: oc内存管理，wang9262，学习
description: OC内存管理学习笔记

---

   本文是自己对内存管理方面的一些见解，由于接触oc时间不长，基础也不怎么牢靠，做起项目来也不能得心应手。于是就好好得看了别人的博客以及自己将不能理解的地方做了demo来解决自己以前一直有疑虑的地方。本文会持续更新，列出自己在学习过程中遇到的内存管理问题。
<!--more-->
1.深浅拷贝
---------------------------------------
   首先是讲一讲深浅拷贝的问题，先看下表
   {% img left /images/blogimg/copytable.png 409 140 'image' 'images' %}
   
   从表中可以看出，对于任何对象(实现了NSCopying协议)调用copy方法（无论是可变还是不可变对象），将会返回不可变对象，调用mutableCopy将会返回可变对象，且产生新的对象，源对象引用计数不变。
(实现NSCopying协议的- (id)copyWithZone:(NSZone *)zone 方法即可调用copy及mutableCopy) 
  
对不可变的对象调用copy方法(浅拷贝)，这时仅仅为指针拷贝，不会产生新的对象，源对象的引用计数将会加1，此时相当于retain。若此时改变了源对象的内容将不会影响到copy之后得到的对象的内容。如下例  

    NSString *string = [[NSString alloc] initWithFormat:@"age is %i", 10];
    NSLog(@"%zi", [string retainCount]);   //1
    NSString *str = [string copy];
    
	// string:0x10010bd80,str:0x10010bd80指针地址一致
    NSLog(@"string:%p,str:%p",string,str);
    NSLog(@"string:%zi", [string retainCount]);//2
    NSLog(@"str:%zi", [str retainCount]);//2
    
    //改变string的内容，此时没有alloc等操作，所以内存是不需自己管理
    string = @"xyz";
    
    //理论上为0，但是打印出来是-1，这个问题stackoverflow上面说
    //对一个引用计数为零的对象调用retainCount方法是不会得到正确的 
    //结果的。
    NSLog(@"string:%zi", [string retainCount]);   
    
    //string:0x1000025f0,str:0x10010bd80此时string的指针地
    //址发生改变，而str保持原来的指针原因是string指向的内容发生了
    //改变,即使此时将其内容改为和原来一样，即(string = @"age is 
    //10"),其指针还是发生了相应的变化，而不是保持为原来的。
    NSLog(@"string:%p,str:%p",string,str);
    
    //此时str的引用计数仍为2
    NSLog(@"str:%zi", [str retainCount]);
     
    //打印的结果：xyz,age is 10       
    NSLog(@"%@,%@",string,str);
    [str release];
    NSLog(@"str:%zi", [str retainCount]);//1
    [str release];
    
    //理论上为0，打印为随机数，理由同上
    NSLog(@"str:%zi", [str retainCount]);
    
由于水平有限，如果发现本文有问题，可以在评论中指出，谢谢！