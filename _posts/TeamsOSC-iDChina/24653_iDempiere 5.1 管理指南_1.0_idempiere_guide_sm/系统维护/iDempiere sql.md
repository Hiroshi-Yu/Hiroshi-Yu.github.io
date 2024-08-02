iDempiere sql
===

概述
---

为了了解ID，很多情况下看系统默认设置是如何做的，故收集如下sql。
一些必须记住的语法和常量：
ad_client_id=0 ; System实体
ad_client_id=11; GardenWorld实体
ad_org_id=0; \* 组织（任何组织）
ad_user_id=0; System用户
@#字段名@：context环境变量
@字段名@：表字段变量
所有维度的默认记录ID都是0。

--Tab-查看sqlwhere

```
select d.name as window , a.name tab, a.WhereClause , b.tablename, c.name as accesslevel
from ad_tab a  
inner join ad_table b on a.ad_table_id=b.ad_table_id
inner join AD_Ref_List c on b.accesslevel=c.value
inner join ad_window d on a.ad_window_id=d.ad_window_id
where c.AD_Reference_ID=5 and WhereClause is not null order by 4;

\* 查询数量：87个 *\
\* 典型应用：ALL 13个, client 2个, client+org 26个，org 28个，system 5个，sys+client 3个* \
\* type类：44个，SOTrx类：11个，User类：12个，
\* 实体级表的自己记录，CreatedBy=@#AD_User_ID@ *\
\* 所有级别表的自己记录，AD_User_ID=@#AD_User_ID@ AND AD_Client_ID=@#AD_Client_ID@ *\
\* 销售事务单据，C_Invoice.IsSOTrx='Y' *\
\* 交叉索引，Used in Column AD_Column.AD_Reference_Value_ID=@AD_Reference_ID@ *\
\* 现金交易无BP，C_Payment.C_BPartner_ID IS NOT NULL * \



```

--Tab-查看tab字段与table字段的差异

```
select 
b.name as table_name,
c.name as tab_name,
a.name as col_name , a.istoolbarbutton,   /* 检查按钮字段的位置 */
d.name as fld_name, isdisplayed
from ad_column a 
join ad_table b on a.ad_table_id=b.ad_table_id
join ad_tab c on b.ad_table_id=c.ad_table_id
left join ad_field d on d.ad_tab_id=c.ad_tab_id and a.ad_column_id=d.ad_column_id
where c.ad_tab_id=220         /* 替换你所需要检查的tab */
order by isdisplayed desc   
```

--Table-无法删除记录的表

```
select name , TABLENAME, accesslevel  from ad_table 
where IsActive='Y' and isview='N' 
and tablename !~'(Trl)|(Acct)$'  
and IsDeleteable = 'N' 
ORDER BY 3 

\* 查询数量：91个 （非视图、非翻译、非账户）
\* 典型应用 :  自动生成记录的表格* \
\* 组织访问级：3个，存货事务、分配表、分配明细 *\
\* 实体访问级：实体、组织、日历、期间、视图、翻译表、账户设置表 *\
\* 系统级：2个 System Registration，Modification *\

```

--Trl-增加丢失的翻译

```
select add_missing_translations();

\* 如果选择非系统语言（中文）登陆，表直接会取翻译的记录，如果存在丢失的翻译，则记录不全 *\
\* 你需要运行这个代码，增加丢失的翻译记录 *\
```

--Process-查看"成本"相关流程的位置

```
select 
a.name as prc_name, a.description as prc_desc,
h.name as menu_name,
b.name as col_name,
c.tablename, c.entitytype,
d.name as tab_name,
e.name as fld_name, e.isdisplayed, e.displaylogic,
f.name as tb_name,
g.name as win_name
from ad_process a
 left  join ad_column b on a.ad_process_id=b.ad_process_id
 left  join ad_table c on b.ad_table_id=c.ad_table_id 
 left  join ad_tab d on c.ad_table_id=d.ad_table_id 
 left  join ad_field e on d.ad_tab_id = e.ad_tab_id and b.ad_column_id=e.ad_column_id 
 left join ad_toolbarbutton f on d.ad_tab_id=f.ad_tab_id                   /* 检查tab按钮是否挂流程 */
 left  join ad_window g on d.ad_window_id=g.ad_window_id 
 left join ad_menu h on a.ad_process_id=h.ad_process_id 
where a.value ~'Cost' and a.IsReport='N' 
order by h.name,e.isdisplayed desc
```

