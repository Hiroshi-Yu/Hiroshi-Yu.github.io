Postgresql sql
===

概述
---

iDempiere提供SQL DDL Statements界面，避免直接操作数据库。
还可以通过2pack导入sql语句，注意oracle要大写关键字。
参考：
http://lovejuan1314.iteye.com/blog/1167671
http://www.alberton.info/postgresql_meta_info.html#.WCqTfWp97Dc

查看版本
---

SELECT VERSION()

监听sql语句
---

select query_start,query from pg_stat_activity where usename='adempiere';

查询表名和描述
---

1. 找到用户数据库的schema的OID（名称空间），比如OID=192305
2. 复制如下sql，更换relnamespace=上述oid

select relname as table_name,(select description from pg_description where objoid=oid and objsubid=0) as comment 
from pg_class 
where relkind ='r' and relnamespace=192305
order by table_name;

查询表大小（函数表）
---

SELECT  
    table_schema || '.' || table_name AS table_full_name,  
    pg_size_pretty(pg_total_relation_size('"' || table_schema || '"."' || table_name || '"')) AS size  
FROM information_schema.tables  where table_name ~'pg_proc'
ORDER BY  
    pg_total_relation_size('"' || table_schema || '"."' || table_name || '"') DESC 

查询表大小
---

SELECT
schemaname,
tablename,
pg_size_pretty(pg_relation_size(schemaname || '.' || tablename)) AS size_p,
pg_total_relation_size(schemaname || '.' || tablename) AS siz,
pg_size_pretty(pg_total_relation_size(schemaname || '.' || tablename)) AS total_size_p,
pg_total_relation_size(schemaname || '.' || tablename) - pg_relation_size(schemaname || '.' || tablename) AS index_size,
(100*(pg_total_relation_size(schemaname || '.' || tablename) - pg_relation_size(schemaname || '.' || tablename)))/CASE WHEN pg_total_relation_size(schemaname || '.' || tablename) = 0 THEN 1 ELSE pg_total_relation_size(schemaname || '.' || tablename) END || '%' AS index_pct
FROM pg_tables where schemaname='adempiere'
ORDER BY siz DESC LIMIT 20;

查询表记录个数
---

select 
relname as TABLE_NAME, 
reltuples as rowCounts 
from pg_class where relkind = 'r' and relnamespace = 580092   /* 必须找到你的schema的oid  */
order by rowCounts desc; 

![schema的oid](https://static.oschina.net/uploads/space/2017/0823/152457_L7Gu_2720480.png)

查询所有的表字段信息(带表名)
---

1. 找到用户数据库的schema的OID（名称空间），比如OID=192305
2. 复制如下sql，更换relnamespace=上述oid

select
(select relname as comment from pg_class where oid=a.attrelid) as table_name,
a.attname as column_name,
format_type(a.atttypid,a.atttypmod) as data_type,
(case when atttypmod-4>0 then atttypmod-4 else 0 end)data_length,
(case when (select count(*) from pg_constraint where conrelid = a.attrelid and conkey[1]=attnum and contype='p')>0 then 'Y' else 'N' end) as 主键约束,
(case when (select count(*) from pg_constraint where conrelid = a.attrelid and conkey[1]=attnum and contype='u')>0 then 'Y' else 'N' end) as 唯一约束,
(case when (select count(*) from pg_constraint where conrelid = a.attrelid and conkey[1]=attnum and contype='f')>0 then 'Y' else 'N' end) as 外键约束,
(case when a.attnotnull=true then 'Y' else 'N' end) as nullable,
col_description(a.attrelid,a.attnum) as comment
from pg_attribute a
where attstattarget=-1 and attrelid in (select oid from pg_class where relname in(select relname from pg_class where relkind ='r' and relnamespace=192305))
order by table_name,a.attnum;

查找重复记录

```
/* id（不重复），name（重复），找name */
select name,count(*) from ad_table_trl group by name having count(*)&gt;1;

/* id（不重复），name（重复），找id */
select ad_table_id, name from ad_table_trl where name in 
(select name from ad_table_trl group by name having count(name)&gt;1);

```

