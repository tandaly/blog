---
layout: post
title: 利用jsdoc生成javascript api文档
date: 2014-06-13 12:24:28 
---

[英文版](https://github.com/jsdoc3/jsdoc)

[编译版本](http://travis-ci.org/jsdoc3/jsdoc)

jsdoc是一个面向Javascript API文档生成器。

## 安装与用法
你可以在Node.js上安装运行jsdoc。

### Node.js
首先你需要安装Node.js，不知道的上官网[Node.js](http://nodejs.org/)。

#### 利用Node.js安装jsdoc
**稳定版**

```
npm install jsdoc@"<=3.3.0"
```

**开发版**

```
npm install git+https://github.com/jsdoc3/jsdoc.git
```

#### 在Node.js上运行jsdoc

```
./node_modules/.bin/jsdoc yourJavaScriptFile.js
```

如果你在环境变量path中配置了全局访问，那么就可以在命令行任意位置输入以下命令进行编译。

```
jsdoc yourJavaScriptFile.js
```

jsdoc默认输出文档文件的目录是out，你也可以通过```--destination```或者(```-d```)参数命令指定其他输出目录

运行```jsdoc --help ```可以查看命令行参数清单


---
译者：[tandaly](http://tandaly.github.com)


###参考
>* [JS文档生成工具：JSDoc 介绍](http://jianshu.io/p/6c49e2a0cebe)

