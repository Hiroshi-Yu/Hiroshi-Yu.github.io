---
title: 拉取iDempiere最新代码后开发环境报错
date:  2024-11-07 +0800
categories: [iDempiere学习] #上级文文档，下级文档
tags: [开发研究]     # TAG
media_subpath : /assets/img/in-post/2024/2024-11-07/
description: 拉取iDempiere最新代码后开发环境报错
image:
    path: lcn.jpg
---

  > 非常遗憾，这篇文章目前没有什么意义，目前仅作为一个失败的参考留下来吧。
{: .prompt-warning }

  > 按照Diego Ruiz的回答还是没有解决这个问题，但无疑mvn clean verify这部还是让人耳目一新
  > 我实际的解决办法是在下述操作前，把Eclipse上的所以工程都删掉，mvn clean verify，后续接iDempiere开发环境搭建手顺解决的问题。
  > 不管怎样，下次遇到同样问题时，我也...I will surely include the mvn clean step when it happens again.
{: .prompt-info }

## 拉取iDempiere最新代码后开发环境报错

### 问题现象

    本来开发环境运行的好好的，由于“手比较欠“，更新了最新代码并跑了最新的数据库脚本之后，发现开发环境报错了。
    以前找过很多次问题原因，都由于时间紧任务重，没有过多的时间进行调查，所以我都是再搞几杯哥伦比亚咖啡喝喝（重新搭建开发环境），至少能确保问题一定被解决。
    正好这次比较有时间，这尝试准备彻底找找问题的原因。翻出去看看，结果不查不知道，果然有人和我有同样的烦恼，并且这次烦恼的现象都是一样的。

    错误现象：
    *. org.adempiere.ui.zk
       POM文件错误

    *. org.idempiere.webservices 
       java代码错误，主要是lib/idempiere-xmlbeans.jar这个jar包有问题。
 
    由于之前环境是好的，其他也都没有问题，iDempiere开发环境搭建中存在瑕的可能行就比较大。

### 信息源

Google Group: [How to properly update the idempiere development environment with changes from the upstream repository?
](https://groups.google.com/g/idempiere/c/wQRRVYGyUhU/m/M2O8AYOUBgAJ)
 
![googleGroupMessage](googleGroupMessage.png)
_原文_

### 思路
参看Diego Ruiz的回答
![googleGroupAnswer](googleGroupAnswer.png)
_解决方案_

### 实践

  > 下面的步骤仅仅为参考GoogleGroup上提示的步骤进行的不完全操作
{: .prompt-info }

1. Setp 1： mvn clean verify
    > 一般情况下看到这个命令就代表着需要大量时间，特别是没有翻墙的情况下。
    其中，尤其是下载github上资源的时候最花费时间，Message例：Downloading from p2: https://idempiere.github.io/binary.file/...

1. Setp 2： Reload target Platform
    > 在开发环境打开org.idempiere.p2.targetplatform下的org.idempiere.p2.targetplatform.target
    > 打开文件后会下载一些文件，等待下载完成后，点击Reload target Platform

1. Setp 3:  Refresh All Projects

1. Step 4： update Maven Projects

1. Setp 5： Clean All Projects

---
