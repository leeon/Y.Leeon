---
layout:     note
title:      Mac日常
description: 使用Mac的日常
---

* 目录(this text will be scraped).
{:toc}


#基础

安装软件主要有四种方式：

+ .app文件拖拽到applications文件夹中
+ 通过AppStore
+ 通过程序自带的安装器
+ 关于Linux的安装方式

> 推荐使用Homebrew进行软件包的管理，一般在`/usr/local/Cellar`目录下。可执行文件存放在`bin`下，库文件在`lib`下，其他资源文件在`share`下。
> brew使用 --prefix安装，将上述资源文件放在一个文件夹里面。**(推荐)**

----------

#日常类

[Alfred](http://www.alfredapp.com/) 快速打开程序的利器

[Evernote](http://www.evernote.com) 笔记同步，知识管理，丰富的chrome插件支持

[Mindmanager]() 强大的思维导图工具

[日历]() Mac自带的日历工具，搭配exchange服务，所平台同步

[Mou](http://mouapp.com/) 可视化Markdown编辑器

[AirMail](http://airmailapp.info/) 最新推出的邮件客户端，做的很用心

[Kindle](https://itunes.apple.com/us/app/kindle/id405399194?mt=12) 这个就不要多说了，这个方便做笔记

[百度云](http://yun.baidu.com) 很好的解决了我的Mac和Windows Phone之间数据同步的问题

[Lightroom](http://www.adobe.com/support/downloads/product.jsp?platform=Macintosh&product=113) 爱好摄影不可缺少

[Photoshop](http://www.adobe.com/support/downloads/product.jsp?product=39&platform=Macintosh) 神器

-----------

#开发类
[Sublime Text 3](http://www.sublimetext.com/) very sexy的编辑器

[Dash](https://itunes.apple.com/cn/app/dash-docs-snippets/id458034879?mt=12) 快捷的查看API工具，可与st3和Alfred结合

[iTerm2](http://www.iterm2.com/) 终端工具，搭配zsh

[zsh](https://github.com/robbyrussell/oh-my-zsh/) 再见，bash

[Homebrew](http://mxcl.github.io/homebrew/) 管理软件包

[Git](http://git-scm.com/‎) 不解释


----------

#快捷键

`option+space` 打开alfred

`cmd+option+esc` 强制退出某个任务 

`Fn + F1` 举例子，使用f1功能键

`cmd+tab`应用间切换

---------


#iTerm 2

iTerm 2 是一款替代原生系统终端工具的利器，它支持多窗口多标签的操作，同时允许用户自定义很多种主题。

我喜欢的特性：

###自定义主题
在preferences -> Profiles -> Colors -> loadPresets下可以导入本地的主题文件。

我使用的主题文件是：[Solarized](https://github.com/leeon/dotFiles/tree/master/res/iterm)

###多窗口管理
我们可以使用多窗口，也可以使用多标签来操作。

分割当前的窗口可以使用：

`cmd + d` 水平分割窗口

`cmd + shift + d` 垂直分割窗口

`cmd + [` 和 `cmd + ]`在分割窗口之间切换



使用多标签页可以采用

`cmd + n` 创建新的标签页

`cmd + 数字`  在不同的标签中切换

`cmd + w`关闭当前的标签页


其他操作:
`cmd + ;` 显示历史命令
`ctrl + u` 清除当前行命令
`ctrl + a` 快速移到行首
`ctrl + e` 快速移到行尾




#zsh
如果觉得每天不断的敲cd命令烦死了人，或者被各种tab扰乱，那么肯能你就是需要换一个终端环境了，那就是zsh.

zsh和原来的bash相比主要的优势就是更多的人性化的操作体验，主要在命令补全方面,一些tips:

+ 输入`d`,zsh会列出最近访问的目录，再输入对应的编号，即可快速切换到某个目录
+ 输入一些命令的一部分，`tab`自动补全

Mac OS 上推荐使用homebrew安装:

{% highlight bash %}
brew install zsh
{% endhighlight %}



### oh-my-zsh
oh-my-zsh是很多程序员使用zsh的一个重要的原因，他是一个管理zsh配置的工具，提供了主题和插件等的管理，下面一段是来自其Github主页的介绍：

> A community-driven framework for managing your zsh configuration. Includes 120+ optional plugins (rails, git, OSX, hub, capistrano, brew, ant, macports, etc), over 120 themes to spice up your morning, and an auto-update tool so that makes it easy to keep up with the latest updates from the community.

安装方法：
可以参照 Github项目主页 ：[链接](https://github.com/robbyrussell/oh-my-zsh)

安装完成后，可以看到你的`~`目录下，多了`.oh-my-zsh`，里面包含了很多主题和插件，可以在`.zshrc`下面进行具体的配置，动手吧。

###参考
我的配置文件放在Github上进行管理，[链接](https://github.com/leeon/dotFiles)

