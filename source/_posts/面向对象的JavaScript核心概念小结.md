---
title: 面向对象的JavaScript核心概念小结
date: 2016-04-18 20:49:45
tags: 
 - JavaScript
 - 面向对象
categories: 前端技术

---
## 前言
本文可视作《JavaScript设计模式与开发实践》第一部分第一章和《JavaScript高级程序设计》第六章的学习笔记。简单概括了JavaScript面向对象的三大核心概念（多态，封装，继承）背后的核心思想。 


## 鸭子类型 Duck Typing

> 从前在JavaScript王国里，有一个国王，他觉得世界上最美妙的声音就是鸭子的叫声，于是国王召集大臣，要组建一个1000只鸭子组成的合唱团。大臣们找遍了全国，终于找到999只鸭子，但是始终还差一只，最后大臣发现有一只非常特别的鸡，它的叫声跟鸭子一模一样，于是这只鸡就成为了合唱团的最后一员。

这个故事形象地暗示了鸭子类型的核心思想，**只关注对象的行为（鸭子的叫声），而不关注对象本身（是鸭还是鸡）**。
<!--more-->
	var joinChoir = function(animal) {
		if (animal && typeof animal.duckSinging === 'function' )
		...
	}


## 多态 Polymorphism

> 在电影的拍摄现场，当导演喊出“action”时，主角开始背台词，照明时负责打灯光，后面的群众演员假装中枪倒地，道具师往镜头里撒上雪花。在得到同一个消息时，每个对象都知道自己应该做什么。

这个故事形象地暗示了多态性的核心思想，**将“做什么”（导演喊action）和“谁去做以及怎样去做”（各剧组成员各自调用自己的方法）分离**。


	var action = function(member) {
		if (member && typeof member.action === 'function') {
			member.action();
		}
	}

	var actor = {
		action: function() {
			console.log('主角开始背台词');
		}
	}
	
	var lighter = {
		action: function() {
			console.log('灯光师开始布光');
		} 
	}

## 封装 Encapsulation

> 一台榨汁机上的“开始榨汁”按钮就可以被视作一个“接口”，我们并不关心榨汁机内部接收到“开始榨汁”这一命令后是怎样工作的，我们只是调用“开始榨汁”这一方法，得到一杯新鲜的橙汁。

**封装不仅仅是隐藏数据，还包括隐藏实现细节、设计细节以及隐藏对象的类型等**。

JavaScript中只能依赖变量的作用域来实现**数据封装**，而且只能模拟出`public`和`private`这2种封装性。

	var obj = (function(){
		var _name = '赵本山'; // private
		return {
			getName: function() { // public
				return _name;
			}
		} 
	})();
	
	console.log(obj.getName()); // -> 赵本山
	console.log(obj._name); // -> undefined





## 继承 Inheritance

> 正如每个吸血鬼都有一个吸血鬼祖先, 所有JavaScript对象都有一个共同祖先，那就是`Object.prototype`

我们来还原一下 `var obj = new Person('Cool');`创建对象的过程。 

	... 省略Person构造函数
	
	var objFactory = function() {
		var obj = new Object(), // 从Object.prototype上克隆一个空的对象
			Constructor = [].shift.call(arguments); // 取得构造函数
		obj.__proto__ = Constructor.prototype; // 指向构造器的原型，而不是Object.prototype
		var ret = Constructor.apply(obj,arguments); // 调用构造器创建对象并设置属性
		
		return typeof ret === 'object' ? ret : obj;
	}
	
	console.log(objFactory(Person, 'Cool')) 

---
我们来看一下**寄生组合式继承**, 该例明确地表明了JavaScript继承的核心思想是，将**构造函数的原型指向另一构造函数的原型，实现原型委托继承**。

	function SuperType(name) {
		this.name = name;
	}
	SuperType.prototype.getName = function() {
		return this.name;
	}
	
	function SubType(name, gradesReport) {
		SuperType.call(this, name);
		this.gradesReport = gradesReport;
	}
	inheritPrototype(SubType, SuperType);
	
	function inheritPrototype(subType, superType) {
		var prototype = Object(superType.prototype);
		prototype.constructor = subType;
		subType.prototype = prototype;
	}


			
+ `SuperType.call(this, name)` 借调SuperType构造函数创建对象并设置属性
+ `var portotype = Object(superType.prototype); subType.prototype = prototype;` 继承SuperType构造函数的原型对象
+ `prototype.constructor = subType;` 将constructor指向正确的构造函数


