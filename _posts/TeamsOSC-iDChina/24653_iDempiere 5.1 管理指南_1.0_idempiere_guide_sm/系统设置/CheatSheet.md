CheatSheet
===

有的东西太零散了，写这个CheatSheet速查表格方便快速抓住重点。

变更日志
---

列：
IsAllowLogging：设置列是否记录变更日志。Y（默认）：记录；N：不记录。
表：
IsChangeLog：设置表是否记录变更日志。Y（默认）：记录；N：不记录。
角色：
IsChangeLog：设置角色对记录的变更是否记录。Y：记录；N（默认）：不记录

【策略】
1. 列是主要的判断对象，尽量减少列的变更记录以获得更好的服务器性能
2. 即使列变更Y，而对表变更设置N，也不会得到变更。（2者最好能用callout自动关联）
3. 角色一般不用开启变更，除非是测试目的。
4. AD对象的表，总是记录变更。参考：	MChangeLog.fillChangeLog Line60

String sql = "SELECT t.AD_Table_ID FROM AD_Table t "
+ "WHERE t.IsChangeLog='Y'"					//	also inactive
+ " OR EXISTS (SELECT * FROM AD_Column c "
+ "WHERE t.AD_Table_ID=c.AD_Table_ID AND c.ColumnName='EntityType') "
+ "ORDER BY t.AD_Table_ID";

等价于这些AD对象表：
SELECT c.ad_table_id,t.name FROM AD_Column c 
join ad_table t on c.ad_table_id= t.ad_table_id 
WHERE c.ColumnName='EntityType' 
order by 1

【注意】
修改表的变更选项，重新登录（更改会话）无效，需重启服务器后，才可以生效。

【参考】
https://www.compiere-distribution-lab.net/2015/12/14/idempiere-lab-idempiere%E3%82%92%E6%97%A9%E3%81%8F-%E8%BB%BD%E3%81%8F%E3%81%99%E3%82%8B%E3%83%91%E3%83%A9%E3%83%A1%E3%83%BC%E3%82%BF%E8%A8%AD%E5%AE%9A4/

![记录变更判断](https://static.oschina.net/uploads/space/2017/1223/013232_KnUW_2720480.png)

角色权限
---

窗口：
IsActive：有效。Y：可以通过菜单或者链接访问。N：无菜单，无链接可用。
IsReadWrite：是否可读写。Y：可以CRUD。N：只可以读。

角色数据权限：
表权限：
IsExclude ：排除。Y：角色将无法访问该表，报错提醒如下图。
IsReadOnly：只读。Y：角色以只读方式访问该表，所有用到该表的窗口起效。菜单栏删除按钮变灰。

列权限：略
数据权限：略

工具栏按钮权限：
窗口：表头工具栏的权限。Y：直接隐藏，用户无法操作按钮。
明细：明细工具栏的权限。同上。
报表：报表工具栏的权限。同上。

![](https://static.oschina.net/uploads/space/2017/1202/225713_KhIj_2720480.png)

表/字段的权限
---

表：
IsDeleteable记录可删除：表的记录是否允许删除。Y：可以删除。N：不可以删除，那用户只能设置isActive=N

列：
IsAllowCopy允许复制：是否复制该列的数据。Y：复制；N：不复制。
IsUpdateable可更新：该列是否可被用户更新。Y：可更新；N：不可更新，系统自己用上下文填写。
IsAlwaysUpdateable总可更新：即使记录无效或已处理，非只读列总可更新。Y：总可更新；N：有限更新。
IsMandatory必填项：字段是否必填。Y：必填；N：不必填。

页签：
IsReadOnly只读：页签只读。Y：只读；N：可编辑。
IsInsertRecord可新建记录：Y：可新建；N；不可新建，只能修改记录。比如翻译、实体信息（1-1）

字段：【注意字段对列的设置覆盖】
IsReadOnly只读：字段只读。Y：只读；N：可编辑。
【IsAllowCopy】：是否复制该字段的数据。【字段设置覆盖列的设置】
【IsUpdateable】：字段是否可被用户更新。【字段设置覆盖列的设置】
【IsAlwaysUpdateable】：非只读的字段是否永远可更新，即使记录无效或已处理。【字段设置覆盖列的设置】
【IsMandatory】：字段是否必填。【字段设置覆盖列的设置】

> ###常见需求
> 1. 对某个页签需要这3个不同权限
> A：只读
> B：可以修改，不能删除
> C：可以新建，可以修改，可以删除
> 
> 解决方案：
> A：
> 方法1：角色/窗口权限/可读写=N；
> 方法2：角色/角色数据权限/表权限，选择某个表，只读。【效果：只读，不可新建、修改、删除】
> 方法3：单独做窗口，设置单个页签只读状态。表头可编辑，明细只读。
> B：
> 方法1：角色/工具栏按钮权限，去掉”删除“功能。仅限指定窗口/窗口明细/报表。
> 【注】不能使用表的”IsDeleteable记录可删除“选项，否则所有基于该表的窗口都无法删除记录。
> C：
> 方法：角色/窗口权限/有效=Y，可读写=Y；其他排除控制都不给。
> 
> 2. 不同组织对记录的控制
> Org A - User A : 对A组织的某个表记录全部访问
> Org B - User B：对A组织的某个表记录只读访问
> 
> 方法：Role B - 组织权限 - 添加A组织，但选择只读选项。
> 结果：所有A组织的记录对B来说，都是只读。
> 

主从链接
---

表：无设置

列：
IsKeyColumn：主键列。【何时设置】
IsParent：父键列。子表中，链接到父表的列。比如明细表链接到表头的主键列（C_Order_ID）。【何时设置】

页签：
AD_Column_ID：链接列。如果子表没有主键，在子表的TAB中，指定用于链接到父表的列。
Parent_Column_ID：父列。通常明细页签表肯定有一个表头的主键，如果没有，手工选择一个父列。
【注1】每个子表都有一堆ID列，必须指定用哪个ID链接到父表同名列。所以链接列很常见，有330个记录。
【注2】如果要链接的列不同名，还需要子表指定是父表的哪个列。这种情况很少，有6条记录。
【注3】子页签，在设置父列时，是在自己的页签上选择父页签的某个字段。

> ###快速链接父子表
> 表
> 主表： head_id (主键列 IsKey)
> 子表： line_id（主键列 IsKey），head_id（IsParent）
> 
> Column
> isKey，表示单主键表还是多主键表（多主键表无需设置列为 IsKey）。
> IsParent，在子表的列上设置，表示用该键与父表关联。
> 
> TAB：【都在子表TAB上设置】，【都在子表TAB上设置】，【都在子表TAB上设置】
> 主表tab层=0
> 子表tab层=1
>  
> 1. 如果子表是单外键表（isparent数量=1），无设置，系统自动匹配（子表的isParent列找父表的同名列）。 
> 2. 如果子表是多外键表（isparent数量>1），则需要进一步指定子表TAB用哪个link column找父表。
> 3. 如果不是匹配父表的同名列，还需在子表TAB上指定父表的Parent link（比如，link列=sales_id，parent列=ad_user_id）。
> 
> 案例1：订单表头--订单明细 （无需设置）
> 案例2：角色--流程权限 （挂在角色下，子表肯定用ad_role_id做链接列，而不是ad_process_id）。
> 案例3：销售代表--机会（子表用sales_id做链接列，还需指定要挂父表的ad_user_id）。

