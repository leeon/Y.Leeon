---
layout:     note
title:      HTML
description: 前端网页基础
---

* 目录(this text will be scraped).
{:toc}


#HTML
`<!DOCTYPE html>`  
这是一个HTML文件

`<head></head>`

`<title>网页标签名</title>`



###引用
`<link href="some_file_you_want_to_link.css" rel="stylesheet" type="text/css" media="all">`


+ href 引用文件路径
+ rel 引用文件和当前文件的关系
+ type 引用文件类型
+ meta 适用的设备


`<script src="js."/>`

###\<meta\>
`<meta>`
网页元数据标签

`<meta name="viewport" content="width=device-width,intial-scale=1.0,maximum-scale=3" >`

控制移动平台的浏览器页面显示

+ width 宽度
+ initial-scale 初始放大
+ maximum-scale 最大放大效果

`<meta charset="utf-8">`

设置网页字符编码


###\<form\>

`<label for="id_of_someinput">输入</label>`

用于和一个输入控件关联，label被点击的时候，会激活控件


`<input type="">`

表单中的输入控件，有不同的输入类型

####text
普通文本输入域

####password
密码输入域

####checkbox
勾选框，可以勾选多个选项
`checked="checked"`表示被勾选。