TODO
===

TODO
===

0429
---

问题：打开“仓库估值报告”出错，提示主键重复。ERROR: duplicate key value violates unique constraint "ad_workflow_name"
解决：尝试使用单个pg_dump单个数据库为*.dmp，每次恢复都要新建数据库，修改adempiere用户。
以前是pgAdmin III选择“备份服务器”（整个数据库集群），pg_dumpall > outfile.sql，恢复是 psql -f infile -U postgres。

0430
---

1. translation tab导致的用户无法访问tab
2. 明细行自定义打印无法按id筛选------------------5.1 原来是没有设置AD_Pinstance_ID----5.3 好像不是这个问题
3. adempiere enterprise application framework的文章找不到了--------5.3 找到http://wiki.adempiere.net/ADempiere_Rapid_Development
4. 快速翻译提交

0501
---

1. DataEngine.java
2.[22:41] <AlbertChen> Read Doc_Invoice.java
[22:42] <AlbertChen> the ARI and ARC use same Account_Element

0503
---

JBOSS相关配置，连接数
jetty的相关配置，主机，端口，timeout

0507
---

http://wiki.adempiere.net/Usage_Tips_And_Tricks
1. BP窗口可以选择语言，以确定单据日期格式。
2. Search框在window High Volumn设置，或者Role Confirm Query Records设置。

0522
---

 | 表头必填项 | 明细行 | 备注 | 
---|---|---|---|
采购合同 | 客户、交易地点、价格表 | 1、必须有明细行
2、可以无产品，无费用 | 相当于“合同类采购订单” | 
销售合同 | 客户、交易地点、价格表 | 1、必须有明细行
2、可以无产品，无费用 |  | 
采购发票 | 客户、交易地点、价格表 | 1、必须有明细行
2、可以无产品，无费用
3、采购合同可选 | 借：产品费用
贷：应付账款 | 
销售发票 | 客户、交易地点 | 1、必须有明细行
2、可以无产品，无费用
3、销售合同可选 | 借：主营收入
贷：应收账款 | 
付款单 | 客户、交易地点 | 1、可以无分配明细行
2、直接输入收款（相当于采购定金） | 借：付款选择
贷：资金在途 | 
收款单 | 客户、交易地点 | 1、可以无分配明细行
2、直接输入收款（相当于销售定金） | 借：资金在途
贷：未分配发票收款 | 

0416
---
https://groups.google.com/d/topic/idempiere/CRCEa6FytS0/discussion 小数点问题
https://groups.google.com/d/topic/idempiere/Am91pXnoR_A/discussion 资产管理插件web客户端问题
http://adempiere.net/web/guest/forums#!/thread/49075 拆解业务
http://wiki.idempiere.org/en/Plugin:_Couple_Product 卖线缆or窗帘业务
https://groups.google.com/d/topic/idempiere/g-HenapR7KU/discussion 过账扩展
https://groups.google.com/d/topic/idempiere/HdV0GG_5O1M/discussion 有库存帐后导入库存没有成本记录 no cost问题
https://groups.google.com/d/topic/idempiere/DLkXRBGxsyg/discussion Change_costing_method
https://groups.google.com/d/topic/idempiere/9fl2xkH9b0E/discussion 多组织公用仓库
https://groups.google.com/d/msg/idempiere/URZpViqIKG8/mVSaOYLYVe8J 存货计价类型javaclass
http://wiki.adempiere.net/index.php/Cost_Engine/Testing#Use_Cases 成本数据测试case
https://groups.google.com/d/topic/idempiere/myYSTZZAVg0/discussion 移动成本导致利润负值
https://groups.google.com/d/topic/idempiere/3Np1C63CV_8/discussion 初次导入库存，做采购单
https://groups.google.com/d/topic/idempiere/tNj6FikhFRQ/discussion 计价方式的推荐：标准成本，移动采购价格
https://groups.google.com/d/msg/idempiere/EpXFDzc5cys/AfdbGPp1AgAJ 按进度开票问题
https://groups.google.com/d/topic/idempiere/nxtw-l4aq-A/discussion Average and Std costing
https://groups.google.com/d/topic/idempiere/zmZD5e8dsds/discussion BOMDrop

0914
---
http://open.kingdee.com/K3cloud/Democenter/ProductInfo.aspx?id=112322

Toread list
https://docs.oracle.com/cd/B53825_08/current/html/docset.html
http://docs.oracle.com/cd/E26401_01/index.htm

http://www.erp-in.com/blog-2-18.htmlERP实施 各有各的苦
https://www.baidu.com/link?url=xH6kIy0lCmCoF2GDVpWb6NQLu2CH_i6B-AfrIRpjV-sb5Jwb_OkYOOB4NKKc1iTjyXuSWAER4KNki6aU0kEcHCmSJmXqHRaztu1zIWQCBuu&wd=&eqid=bf519a9c000373200000000556f2dba2ORACLE EBS 系统架构与应用实践


http://sap.readthedocs.org/en/latest/index.html SAP 百日

https://www.douban.com/note/432060336/ SAP 上线体会

https://sourceforge.net/projects/idempiereksys/files/ 国内大神的K Long项目
https://github.com/search?q=user%3Alongnan+IDEMPIERE+
https://groups.google.com/forum/#!searchin/idempiere/docker/idempiere/nY2ebmPxiEA/FisMTeh6yykJ

Hi Everyone,
The purpose of this post is to provide guidance on how to create a small barcode license plate to support serial and lot tracking in iDempiere and ADempiere Open Source ERP.
Here are the steps:
  1. Log in as System Administrator
  2. Create a new Report View record called “MRLine License Plate”. Set the table to “M_InOutLine”.
  3. Create a Report and Process called “Print MRLine License Plate”. Set the Report View to “MRLine License Plate”.
  4. Open the Window, Tab and Field for “Material Receipt”. Navigate to the “Receipt Line” tab. Add your new “Print MRLine License Plate” process to the tab’s Process field.
  5. Create a new Print Paper record called “License Plate”. Set the Print Paper “License Plate” validation code to “NA”. Set the Dimension Units to your desired value. Set the Size X and Size Y values to the size of your paper. In my case, I am printing to a 3″ by 2″ zebra or printronix label printer
  6. Log in as your admin user (“SuperUser”) and admin role (“GardenWorld Admin”).
  7. Find a Material Receipt with a line. Click the Receipt Line’s print button. This will create a new Print Format record for you.
  8. Click the newly created Print Format’s Customize button (wrench and screw driver icon). Click the Print Format’s Form checkbox to show a “Checked”, “Yes” or “True” value. Set the Print Paper field to “License Plate”.
  9. Reset the cache using the Cache Reset process.
  10. Go back to your MR Line and print. You should now have a very ugly label.
  11. Play, Play, Play – to make it pretty. Choose what lines you want to show as barcodes using the Format Item’s Barcode Type field.

http://wiki.idempiere.org/en/Create_a_custom_document_class
http://wiki.idempiere.org/en/Developing_Plug-Ins_-_IDocFactory_(Custom_Accounting)
http://wiki.idempiere.org/en/Developing_Plug-Ins_-_Models,_Documents_and_custom_accounting
http://wiki.adempiere.net/How_to_create_a_new_document_with_specific_accounting

weiterführende Erklärungen
http://wiki.adempiere.net/How_to_Activate_Document_Approval_Workflow
http://wiki.adempiere.net/Document_Action_Dialog 
http://wiki.adempiere.net/Document_Sequence - Document Sequence

Erweiterungen
http://wiki.adempiere.net/Enhance_Document_No_Formatting
http://wiki.idempiere.org/en/NF001_Document_Sequence_Improved
http://wiki.adempiere.net/Sponsored_Development:_Document_Signing

Artikel, die ich als Quellen verwendet, aber bereits weitgehend hier eingearbeitet habe:
http://wiki.adempiere.net/HOWTO_Process_Documents
http://en.wikiversity.org/wiki/Adempiere_Technical_Training#Document_Process_Workflow
http://wiki.adempiere.net/Document_Engine
https://groups.google.com/forum/#!topic/idempiere/WtTlVL1ZjWw

Fenster, die mit Dokumenten zusammenhängen:
http://wiki.idempiere.org/en/Document_Type_%28Window_ID-135%29
http://wiki.idempiere.org/en/Document_Sequence_%28Window_ID-112%29
http://wiki.idempiere.org/en/Unprocessed_Documents_%28All%29_%28Window_ID-53087%29
http://wiki.idempiere.org/en/UnPosted_Documents_%28Window_ID-294%29
http://wiki.idempiere.org/en/My_Unprocessed_Documents_%28Window_ID-53086%29
http://wiki.idempiere.org/de/Workflow_%28Fenster_ID-113%29
http://wiki.idempiere.org/en/Verify_Document_Types_%28Process_ID-233%29

收款单核销手工挂的应收，手工的入账不能核销发票
付款单界面点击BP，无法带出BP的帐号信息




