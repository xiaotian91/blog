---
title: Ajax经验整理
date: 2017-09-10 10:56:23
tags:
 - JavaScript
 - Ajax
 - Node
categories: 前端技术
---

### 前言

本文将对Ajax的四种上传数据的方式application/x-www-form-urlencoded,application/json,application,multipart/form-data, application/xml进行总结，并且也会涉及到一部分nodejs的知识，应用为主。

<!-- more -->

### GET与POST

GET方式适合数据量小的场景，但由于参数会附加到url后面，而且会被浏览器缓存下来，因此有安全问题，因此涉及到用户密码的发送时，前端需要进行一定的加密保证安全。

POST方式需要服务器端进行一定的配置，例如跨域问题、body请求体解析等等。

### url传参(即Query String)

无论是GET请求还是POST请求，都可以用传统方式直接在url后面用?&的方法传参

		$.ajax({
		    url: 'http://localhost:8888/api/test1?key1=value1&key2=value2',
		    type: 'GET' || 'POST'
		});

GET请求，也可以用ajax方法里的data参数传参，最终也会拼接到url后面

		$.ajax({
		    url: 'http://localhost:8888/api/test1',
		    type: 'GET',
		    data: {
		        fname: 'xt',
		        lname: 'li'
		    }
		});
		
Node用 `req.query` 接收

### body请求体传参(即Form Data)

jQuery的ajax方法默认的向服务器发送的数据格式是`x-www-form-urlencoded`
		
		$.ajax({
			url: 'http://localhost:8888/api/test1',
			type: 'POST',
			contentType: 'application/x-www-form-urlencoded',
			data: { // data传参
				fname: 'xt',
				lname: 'li'
			}
		});
		
Node首先用bodyParser模块设置`urlencoded`为`true`，其他后台语言肯定也有类似的方法设置的，`req.body`接收用data传参方式传递的参数, 如果url里也传了参数，那么也可以用`req.query`收到
	
		var express = require('express');
		var app = express();
		var bodyParser = require('body-parser');
		app.use(bodyParser.urlencoded({ extended: true }));
		
		router.post('/test1', function(req, res) {
			console.log(req.query);
			console.log(req.body);
		});
	

如果设置contentType为json的话，必须用data方式传参，如果传的是js对象前端还需要json化一下或者node里用`app.use(bodyParser.json())`设置一下，然后前端直接传普通js对象即可;使用angular的http服务的话，不需要对contentType做特殊处理，只要保证后端接口是POST的，基本不会有问题
	
		$.ajax({ // jquery方式
			url: 'http://localhost:8888/api/test1',
			type: 'POST',
			contentType: 'application/json',
			data: JSON.stringify(params) // 将对象json化一下
		})
		
		$http({ // angular方式
    		method: 'POST',
    		//url: 'http://localhost:8888/api/test1?name=li&lname=x',
    		url: 'http://localhost:8888/api/test1',
    		params: { // url传参
    			test: '1'
    		},
            data: params // body请求体
        })
 
### 文件上传(multipart/form-data)


首先在页面中给出一个form, 然后用下面的方式向服务器发送Form Data, Node使用`connect-multiparty`中间件，然后可以用`req.files`获取到上传文件的文件名、临时路径等，当然用`req.body`还可以获得其他参数

		 var formData = new FormData($('#form')[0]);
	    formData.append('b', 3);// 还可以添加额外的表单数据
	    $.ajax({
	        type: 'POST',
	        url: 'http://localhost:8888/api/upload',
	        data: formData,
	        contentType: false,// 当有文件要上传时，此项是必须的，否则后台无法识别文件流的起始位置
	        processData: false,// 是否序列化data属性
	        success: function(data) {
	            $('body').append('完成');
	        }
	    });
	    
### 发送xml

向服务端发送xml数据并不常见，不过听说微信和腾讯是这么干的，前端发送xml很简单，只要将contentType设置一下就行，主要Node的bodyParser是不支持xml的，因此需要利用req上定义的事件data来获取http请求流，end事件结束请求流的处理
		
		var xmlData = '<root><classCode>cellphone</classCode><city>GuangDong</city></root>'
		$.ajax({
			url: 'http://localhost:8888/api/test1',
			type: 'POST',
			contentType: 'application/xml',
			data: xmlData
		})
		
		req.rawBody = '';
	    var json = {};
	
	    req.on('data', function(chunk) { // 处理xml数据
	        req.rawBody += chunk;
	    });
	
	    req.on('end', function() {
	        console.log(req.rawBody);
	        json = xml2json.toJson(req.rawBody);
	        console.log(json);
	    })