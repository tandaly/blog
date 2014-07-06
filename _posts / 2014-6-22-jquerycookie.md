---
layout: post
title: jquery.cookie.js的用法
date: 2014-06-28 21:32:10  
---

[英文版](https://github.com/carhartl/jquery-cookie)

## 浏览的支持情况
![](https://camo.githubusercontent.com/1176ea6ce99215a0b8de53defaa91c77669b1921/68747470733a2f2f73617563656c6162732e636f6d2f62726f777365722d6d61747269782f6a71756572792d636f6f6b69652e737667)

## 引入js文件
使用前需要先引入`jquery.cookie.js`文件，代码如下：

```
<script src="jquery.cookie.js"></script>
```

## 用法
创建cookie:

```javascript
$.cookie('name', 'value');
```

创建有效期为7天的cookie:

```javascript
$.cookie('name', 'value', { expires: 7 });
```

创建到期cookie并能用于整个网站的cookie:

```javascript
$.cookie('name', 'value', { expires: 7, path: '/' });
```

读取cookie:

```javascript
$.cookie('name'); // => "value"
$.cookie('nothing'); // => undefined
```

读取所有可用的cookie:

```javascript
$.cookie(); // => { "name": "value" }
```

删除cookie:

```javascript
// 当cookie成功删除后返回true,否则返回false
$.removeCookie('name'); // => true
$.removeCookie('nothing'); // => false

// 需要使用相同的属性(path, domain)来进行操作
$.cookie('name', 'value', { path: '/' });
// 这样是无效的
$.removeCookie('name'); // => false
// 这样就是可以的
$.removeCookie('name', { path: '/' }); // => true
```

> Note：当删除Cookie的时候，您必须通过相同的path，domain和安全选项，用于设置cookie，除非你是依赖于默认选项是。

---
译者：[tandaly](http://tandaly.github.com)
