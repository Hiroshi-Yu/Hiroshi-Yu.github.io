数据库函数说明 (Postgresql版)
===

No. | 函数定义 | 分类 | 说明 | 
---|---|---|---|
1 | add_missing_translations() | 工具 | 当某系统语言的某些翻译行没有生成时，通过执行该函数，可以自动生成翻译表中不存在的缺省翻译行信息。

```
-- 例：对AD_Ref_List的翻译信息更新SQL（执行函数添加LOG所得）
INSERT INTO AD_Ref_List_TRL (ad_language,ad_client_id,ad_org_id,created,createdby,updated,updatedby,isactive,istranslated,AD_Ref_List_id,Description,Name) SELECT l.ad_language,t.ad_client_id,t.ad_org_id,t.created,t.createdby,t.updated,t.updatedby,t.isactive,'N' as istranslated,AD_Ref_List_id,t.Description,t.Name from AD_Ref_List t, ad_language l WHERE l.issystemlanguage='Y' AND NOT EXISTS (SELECT 1 FROM AD_Ref_List_TRL b WHERE b.AD_Ref_List_id=t.AD_Ref_List_id AND b.AD_LANGUAGE=l.AD_LANGUAGE)
``` | 
2 | add_months(timestamp with time zone, numeric) | 程序 | 在指定的日期基础上加指定的月数
> **参数：**
datetime timestamp with time zone=指定日期
months numeric=增加的月数
**返回值：**
date 求得的新日期

```
-- 例：得到当前日期三个月后的日期
select add_months(now(),3);
``` | 
3 | adddays(timestamp with time zone, numeric) | 程序 | 在指定的日期基础上加指定的天数
用法同add_months，增量单位为天
唯一要注意的是add和days中间没有下划*线......*
```
-- 例：得到当前日期三天后的日期
select adddays(now(),3);
-- 提醒：别告诉我第二个参数你不准备放负数
``` | 
4 | adddays(interval, numeric) | 程序 | 在指定的日期基础上加指定的天数
> **参数：**
inter interval=骚包的类型
days numeric=增加的天数
**返回值：**
integer 总天数

```
-- 例：别问为什么等于1552
select adddays(interval '4 year 2 month 28 days',3);

--** 内部实现方法：**
RETURN ( EXTRACT( EPOCH FROM ( inter ) ) / 86400 ) + days;
--看模样是转换成了秒之后，除一天的秒数(86400)得来的
``` | 
5 | altercolumn(name, name, name, character varying, character varying) | 工具 | iDempiere内部工具
在应用字典中调整列的配置时，往往只向t_alter_column表中插入一行即可。其实在该表中配置了规则，重定向到了altercolumn | 
6 | bompricelimit(numeric, numeric) | 程序 | 取得产品的限制价格(PriceLimit)，如果BOM上已经定义限制价格，优先取限制价格，如果没有的话，合计所有构成品的限制价格
> **参数：**
product_id numeric=产品ID
pricelist_version_id=价格表版本ID
**返回值：**
numeric 限制价格

```
-- 例：*暂无*

``` | 
7 | bompricelist(numeric, numeric) | 程序 | 取得产品的价格表列表价格(PriceList)，如果BOM上已经定义价格，优先取BOM价格，如果没有的话，合计所有构成品的价格
> **参数：**
product_id numeric=产品ID
pricelist_version_id=价格表版本ID
**返回值：**
numeric 列表价格

```
-- 例：*暂无*

``` | 
8 | bompricestd(numeric, numeric) | 程序 | 取得产品的价格表标准价格(PriceStd)，如果BOM上已经定义价格，优先取BOM价格，如果没有的话，合计所有构成品的价格
> **参数：**
product_id numeric=产品ID
pricelist_version_id=价格表版本ID
**返回值：**
numeric 标准价格

```
-- 例：*暂无*

``` | 
9 | bomqtyavailable(numeric, numeric, numeric) | 程序 | 取得指定BOM产品在指定仓库、库位的可供货数量
计算逻辑:
bomQtyOnHandForReservation - bomQtyReserved
可供订货库位在手数量-保留库存数量 | 
10 | bomqtyonhand(numeric, numeric, numeric) | 程序 | 取得指定BOM产品在指定仓库、库位的在手数量(M_Storageonhand.QtyOnHand)
> **参数：**
product_id numeric=产品ID
warehouse_id numeric=仓库ID
locator_id numeric=库位ID
**返回值：**
numeric 在手数量。如果为BOM品并且为库存品的话，直接返回在手数量。如果为BOM品但并非是库存品的话，返回相应仓库构成品可以组装出来的最大BOM品数量
**注意事项：**
如果产品ID，仓库ID，库位ID指定错误返回0
如果产品非BOM的同时，并且为非物料商品或者非库存品的话，返回99999

```
-- 例：*暂无*
``` | 
11 | bomqtyonhandforreservation(numeric, numeric, numeric) | 程序 | 取得指定BOM产品在指定仓库、可供预订库位的在手数量(M_Storageonhand.QtyOnHand) | 
12 | bomqtyordered(numeric, numeric, numeric) | 程序 | 取得指定BOM产品在指定仓库、库位的下单库存数量(M_StorageReservation.Qty IsSOTrx='N') | 
13 | bomqtyreserved(numeric, numeric, numeric) | 程序 | 取得指定BOM产品在指定仓库、库位的保留库存数量(M_StorageReservation.Qty IsSOTrx='Y') | 
14 | bpartnerremitlocation(numeric) | 程序 | 取得业务伙伴的收款地址ID

> **参数：**
p_c_bpartner_id numeric=业务伙伴ID
**返回值：**
numeric 业务伙伴地址ID（C_BPartner_Location_ID）

```
-- 例：*暂无*

``` | 
15 | charat(character varying, integer) | 程序 | 取得指定字符串某一位置的一个字符。

> **参数：**
character varying=指定字符串
integer=位置
**返回值：**
character varying 一个字符

```
-- 例：得到指定字符串的第15个字符
select charat('iDempiere中文社区文档协作',15);
-- *不明白为什么要有这个函数，内部实现为SUBSTR($1, $2, 1);*

``` | 
16 | currencybase(numeric, numeric, timestamp with time zone, numeric, numeric) | 程序 | 取得指定币种金额对应的本位币金额

> **参数：**
p_amount numeric=金额
p_curfrom_id=币种
p_convdate=兑换日期
p_client_id
p_org_id
**返回值：**
numeric 本位币金额

```
-- 例：*暂无*

``` | 
17 | currencyconvert(numeric, numeric, numeric, timestamp with time zone, numeric, numeric, numeric) | 程序 | 取得指定币种金额对应币种的换算后金额

> **参数：**
p_amount numeric=金额
p_curfrom_id=币种
p_curto_id=对应币种
p_convdate=兑换日期
p_conversiontype_id=变换类型(只在查询信息时使用，不参与计算)
p_client_id
p_org_id
**返回值：**
numeric 对应币种的换算后金额

```
-- 例：*暂无*

``` | 
18 | currencyrate(numeric, numeric, timestamp with time zone, numeric, numeric, numeric) | 程序 | 取得指定币种和对应币种的换算汇率

> **参数：**
p_curfrom_id=币种
p_curto_id=对应币种
p_convdate=兑换日期
p_conversiontype_id=变换类型
p_client_id
p_org_id
**返回值：**
numeric 对应币种的换算汇率

```
-- 例：*暂无*

``` | 
19 | currencyround(numeric, numeric, character varying) | 程序 | 指定币种金额的四舍五入

> **参数：**
p_amount numeric=金额
p_curto_id numeric=对应币种
p_costing character varying=是否为成本金额 'Y/N'
**返回值：**
numeric 四舍五入后金额

```
-- 例：*暂无*

``` | 
20 | daysbetween(timestamp with time zone, timestamp with time zone) | 程序 | 返回两个日期的差异天数

```
-- 例：
select daysbetween('2017-2-1','2017-1-1');```
 | 
21 | ~~documentno(numeric)~~ | ~~程序~~ | iDempiere内部使用
** 推测是生产模块使用的函数 ** | 
22 | firstof(timestamp with time zone, character varying) | 程序 | date_trunc的扩展函数，针对datepart 参数支持如下内容的变换。
```
  IF $2 IN ('') THEN
    datepart = 'millennium';
  ELSEIF $2 IN ('') THEN
    datepart = 'century';
  ELSEIF $2 IN ('') THEN
    datepart = 'decade';
  ELSEIF $2 IN ('IYYY','IY','I') THEN
    datepart = 'year';
  ELSEIF $2 IN ('SYYYY','YYYY','YEAR','SYEAR','YYY','YY','Y') THEN
    datepart = 'year';
  ELSEIF $2 IN ('Q') THEN
    datepart = 'quarter';
  ELSEIF $2 IN ('MONTH','MON','MM','RM') THEN
    datepart = 'month';
  ELSEIF $2 IN ('IW') THEN
    datepart = 'week';
  ELSEIF $2 IN ('W') THEN
    datepart = 'week';
  ELSEIF $2 IN ('DDD','DD','J') THEN
    datepart = 'day';
  ELSEIF $2 IN ('DAY','DY','D') THEN
    datepart = 'week';
    -- move to sunday to make it compatible with oracle and SQLJ
    offsetdays = -1;
  ELSEIF $2 IN ('HH','HH12','HH24') THEN
    datepart = 'hour';
  ELSEIF $2 IN ('MI') THEN
    datepart = 'minute';
  ELSEIF $2 IN ('') THEN
    datepart = 'second';
  ELSEIF $2 IN ('') THEN
    datepart = 'milliseconds';
  ELSEIF $2 IN ('') THEN
    datepart = 'microseconds';
  END IF;
``` | 
23 | generate_uuid() | 程序 | 得到UUID
目前使用的是uuid_generate_v4 | 
24 | get1099bucket(numeric, timestamp with time zone, numeric) | 程序 | 和美国的1099box报税相关
根据指定日期查询当年截止到指定日期的所有符合1099报税规则对应的发票总金额
注：DOCTYPE为APC的会被反冲（Vendor Credit Memo） | 
25 | get_sysconfig(character varying, character varying, numeric, numeric) | 程序 | 取得系统配置信息

> **参数：**
sysconfig_name character varying=配置名称
defaultvalue character varying=配置缺省值
client_id
org_id
**返回值：**
配置信息

```
-- 例：
select get_sysconfig('PROJECT_ID_USE_CENTRALIZED_ID','Y',0,0);
``` | 
26 | invoicediscount(numeric, timestamp with time zone, numeric) | 程序 | 取得系统发票对应的折扣金额

> **参数：**
p_c_invoice_id numeric=系统发票ID
p_paydate timestamp with time zone=付款日期
p_c_invoicepayschedule_id numeric=发票付款计划ID
**返回值：**
numeric=折扣金额
**注意事项：**
如果有付款计划的按照付款计划的折扣计算，如果没有付款计划的按照付款条件来计算

```
-- 例：*暂无*

```
 | 
27 | invoiceopen(numeric, numeric) | 程序 | 计算系统发票的尚未付款金额

> **参数：**
p_c_invoice_id numeric=系统发票ID
p_c_invoicepayschedule_id numeric=发票付款计划ID，为空的话，代表所有的付款计划
**返回值：**
numeric=Open Item Amount

```
-- 例：*暂无*
SELECT C_InvoicePaySchedule_ID, DueAmt FROM C_InvoicePaySchedule WHERE C_Invoice_ID=109 ORDER BY DueDate;
SELECT invoiceOpen (109, null) FROM AD_System; - converted to default client currency
SELECT invoiceOpen (109, 11) FROM AD_System; - converted to default client currency
SELECT invoiceOpen (109, 102) FROM AD_System;
SELECT invoiceOpen (109, 103) FROM AD_System;
```
 | 
28 | invoiceopentodate(numeric, numeric, timestamp with time zone) | 程序 | 同invoiceopen。
可以在第三个参数指定记账日期，所有记账日期小于等于该日期的发票都是统计对象 | 
29 | invoiceopentodate(numeric, numeric, date) | 程序 | 同invoiceopen。
可以在第三个参数指定记账日期，所有记账日期小于等于该日期的发票都是统计对象 | 
30 | invoicepaid(numeric, numeric, numeric) | 程序 |  * Title:  Calculate Paid/Allocated amount in Currency
 * Description:
 *  Add up total amount paid for for C_Invoice_ID.
 *  Split Payments are ignored.
 *  all allocation amounts  converted to invoice C_Currency_ID
 *  round it to the nearest cent
 *  and adjust for CreditMemos by using C_Invoice_v
 *  and for Payments with the multiplierAP (-1, 1)
 *
 *
 * Test:
    SELECT C_Invoice_ID, IsPaid, IsSOTrx, GrandTotal, 
    invoicePaid (C_Invoice_ID, C_Currency_ID, MultiplierAP)
    FROM C_Invoice_v; | 
31 | invoicepaidtodate(numeric, numeric, numeric, timestamp with time zone) | 程序 | 同invoicepaid
可以在第四个参数指定C_ALLOCATIONHDR的记账日期 | 
32 | ~~无~~ | ~~无~~ | ~~无~~ | 
33 | ismemberofacctschema(numeric, numeric, numeric) | 程序 | ~~确认指定的组织是否有对应会计模式~~
> **参数：**
p_ad_client_id numeric
p_ad_org_id numeric
p_c_acctschema_id numeric
**返回值：**
boolean

```
-- 例：*暂无*

```
 | 
34 | migr_fix_payment_cashline() | 程序 | 仅为推测：为现金日记帐明细更新收付款ID | 
35 | nextbusinessday(timestamp with time zone, numeric) | 程序 | 取得下一个工作日
> **参数：**
p_date timestamp with time zone=指定日期
p_ad_client_id numeric
**返回值：**
timestamp with time zone 下一个工作日

```
-- 例：*暂无*

```
 | 
36 | nextid(integer, character varying) | 程序 | 取得指定队列的下一个ID的函数
> **参数：**
IN p_ad_sequence_id integer=队列ID
IN p_system character varying=传'N'
OUT o_nextid integer=返回值，下一个ID值
```
-- 例：*暂无*
```
 | 
37 | nextidfunc(integer, character varying) | 程序 | 取得指定队列的下一个ID的函数
> **参数：**
IN p_ad_sequence_id integer=队列ID
IN p_system character varying=传'N'
**返回值：**
integer 下一个ID值

```
-- 例：*暂无*

```
 | 
38 | paymentallocated(numeric, numeric) | 程序 | - Title:  Calculate Allocated Payment Amount in Payment Currency

- Description: 

       SELECT paymentAllocated(C_Payment_ID,C_Currency_ID), PayAmt, IsAllocated
       FROM C_Payment_v
       WHERE C_Payment_ID<1000000;


    UPDATE C_Payment_v 
    SET IsAllocated=CASE WHEN paymentAllocated(C_Payment_ID, C_Currency_ID)=PayAmt THEN 'Y' ELSE 'N' END
    WHERE C_Payment_ID>=1000000; | 
39 | paymentavailable(numeric) | 程序 |  * Title:  Calculate Available Payment Amount in Payment Currency
 * Description:
 *    similar to C_Invoice_Open | 
40 | paymenttermdiscount(numeric, numeric, numeric, timestamp with time zone, timestamp with time zone) | 程序 | 根据指定的付款条件ID取得对应的折扣金额

> **参数：**
amount numeric=金额
currency_id numeric=币种ID
paymentterm_id numeric=付款条件ID
docdate timestamp with time zone=过账日期
paydate timestamp with time zone=付款日期
**返回值：**
numeric=折扣金额
**注意事项：**
折扣1和折扣2不可同时有效

```
-- 例：*暂无*
``` | 
41 | paymenttermduedate(numeric, timestamp with time zone) | 程序 | 根据指定的付款条件ID取得DueDate
Get Due timestamp with time zone

> **参数：**
paymentterm_id numeric=付款条件ID
docdate timestamp with time zone=过账日期
**返回值：**
RETURNS timestamp with time zone=  
**注意事项：**
折扣1和折扣2不可同时有效

```
-- 例：
select paymenttermDueDate(106, now());
``` | 
42 | paymenttermduedays(numeric, timestamp with time zone, timestamp with time zone) | 程序 |  * Title:  Get Due Days
 * Description:
 *  Returns the days due (positive) or the days till due (negative)
 *  Grace days are not considered!
 *  If record is not found it assumes due immediately
 *
 *  Test:  SELECT paymenttermDueDays(103, now(), now());
 *
 * Contributor(s): Carlos Ruiz - globalqss - match with SQLJ version | 
43 | prodqtyordered(numeric, numeric) | 程序 | 根据指定的产品ID和仓库ID，返回生产工单（明细）的待入库成品总数

> **参数：**
p_product_id numeric=产品ID
p_warehouse_id numeric=仓库ID

**返回值：**
numeric=待入库成品总数量
**注意事项：**
仓库ID参数有核对；
产品必须可存货；
生产工单明细未处理；
服务无数量=0；
返回数量会取整；
```
-- 例：**省略**
``` | 
44 | prodqtyreserved(numeric, numeric) | 程序 | 根据指定的产品ID和仓库ID，返回生产工单（明细）的预留待分解成品总数

> **参数：**
p_product_id numeric=产品ID
p_warehouse_id numeric=仓库ID

**返回值：**
numeric=预留待分解产品总数
**注意事项：**
仓库ID参数有核对；
产品必须可存货；
生产工单明细未处理；
服务无数量=0；
返回数量会取整；
```
-- 例：**省略**
``` | 
45 | productattribute(numeric) | 程序 | 根据指定的产品属性实例ID，返回产品实例名称(属性集-序列号-批号-有效日期)

> **参数：**
M_AttributeSetInstance_ID numeric=产品属性实例ID

**返回值：**
VARCHAR=实例名称(属性集-序列号-批号-有效日期)
**注意事项：**
M_AttributeSetInstance_ID=0是默认属性集，其名称为空；
```
-- 例：**省略**
``` | 
46 | register_migration_script(character varying) | 工具 | 把更新到系统的SQLPatch文件名更新到ad_migrationscript表中进行履历管理。
同时把最后更新的Patch名字更新到AD_System.LastMigrationScriptApplied | 
47 | role_access_update() | 工具 | 根据最新的角色访问配置更新角色访问信息 | 
48 | subtractdays(interval, numeric) | 程序 | 同adddays(interval, numeric)，只是第二个参数为减

--** 内部实现方法：**
RETURN ( EXTRACT( EPOCH FROM ( inter ) ) / 86400 ) - days;
``` | 
49 | subtractdays(timestamp with time zone, numeric) | 程序 | 内部调用addDays(day,(days * -1)) | 
50 | update_sequences() | 工具 | 更新所有的sequences id | 
51 | uuid_generate_v1() | 程序 | 同'$libdir/uuid-ossp', 'uuid_generate_v1' | 
52 | uuid_generate_v1mc() | 程序 | 同'$libdir/uuid-ossp', 'uuid_generate_v1mc' | 
53 | uuid_generate_v3(uuid, text) | 程序 | 同'$libdir/uuid-ossp', 'uuid_generate_v3' | 
54 | uuid_generate_v4() | 程序 | 同'$libdir/uuid-ossp', 'uuid_generate_v4' | 
55 | uuid_generate_v5(uuid, text) | 程序 | 同'$libdir/uuid-ossp', 'uuid_generate_v5' | 
56 | uuid_nil() | 程序 | 同'$libdir/uuid-ossp', 'uuid_nil' | 
57 | uuid_ns_dns() | 程序 | 同'$libdir/uuid-ossp', 'uuid_ns_dns' | 
58 | uuid_ns_oid() | 程序 | 同'$libdir/uuid-ossp', 'uuid_ns_oid' | 
59 | uuid_ns_url() | 程序 | 同'$libdir/uuid-ossp', 'uuid_ns_url' | 
60 | uuid_ns_x500() | 程序 | 同'$libdir/uuid-ossp', 'uuid_ns_x500' | 
61 | acctbalance(numeric, numeric, numeric) | 程序 | 根据指定的科目ID返回借贷余额

> **参数：**
p_account_id numeric=科目ID
p_amtdr numeric=借方金额
p_amtcr numeric=贷方金额

**返回值：**
numeric=借贷余额
**注意事项：**
借贷余额的正负取决于科目类型和方向标记

```
-- 例：select acctbalance(1000012, 1000, 200)
``` | 

Oracle函数的实现

No. | 函数定义 | 参考信息 | 
---|---|---|
1 | getdate() | pg函数：now()

例：select getdate(); | 
2 | instr(character varying, character varying, integer, integer)
instr(character varying, character varying, integer)
instr(character varying, character varying)
 | pg函数：substring()，position()等
例：select instr('abcdeabcdeb','b');
select instr('abcdeabcdeb','b',5);
select instr('abcdeabcdeb','b',5,2);
 | 
3 | round(numeric, numeric) | 在Postgresql中的Round的第二个参数类型为integer，这里添加Oracle用的decimals（即numeric）类型 | 
4 | trunc(timestamp with time zone, character varying)
trunc(interval)
trunc(timestamp with time zone) | pg函数：DATE_Trunc() | 

