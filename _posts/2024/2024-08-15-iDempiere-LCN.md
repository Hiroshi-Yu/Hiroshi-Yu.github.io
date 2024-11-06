---
title: iDempiere的本地化
date:  2024-08-15 +0800
categories: [iDempiere本地化] #上级文文档，下级文档
tags: [iDempiere本地化]     # TAG
media_subpath : /assets/img/in-post/2024/2024-08-15/
description: iDempiere的本地化介绍
image:
    path: lcn.jpg
---

## iDempiere本地化的概要介绍

### 简称 - LCN
 参考哥伦比亚、巴西等国家本地化的命名规则，中国本地化的Code Name为LCN(Localization China)。
 
 大小写使用建议
 - 在文档类中使用大写LCN，如本文档。
 - 在代码中使用小写lcn，如package命名：```package org.idempiere.process.annotation.lcn; ```

 _目前对使用 ```cn.idempiere``` 还是 ```org.idempiere.lcn``` ，尚未做最终判断_

### 构成
> 按完成度从高至低排序

1. UI中文化

   gitee：[UI中文化](https://gitee.com/idchina/chinese-translation/tree/master/zh_CN)


1. CoA（Chart of Account）

   gitee：[会计科目表](https://gitee.com/idchina/chinese-translation/tree/master/Utils/iDChina_AccountingCN.xlsx)

1. 主数据

   gitee：[省市主数据](https://gitee.com/idchina/chinese-translation/tree/master/Utils/sql_region_city.sql) 
   > 注：_**仅可在Postgresql中执行。** 应该没有使用特殊语法，在Oracle中应也可直接执行Insert Into语句_

1. **(尚无)** 本地化功能

   后续罗列清单，逐一实现各需要本地化的功能

1. **(尚无)** DEMO

   仿GardenWorld创建一个符合国内商业习惯的demo用tenant

## 操作手册

### 1. UI中文化

#### 1.1. 实施前提
> 安装iDempiere环境
> 
> [iDempiere官网 执行环境 or 开发环境安装](https://wiki.idempiere.org/en/Installing_iDempiere)
{: .prompt-tip }

#### 1.2. 登录系统（System）

#### 1.3. 配置中文为系统语言并可用中文登录

<details class="details-block" markdown="1">
<summary> 使用功能：Language </summary>
菜单路径 System Admin->General Rules->System Rules
</details>
{: .box-info }

>
| 概要说明 | 字段 | 操作 |
| :-- | :-- | :-- |
| 1. 查询中国信息维护界面 | Language | 输入 zh_CN |
| 2. 配置中国语言并保存 | System Language | 勾选 |
| - | Login Locale | 勾选 |
| 3. 重建中文翻译 | Language Maintenance | 点击 |
| - | Maintenance Mode | 选择Re-Create Translation后执行 |
{: .box-tip }

#### 1.4. 导入中文配置文件并切换系统语言

>从[UI中文化](https://gitee.com/idchina/chinese-translation/tree/master/zh_CN)下载文件夹，并把文件夹放到IDEMPIERE_HOME下。
{: .box-tip }

---
>
<details class="details-block" markdown="1">
<summary> 使用功能：Translation Import/Export </summary>
菜单路径 System Admin->General Rules->System Rules
</details>
{: .box-info }
>
| 概要说明 | 字段 | 操作 |
| :-- | :-- | :-- |
| 1. 配置参数 | Import/Export | 选择Import |
| - | Language | 选择Chinese (China) |
| - | Folder | 选择从[UI中文化]下载过来放到IDEMPIERE_HOME下的文件夹 |
| 2. 确认结果无错 | OK | 点击执行 |
{: .box-tip }

---
>
<details class="details-block" markdown="1">
<summary> 使用功能：Synchronize Terminology 并直接执行</summary>
菜单路径 System Admin->General Rules
</details>
{: .box-info }

---
>
<details class="details-block" markdown="1">
<summary> 使用功能：Cache Reset 并直接执行</summary>
菜单路径 System Admin->General Rules
</details>
{: .box-info }

---
> 重新登录系统时，系统登录界面的Language字段可以选中文，选择中文后，理论上翻译文件中的所有中文字段都可以显示了。
{: .box-tip }

### 2. CoA（Chart of Account）

#### 2.1. 科目表的准备

>从gitee下载[会计科目表](https://gitee.com/idchina/chinese-translation/tree/master/Utils/iDChina_AccountingCN.xlsx)文件。由于iDempiere仅支持CSV文件的上传，需要手工的把Sheet：AccountingCN另存为CSV文件。
>
_注：如果没有Excel工具，可以直接下载[会计科目表CSV文件](https://gitee.com/idchina/chinese-translation/blob/master/Utils/iDChina_AccountingCN.csv)。由于可能会有操作遗漏等问题，这个CSV文件可能比Excel晚一个版本_
{: .box-tip }

<details class="details-block" markdown="1">
<summary>补充信息：科目表文件的说明</summary>
这个文件是一个Excel文件，公有3个Sheet，这里做一个简单的说明
- 【AccountingCN】             对象科目表信息
- 【AccountingCN-工作前版本】   由历届大神编辑的科目表文件，我在QQ组中下载的。我也是基于这个版本做的更新
- 【AccountingUS】             英文科目表，用于比对用
</details>
{: .box-info }

#### 2.2. 创建新租户、导入科目表

> 创建新租户(Initial Tenant Setup)的方法可以参考以下信息：
> 
> 购买书籍(_根据你自己的情况任意添加手动狗头_)[《Adempiere 3.4 Erp Solutions》](https://www.amazon.com/exec/obidos/ASIN/1847197264/acmorg-20)后参考相关章节创建Tenant。
> 
> 这本书的内容虽然还是ADempiere的界面构成，但现在仍然可用。但对iDempiere做了功能增强部分需要参看官网说明 - [NF2.1 Ease Initial Client Setup](https://wiki.idempiere.org/en/NF2.1_Ease_Initial_Client_Setup)
{: .box-tip }

>
- 创建新租户(Initial Tenant Setup)
  * 在iDempiere中建议选择【Use Default CoA】，这样会自动创建缺省科目，不需要使用科目表文件。
  * 也可以按照ADempiere时的操作方法，在字段【Chart of Accounts File】中选择我们的科目表CSV文件即可创建缺省科目的信息后创建新租户。
- 导入科目表（Import File Loader）
  在该功能中选择我们的科目表CSV文件导入信息
{: .box-tip }

### 3. 主数据

#### 3.1. 主数据的准备

> 下载 gitee：[省市主数据](https://gitee.com/idchina/chinese-translation/tree/master/Utils/sql_region_city.sql) 
{: .box-tip }

#### 3.2. 添加主数据

> 打开pgadmin4等工具，连接到iDempiere数据库中后执行该SQL文件
{: .box-tip }

#### 3.3. 修改UI中文化文件也无法更改的界面信息

> 请使用插件并执行相关功能即可，请参考本地化插件的说明信息
> [https://idempiere.cn/posts/iDempiere-LCN-plugins/](https://idempiere.cn/posts/iDempiere-LCN-plugins/)
{: .prompt-danger }
