## Jquery总结 ##
### 1、概述 ###
	JQuery概述：Jquery是继prototype之后又一个优秀的Javascrīpt框架。它是轻量级的js库(压缩后只有21k) ，它兼容CSS3，还兼容各种浏览器（IE 6.0+, FF 1.5+,
	 Safari 2.0+, Opera 9.0+）。jQuery使用户能更方便地处理HTML documents、events、实现动画效果，并且方便地为网站提供AJAX交互。jQuery还有一个比较大的
	优势是，它的文档说明很全，而且各种应用也说得很详细，同时还有许多成熟的插件可供选择。jQuery能够使用户的html页保持代码和html内容分离，也就是说，不用再在
	html里面插入一堆js来调用命令了，只需定义id即可。其宗旨是写更少的代码,做更多的事情。
### 2、Jquery引入 ###
	<script type="text/javascript" src="jquery-1.4.4.js" ></script> 及引入jquery-1.4.4.js 文件
### 3、加载  ###
	Jqery的页面载入事件处理方式：$().ready(function(){  内容。。。。。 });或者$(function(){ 内容。。。。});
### 4、 DOM对象与JQuery对象之间进行相互转化 ###
	DOM对象转化为Jquery对象,ep:var $domcument = $(domcument)
	Jquery对象转化为DOM对象，ep：var domObject = $(".rdc").get(0);
### 5、Jquery中的基本选择器 ###
	id选择器     ID匹配的选择器Jquery对象   例：$("#thed")
	class选择器  class匹配选择器Jquery对象 例：$(".thed")
	元素选择器    根据给定的元素名称获取Jquery对象 例：$("tr")
	 *           匹配所有的元素的Jquery对象 例：$("*")
	并列选择器    匹配所有选择器的Jquery组合对象，用英文的逗号区分，ep：$("tr,tr.rdc")
	其他:查看相关api
### 6、Hello World ###
![](https://i.imgur.com/AiIvLIg.jpg)
### 7、其他列子 ###
![](https://i.imgur.com/YURWTUG.jpg)
	
	关于silblings： 取得一个包含匹配的元素集合中每一个元素的所有唯一同辈元素的元素集合。可以用可选的表达式进行筛选。

## 重点：AJAX ##
#### 同步异步区别 ####
	同步：指发送方发送数据后，等待收方发送相应才放松下一个数据
	异步：指发送方发送收据后，不等待对象响应直接放松下一个数据
#### AJAX简介 ####
	Ajax是(Asynchronous JavaScript And XML)是异步的JavaScript和xml。也就是异步请求更新技术。
#### Jquery实现异步操作 ####
**ajax事件实现方式**
![](https://i.imgur.com/qfTtRak.jpg)
**post事件实现方式**
![](https://i.imgur.com/0FqJXFt.jpg)

### 其他关于AJAX ###
详情链接:[https://www.cnblogs.com/blackgan/p/5860393.html](https://www.cnblogs.com/blackgan/p/5860393.html)