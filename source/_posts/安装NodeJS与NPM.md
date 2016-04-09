---
title: 安装NodeJS与NPM
date: 2015-10-09 11:26:33
tags: 
 - node
 - npm
categories: 前端技术

---
## 前言
今天下午的时候突然心血来潮想把nodejs升级到最新版本, 于是询问度娘找到一个方法, 用npm安装了一个叫n的模块,然后直接在terminal里输入`n stable`, 然后发现node和npm都不能正常运行了. 我的解决方案是将node和npm完全删除干净, 然后再重新安装, 最后成功解决了问题. 写这篇文总结一下, 以防日后再次遇到相同问题时抓瞎, 也给遇到相同问题的朋友一些帮助.
<!--more-->
## 完全删除Node和NPM
要手动删除的东西不少，原则就是将所有**node**, **node_modules**还有**npm**文件夹删光, 这些文件存放的地方如下: 
- `/usr/local/lib`
- `/usr/local/include`
- `brew uninstall node`(如果用brew装的node)
-  任何`/home`下的`local`或`lib`或`include` 
- `/usr/local/bin`
- `/usr/bin`
- `/usr/local/lib/dtrace/node.d`
- `/usr/local/share/man/man1/node.1`
- `/usr/local/share/systemtap/tapset/node.stp`

删除的命令是`sudo rm -rf /dirName/ `
这些基本就是全部，不过不排除还有其他，各位可以根据系统给出的错误提示判断还有哪些需要删除.
***
## 重新安装Node和NPM
- http://brew.sh 是homebrew的官网
- `brew install node`会帮我们下载node，`brew link--overwrite node`会把node link过去, 这里我还遇到一个错误`/usr/local/share/systemtap/tapset is not writable`, 这个是权限的问题, 用`sudo chmod 775 /dirName/`的方法解决
- `curl -L https://www.npmjs.com/install.sh | sh`是安装npm最方便快捷的方法，当然选择重新安装node官网上提供的dmg也是很好的，详情参考[官网](https://github.com/npm/npm)
***
## 升级Node和NPM
`sudo npm install -g npm`升级npm
node更新不频繁, `sudo brew upgrade node`可以搞定升级