---
title: 关于Font Icon(图标字体库和CSS框架)
date:  2025-1-23 +0800
categories: [iDempiere开发] #上级文文档，下级文档
tags: [iDempiere开发]     # TAG
description: 理解iDempiere使用的图标字体库的大致逻辑
media_subpath : /assets/img/in-post/2025/2025-01-23/
---

## 关于Font Icon(图标字体库和CSS框架)
idempiere自v6.2开始，在图形界面中提供了Font Icon能力，让整体的界面档次上了一个新台阶。

官方说明 [Font Icon功能说明](https://wiki.idempiere.org/en/NF6.2_Font_Icons)

## 图标字体库的引用说明

iDempiere这个新特性是使用了[ZKOSS](https://www.zkoss.org/)在zk7之后版本提供的能力。
注: iDempiere v6.2使用的是zk 8.6.0.1

而ZKOSS使用的是图标字体库是著名的[Font Awesome](https://fontawesome.com/)的免费版（可商用）。

## iDempiere v12中的使用说明

### 字体库位置
* 在Target Platform找到zul_10.0.1.jar
* 在web/zul/less/font中可以看到Font Awesome的字体库
* 在web/zul/font中font-awesome.less文件内显示使用的Font Awesome版本为6.4.2

### iDempiere图标和Font Awesome之间的对应关系配置位置

一般情况下，在iDempiere的主题中可以找到配置对应关系的文件，中2缺省使用的iceblue主题中的文件位置如下：

%IDEMPIERE_HOME%\org.idempiere.zk.iceblue_c.theme\src\web\theme\iceblue_c\css\fragment\font-icons.css.dsp


例如，查询按钮定义如下：
```css
.z-icon-Search:before {
	content: "\f002";
}
```
> * .z-icon 是zkoss的要求
> 
> * Search 是iDempiere中对图标的定义
> 
> * f002 是Font Awesome对应的ID