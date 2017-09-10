---
title: 理解JS里让人抓狂的this
date: 2016-04-24 09:32:30
tags: 
 - JavaScript
 - this
 - call
categories: 前端技术

---
### 前言
在阅读各种jQuery插件的过程中经常会看到`this`和`call`, 那时我就知道这两个东西对我理解JS很重要。最近发现自己理解的好像还可以了, 但还是有的时候会感觉有点忘, 然后看了慕课上的一篇文章, 上面提到的记忆方法觉得对我很有用, 因此也就胆子大了一点, 写一篇小文, 权当再复习一遍, 同时与君共赏。 [原文地址](http://www.imooc.com/article/1758)

### 需要理解的核心概念

- **this的指向问题**。调用函数的对象不同, 那么this的指向自然就不同。

- **Function也是对象**, 只要定义了一个函数, 它就会拥有一个`call()`方法，此方法存在于Function对象的**原型对象**中。

- **函数执行时是在某个特定的上下文中执行**，有点像我们平时说话的语境。
<!--more-->

### Function.prototype.call


先看`call()`用法:

	function say(msg) {
		console.log("From " + this + ": Hello " + msg);
	}
	say.call("Bob", "World"); // -> From Bob: Hello World

再分析一下`call()`执行时的步骤:
	
+ Step 1:  把第二参数到最后一个参数作为函数执行时要传入的参数
+ Step 2:  把函数执行时的`this`指向第一个参数
+ Step 3:  在上面这个特殊的上下文中执行函数

**js执行函数时会默认完成以上3步**, 你可以把直接调用函数理解为一种语法糖.如下：

+ 全局环境中:

		`func(args) 等价于 func.call(window, args)`

+ 当作对象的方法调用:
		
		`obj.func(args) 等价于 obj.func.call(obj, args)`

### 丢失的This

	var x = 10;
	var obj = {
		x: 20,
		f: function() {
			console.log(this.x)
			var foo = function() {
				console.log(this.x)
			}
			foo();
		}
	}
	obj.f(); // -> 20 10

这里的问题是, `obj.f`内部的`foo`被当作普通函数调用, 这时`this`指向了`window`, 而通常我们希望`this`指向`obj`。这里就有一个方法用来修正`this`的指向:

	var x = 10;
	var obj = {
		x: 20,
		f: function() {
			var that = this; // //使用that保留当前函数执行上下文的this
			console.log(this.x)
			var foo = function() {
				console.log(that.x) // 此时foo函数中的this仍然指向window，但我们使用that取得obj
			}
			foo();
		}
	}
	obj.f(); // -> 20 20

### 构造函数中的This

	function Person(name) {
		this.name = name;
	}
	var p1 = new Person('Xiaohu');

上面这段在JS高程及各种JS书籍教程中屡见不鲜。这里我们来看一下为什么用了`new`关键字来调用构造函数创建对象时, 构造函数中`this`能够指向它扩展出的对象呢？

	...
	
	var foo = (function(name){
		var _newObj = {
			constructor: Person,
			_proto_: Person.prototype
		};
		_newObj.constructor(name); // 等价于_newObj.constructor.call(_newObj, name)
		return _newObj;
	})();

### jQuery插件中的This

这里截取了一段真实的jQ插件代码, 直接体现了this在不同的上下文中指向的不同。

	$.fn.readmore = function(options) {
		var selector = this.selector;
		options = options || {}
		if (typeof options === 'object') {
			return this.each(function(){
				...
				options.selector = selector;
				$.data(this,'plugin_' + readmore, new Readmore(this, options));
			})
			
		}
	}
	

代码第二行的`this`指向了传入的**jQuery对象**, 而在each函数体的内部的`this`指向的其实是每个被选中的**DOM元素**。

	Readmore.prototype = {
		init: function() {
			var current = $(this.elem);
			current.data({ // 储存数据
				defaultHeight: this.options.collapsedHeight, // 默认高度200px
				heightMargin: this.options.heightMargin
			});
		},
		...
	}

因此在构造函数的内部需要将DOM元素重新转换为jQuery对象, 才能使用像attr()，data()等jQuery方法。

### 总结

**总结起来就是, 把函数调用`foo();`翻译成`foo.call();`的形式, 明确call的第一个参数, 即"谁"调用了这个函数, 就能明确this的指向。**

+ 全局环境中:

		`func(args) 等价于 func.call(window, args)`

+ 当作对象的方法调用:
		
		`obj.func(args) 等价于 obj.func.call(obj, args)`
