---
title: CSV文件用Excel打开后出现中文乱码问题的解决
date:  2025-9-19 +0800
categories: [iDempiere本地化] #上级文文档，下级文档
tags: [iDempiere本地化]     # TAG
description: CSV文件用Excel打开后出现中文乱码问题的解决
---

## 关于CSV文件导出/导入功能

iDempiere的所有窗口都有标准的数据导出/导入功能。由于导入功能仅支持CSV格式，同时这个格式直接使用导出功能的格式，所以使用CSV格式导出就变得非常重要。

## 问题点

由于经常会在Windows环境中使用Excel功能来打开CSV文件，当CSV文件包含中文时，Excel在打开CSV文件时会先判断CSV文件的编码格式。这个判断方法是微软特有的确认文件先头是否带BOM（Byte Order Mark）格式码。如果读不到BOM的格式码时候，Excel会认为是ANSI格式，从而导致中文乱码。

## 原因分析

使用iDempiere的主流用户都不会出现中文信息，所以无需考虑Windows的特殊要求，所以在产品层级很难对应，需要中文地区单独对应。

## 对应方法

在导出文件时，首先导出BOM格式码（这里仅对应UTF-8，如需对应其他Unicode类型，要根据不同格式匹配不同的BOM格式码）

<div class="box-info" markdown="1">
<div class="title">1. 修改导出部分的程序</div>

**类名**：org.adempiere.impexp.GridTabCSVExporter

**方法**：export

**修改位置**： FileOutputStream fileOut = new FileOutputStream (file); 后

**添加内容**：
```java
//LCN-0001
if ( Env.getAD_Language(Env.getCtx()).compareToIgnoreCase("zh_CN") == 0 
		&& Ini.getCharset().compareTo(Charsets.UTF_8) == 0 )
	fileOut.write(/*BOM*/new byte[]{(byte) 0xEF, (byte) 0xBB, (byte) 0xBF});
```
</div>

<div class="box-info" markdown="1">
<div class="title">2. 修改导入部分的程序</div>

  > 文件头含BOM格式码时会报错，所以要在导出时去除BOM格式码。
{: .prompt-tip }

**类名**：org.adempiere.impexp.GridTabCSVImporter

**方法**：fileImport

**修改位置**： CsvPreference csvpref = new CsvPreference.Builder(quoteChar.charAt(0), delimiterChar.charAt(0), "\r\n" /* ignored */).build(); 后

**添加内容**：
```java
//LCN-0001
if ( Env.getAD_Language(Env.getCtx()).compareToIgnoreCase("zh_CN") == 0 
		&& charset.compareTo(Charsets.UTF_8) == 0 ) {
	byte[] bHeader = {(byte)0xFF,(byte)0xFF,(byte)0xFF};
	filestream.mark(0);
	filestream.read(bHeader, 0, 3);
	if ( !(bHeader[0] == (byte)0xEF && bHeader[1] == (byte)0xBB && bHeader[2] == (byte)0xBF) )
		filestream.reset();
}
```
</div>
