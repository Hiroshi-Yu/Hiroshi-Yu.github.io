---
title: iDempiere的本地化 - 插件
date:  2024-10-25 +0800
categories: [iDempiere本地化] #上级文文档，下级文档
tags: [iDempiere本地化]     # TAG
media_subpath : /assets/img/in-post/2024/2024-10-25/
description: iDempiere的本地化介绍 - 插件
image:
    path: lcn.jpg
---

## iDempiere本地化插件的概要介绍

### 命名
 参考哥伦比亚（idempiere-lco）、巴西(idempiere-lbr)等国家本地化的命名规则，中国的插件命名为**idempiere-lcn**。
 
## 插件的使用前提

完成语言包导入等基础操作，例如

> *.导入语言包
> 
> *.导入科目文件
> 
> *.导入城市信息

具体操作方法请参看文档：[iDempiere的本地化](https://idempiere.cn/posts/iDempiere-LCN/)

## 插件安装方法

### 1. 开发环境

#### 代码下载
```
git clone https://gitee.com/idchina/idempiere-lcn.git
```

#### 文件夹构成

``` java
工作文件夹 
|  
+--- idempiere 		// 此处为iDempiere的工作文件夹
|  
|    +--- org.idempiere.parent 
|  
+--- idempiere-lcn  // 本插件
|  
|    +--- cn.idempiere.process.lcn 	// Process的汇总
|    +--- cn.idempiere.test.lcn     // 单元测试
|    ...
|    ...
```
#### 导入插件

1. 在Eclipse中导入插件
1. 在Server.Product中选择idempiere-lcn相关插件
1. 执行并安装插件

### 2. 执行环境

1. 打开Plugin Console
2. 通过Install/Update...功能来安装插件

### 插件功能使用方法

#### 翻译（语言包无法对应的英翻中）

安装插件后，系统中可以看到如下菜单

![菜单路径](idmenu.png)
_菜单路径_

由于不知道在代码中怎么跨Tenant来修改数据，所以该功能需要在System和你自己的Tenant中分别执行一次该Process