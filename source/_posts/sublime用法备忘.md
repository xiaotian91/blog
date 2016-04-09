---
title: sublime用法备忘
date: 2015-10-23 11:33:40
tags: 
	- sublime
	- node
categories: 前端技术
---

这篇文章的目的就在于对平时前端开发中使用**Sublime**的一些常用快捷键，插件，还有设置进行记录，不定期更新。Sublime短小精悍，但是能力无限，丝毫不逊于IDE，用了将近一年了，我现在基本是它的铁粉了。

### 一，快捷键

- `cmd+shift+P`打开命令面板（Command Palette）
- `cmd+D` (选中部分文本时) 直接选中下一次出现的该文本
- `cmd+L` 选中一行
- `cmd+A` 全选
- `fn+F5` 排序, 选中CSS属性或JS代码后按F5就可以按字母顺序排序
- `cmd+T`列出所有标签页
- `ctrl+cmd+F` 全屏
- `cmd+R` 文件爬虫 可以列出文档中所有的CSS选择器或HTML元素

<!-- more -->

### 二，插件
作为安装 Sublime插件的必备利器，**Package Control** 是这款编辑器的标配，可以方便开发人员快速安装需要的插件

### 三，设置

- 构建Node执行环境：点击`Tools`->`Build System`->`New build system...`, 输入以下代码：                 
        {
          "cmd": ["node", "$file"],
          "selector": "source.js"
        }
保存为`Node.sublime-build`. `cmd+B`可打开Node执行环境

- [JSHint配置浅析](http://www.tuicool.com/articles/AzIRviR) ,这篇已经讲的很详细了，我就不重复一遍了，自己戳开看