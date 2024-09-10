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

> **没有开发环境、开发知识的话，暂不要使用**
{: .prompt-danger }

> 这部分功能还没有整合到插件中，现把代码贴出来，请按需使用。
{: .box-tip }

<details class="details-block" markdown="1">
<summary>代码信息</summary>
- _Packag名和Class名都是暂定的，后续会有大的调整_
- _这个Process需要在System和新建租户中分别执行_

``` java
/***********************************************************************
 * This file is part of iDempiere ERP Open Source                      *
 * http://www.idempiere.org                                            *
 *                                                                     *
 * Copyright (C) Contributors                                          *
 *                                                                     *
 * This program is free software; you can redistribute it and/or       *
 * modify it under the terms of the GNU General Public License         *
 * as published by the Free Software Foundation; either version 2      *
 * of the License, or (at your option) any later version.              *
 *                                                                     *
 * This program is distributed in the hope that it will be useful,     *
 * but WITHOUT ANY WARRANTY; without even the implied warranty of      *
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the        *
 * GNU General Public License for more details.                        *
 *                                                                     *
 * You should have received a copy of the GNU General Public License   *
 * along with this program; if not, write to the Free Software         *
 * Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston,          *
 * MA 02110-1301, USA.                                                 *
 * -                                                                   *
 * Contributors:                                                       *
 * - Yang Yu                                                           *
 * -                                                                   *
 **********************************************************************/
package org.idempiere.process.annotation.lcn;

import java.util.List;
import org.compiere.model.MConversionType;
import org.compiere.model.MGLCategory;
import org.compiere.model.MSequence;
import org.compiere.model.MWarehouse;
import org.compiere.model.Query;
import org.compiere.util.DB;
import org.compiere.util.Env;
import org.compiere.util.Msg;
import org.adempiere.base.annotation.Process;
import org.compiere.process.SvrProcess;

@Process
public class InitialTenantData  extends SvrProcess{

	private boolean isSysTenant = false;
	private boolean	p_isInitialConfig = true;
	private boolean	p_isInitialMaster = true;
	private boolean	p_isInitialTrx = true;

	/**
	 *  Prepare - e.g., get Parameters.
	 */
	@Override
	protected void prepare() {
		
		isSysTenant = (Env.getAD_Client_ID(getCtx()) < MSequence.INIT_NO);
		
//		for (ProcessInfoParameter parameter : getParameter()) {
//			String name = parameter.getParameterName();
//			switch (name) {
//			case "isInitialConfig":
//				p_isInitialConfig = parameter.getParameterAsBoolean();
//				break;
//			case "isInitialMaster":
//				p_isInitialMaster = parameter.getParameterAsBoolean();
//				break;
//			case "isInitialTrx":
//				p_isInitialTrx = parameter.getParameterAsBoolean();
//				break;
//			default:
//				MProcessPara.validateUnknownParameter(getProcessInfo().getAD_Process_ID(), parameter);
//			}
//		}
	}	//	prepare
	
	@Override
	protected String doIt() throws Exception {
		
		if (isSysTenant) { 
			initTrlConversionType();
		} else {
			if (p_isInitialConfig) {
				initBaseInfo();
				initTrlGLCategory();
				initTrlDoctype();
			}
			if (p_isInitialMaster) {
				initWarehouse();
			}
			if (p_isInitialTrx) {
				;
			}
		}	
		return "@Ok@";
	}

	private boolean initBaseInfo() {
		String update = "UPDATE AD_Client SET ismultilingualdocument = 'Y' WHERE AD_Client_ID = ?";
		DB.executeUpdateEx(update,
				new Object[]{Env.getAD_Client_ID(getCtx())},
				this.get_TrxName());
		return true;
	}
	
	private boolean initTrlGLCategory() {
		List<MGLCategory> gls = new Query(getCtx(),MGLCategory.Table_Name, null, get_TrxName())
				.setClient_ID(true)
				.setOnlyActiveRecords(true)
				.list();
		for (MGLCategory gl : gls) {
			String name = gl.getName();
			switch (name) {
			case "None":
				gl.setName("无");
				break;
			case "Standard":
				gl.setName("标准");
				break;
			case "Manual":
				gl.setName("手工");
				break;
			case "AR Invoice":
				gl.setName("应收账款 - 发票");
				break;
			case "AR Receipt":
				gl.setName("应收账款 - 收款");
				break;
			case "Material Management":
				gl.setName("物料管理");
				break;
			case "AP Invoice":
				gl.setName("应付账款 - 发票");
				break;
			case "AP Payment":
				gl.setName("应付账款 - 付款");
				break;
			case "Cash/Payments":
				gl.setName("现金/付款");
				break;
			case "Manufacturing":
				gl.setName("生产");
				break;
			case "Distribution":
				gl.setName("分销");
				break;
			case "Payroll":
				gl.setName("工资");
				break;
			default:
				;
			}
			gl.saveEx();			
		}
		return true;
	}

	private boolean initTrlDoctype() {
		String params[][] = {
				{"AP CreditMemo","应付贷项通知单","应付贷项通知单"},
				{"AP Invoice","应付发票","应付发票"},
				{"AP Payment","付款单","付款单"},
				{"AR Credit Memo","应收贷项通知单","应收贷项通知单"},
				{"AR Pro Forma Invoice","应收形式发票","应收形式发票"},
				{"AR Invoice","应收发票","应收发票"},
				{"AR Invoice Indirect","间接应收发票","间接应收发票(POS)"},
				{"AR Receipt","收款单","收款单"},
				{"Allocation","支付分配单","支付分配单"},
				{"Bank Statement","银行对账单","银行对账单"},
				{"Cash Journal","现金日记账","现金日记账"},
				{"Distribution Order","分销订单","分销订单"},
				{"Fixed Assets Addition","资产新增单","资产新增单"},
				{"Fixed Assets Disposal","资产处置单","资产处置单"},
				{"Fixed Assets Depreciation","资产折旧单","资产折旧单"},
				{"GL Document","总账单据","总账单据"},
				{"GL Journal","凭证","凭证"},
				{"GL Journal Batch","凭证批","凭证批"},
				{"Payroll","薪酬单","薪酬单"},
				{"Manufacturing Cost Collector","制造成本归集单","制造成本归集单"},
				{"Cost Adjustment","成本调整单","成本调整单"},
				{"Internal Use Inventory","内部领用单","内部领用单"},
				{"Physical Inventory","库存调整单","库存调整单"},
				{"Material Movement","调拨单","调拨单"},
				{"Material Production","材料生产单","材料生产单"},
				{"MM Customer Return","销售退货单","销售退货单"},
				{"MM Receipt","收货单","收货单"},
				{"MM Shipment","发货单","发货单"},
				{"MM Shipment Indirect","间接发货单","间接发货单(POS)"},
				{"MM Vendor Return","采购退货单","采购退货单"},
				{"Maintenance Order","维护工单","维护工单"},
				{"Manufacturing Order","生产工单","生产工单"},
				{"Quality Order","质检单","质检单"},
				{"Match Invoice","发票匹配单","发票匹配单"},
				{"Match PO","采购匹配单","采购匹配单"},
				{"Project Issue","项目投放单","项目投放单"},
				{"Purchase Order","采购订单","采购订单"},
				{"Vendor Return Material","采购退货授权","采购退货授权"},
				{"Purchase Requisition","请购单","请购单"},
				{"Binding offer","报价单","报价单"},
				{"Credit Order","信用订单","信用订单"},
				{"Customer Return Material","销售退货授权","销售退货授权"},
				{"Non binding offer","销售建议","销售建议"},
				{"POS Order","POS订单","POS订单"},
				{"Prepay Order","预付款订单","预付款订单"},
				{"Standard Order","标准订单","标准订单"},
				{"Warehouse Order","库存订单","库存订单"}
		};
		
		String update = "UPDATE C_Doctype_Trl SET name = ? ,printname = ? WHERE AD_Client_ID = ? AND AD_Language = ? AND name = ?";
		for (int i = 0; i < params.length ; i++ ) {
			DB.executeUpdateEx(update,
							new Object[]{params[i][1],params[i][2],Env.getAD_Client_ID(getCtx()),Env.getAD_Language(this.getCtx()),params[i][0]},
							this.get_TrxName());
		}
		return true;
	}
	
	private boolean initWarehouse() {
		String m_lang = Env.getAD_Language(this.getCtx());
		String defaultName = Msg.translate(m_lang, "Standard");
		
		String sql = "SELECT M_WareHouse_ID FROM M_Warehouse wh "+
				 "WHERE wh.AD_Client_ID = ? AND wh.value = ?"; // AND wh.M_ReserveLocator_ID IS NULL
	
		int warehouseID = DB.getSQLValueEx(get_TrxName(), sql, Env.getAD_Client_ID(getCtx()),defaultName);
		MWarehouse wh = new MWarehouse(this.getCtx(),warehouseID, get_TrxName());
		if ( wh.getM_ReserveLocator_ID() == 0 ) {
			wh.setM_ReserveLocator_ID(wh.getDefaultLocator().get_ID());
			wh.saveEx();
		}
		return true;
	}
	
	private boolean initTrlConversionType() {
		Object params[][] = {
				{MConversionType.TYPE_SPOT,"即期汇率","即期汇率类型"},
				{MConversionType.TYPE_PERIOD_END,"期末汇率","期末汇率类型"},
				{MConversionType.TYPE_AVERAGE,"平均汇率","平均汇率类型"},
				{MConversionType.TYPE_COMPANY,"公司汇率","公司汇率类型"}
		};
		for (int i = 0; i < params.length ; i++ ) {
			MConversionType ct = new MConversionType(this.getCtx(),(int)params[i][0], get_TrxName());
			ct.setName((String)params[i][1]);
			ct.setDescription((String)params[i][2]);
			ct.saveEx();
		}
		return true;
	}
	
}
```
</details>
{: .box-info }
