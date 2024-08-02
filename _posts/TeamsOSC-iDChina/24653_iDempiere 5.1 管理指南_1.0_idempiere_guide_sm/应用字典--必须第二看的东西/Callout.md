Callout
===

概述
---

官方介绍：http://wiki.adempiere.net/Callout
代码索引：http://wiki.adempiere.net/Callout_Code

Callout Code allow for the execution of code when the user tabs off of the field.  This is a data entry consequence and should not be used for validation (which should occur before a user leaves the field).  An example of a callout is in the Sales Order window.  When the user enters a Business Partner, the callout that is executed updates other fields in the window such as Price List, Delivery Rule and Partner Addess.

![](https://static.oschina.net/uploads/space/2017/1027/174737_SNtm_2720480.jpg)

callout目录结构

```
可以搜索源码文件，查看已有的callout文件。（model文件中也可以包含callout脚本）
..\org.adempiere.base.callout
   |__....
   |__src
       |__org
           |__adempiere
           |   |__base
           |   |   |__callout
           |   |       |__callout*.java（1个）
           |   |__model
           |       |__callout*.java（2个）
           |__compiere
               |__model
                   |__callout*.java (30个)
```

查看使用callout的字段的表

```
select 
e.name as win_name,
d.name as tab_name,
c.tablename as table_name,
b.name as col_name,
a.name as val_name
from  ad_element a
left join ad_column b on a.ad_element_id=b.ad_element_id
left join ad_table c on c.ad_table_id=b.ad_table_id
left join ad_tab d on d.ad_table_id=c.ad_table_id
left join ad_window e on e.ad_window_id=d.ad_window_id
where a.name ~ 'Callout'

结果：3
AD_Attribute表有callout字段  （0个callout记录）
AD_Column表有callout字段 （128个callout记录）
AD_ImpFormat_Row表有callout字段 （0个callout记录）
```

查看AD_Column表段的所有callout记录

```
Select distinct col.Callout  
from AD_Column col, AD_Table tab 
where tab.AD_Table_ID = col.AD_Table_ID 
order by Callout asc

结果：128
```

查看含有callout字段的位置

```
select 
d.name as win_name,
c.name as tab_name,
b.tablename,
a.name as col_name,
f.name as fld_name,
a.callout  
from ad_column a
join ad_table b on b.ad_table_id=a.ad_table_id
join ad_tab c on c.ad_table_id=b.ad_table_id and c.isactive='Y'
join ad_field f on f.ad_column_id=a.ad_column_id and f.ad_tab_id=c.ad_tab_id
join ad_window d on d.ad_window_id=c.ad_window_id and d.isactive='Y'       
/* and d.name ='Purchase Order' */
/* 添加相关条件，可以查看某个窗口的callout记录 * /
where a.callout is not null
order by 1,2

结果：586
```

