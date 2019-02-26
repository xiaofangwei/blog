---
title: JS的时间获取
date: 2017-11-22 13:43:49
top: 1
tags: javascript
---

#### JS获取系统时间
  **JS自己拥有一个`Date`类来获取时间**
```javascript	
	var date = new Date();
```
 **如果直接输出的话，会是这样：**
```
	2017-11-03 13:59:25Wed Nov 22 2017 13:59:25 GMT+0800 (中国标准时间)
```
*可想而知，我们并不需要这么多的东西，所以`Date`类还给我们提供了一些方法来对时间进行格式化：*
```javascript
	var year = date.getFullYear();      //获取年份 输出格式为“2017”
	var month = date.getMonth();        //获取月份 输出格式为“10”（类定义0~11为1~12月，所以输出10位11月，实际使用注意在后面加1）
	var day = date.getDate();         //获取日子（1~31）
	var week = date.getDay();        //获取星期 （1~7）
	var hour = date.getHours();        //获取小时数
	var minute = date.getMinutes();     //获取分钟数
	var second = date.getSeconds();      //获取秒钟
```
#### 格式化时间
**因为JS中不提供类似于JAVA中的`SimpelDateFormat`类，所以我们需要自己写一个格式化时间的方法**
```javascript
	<script>
	  function Format(){
		var newDate=new Date();		
		var year=newDate.getFullYear();
		var month=(newDate.getMonth()+1)<10?("0"+(newDate.getMonth()+1)):(newDate.getMonth()+1);
		var day=newDate.getDate()<10?("0"+newDate.getDate()):newDate.getDate();
		var day=newDate.getDay()<10?("0"+newDate.getDay()):newDate.getDay();
		var hours=newDate.getHours()<10?("0"+newDate.getHours()):newDate.getHours();
		var minuts=newDate.getMinutes()<10?("0"+newDate.getMinutes()):newDate.getMinutes();
		var seconds=newDate.getSeconds()<10?("0"+newDate.getSeconds()):newDate.getSeconds();
		document.write(year+"-"+month+"-"+day+" "+hours+":"+minuts+":"+seconds);
      } 
    </script>
```
输出为：
```
	2017-11-22 14:15:34
```
因为项目需要从前端获取时间存到数据库中，所以需要这种格式。后面再写一下时间字符串如何转化为Date的格式存进数据库。