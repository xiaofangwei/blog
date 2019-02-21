---
title: java中的foreach与js中的for in的区别
date: 2017-12-05 12:18:47
tags: for循环
---
js里的for in循环定义如下：

代码如下:
```javascript	
	for(var variable in obj) { ... }
```
**obj可以是一个普通的js对象或者一个数组。如果obj是js对象，那么variable在遍历中得到的是对象的属性的名字，而不是属性对应的值。如果obj是数组，那么variable在遍历中得到的是数组的下标。**

遍历对象实验：

 代码如下:
```javascript
	var v = {};  
	v.field1 = "a";  
	v.field2 = "b";  
	for(var v in v) {  
    	console.log(v);  
	}
```
  
控制台下输出：
```
	field1
	field2
```
遍历数组实验：

代码如下:
```javascript
	var mycars = new Array()
	mycars[0] = "Saab"
	mycars[1] = "Volvo"
	mycars[2] = "BMW"
	  
	for (var x in mycars){
	  console.log(x);
	}
```
  
控制台输出：
```
	0
	1
	2
```
**拿java的foreach循环来做对比，有两大差别。首先java的foreach循环不会去枚举一个java对象的属性。其次，java的foreach循环枚举一个数组或者任何实现了Iterable接口的对象的时候，for(Object o : list), 对象o得到的是list一个元素，而非在列表中的下标。**

java的遍历代码就不贴出来了。经常写后台代码，foreach循环很熟悉。写前台js代码的时候，难免会套用java的语法，所以第一次用js的for in循环的时候犯错了。
