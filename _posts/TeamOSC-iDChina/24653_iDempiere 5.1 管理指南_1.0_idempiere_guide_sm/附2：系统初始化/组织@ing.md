组织@ing
===

组织这个东西，单拿出来笔记，内容太复杂了。

公司级应用
---

一个公司。
N个部门，处理采购、销售、库存
2个门店，处理销售、库存

总账维度：
组织：
科目：
业务伙伴：
事务组织：如果使用事务组织=部门，则组织记录中要做部门记录。且用户有事务组织字段。
责任中心：用户元素1（比如利润中心，成本中心）
业务板块：用户元素2（比如制造、零售）
。。。。。

单公司一般：
1. 核算组织多，账套组织很少（单独核算的门店或者研发部）
2. 每个部门一个组织，如果公司内部一旦出现账套组织，组织平衡段就要启动，要么：
2.1 核算组织的部门使用总账分配，将自动生成的会计分录自动消除。仅限很少部门，否则有点累。
2.2 

> ###账套组织如何不生成分录
> 既打开平衡段=Y，保证某些单个账套组织可以组织往来核算，又要保证部门这些安全边界组织不生成分录。
> 1. 总账分配，所有单据，一旦某个部门发生内部往来账户，全部取消
> 2. 待测试

1. 多组织好处
---

部门按照业务或者安全界限分为不同组织的好处：
1. 数据过滤
2. 可灵活设置，总部可以看分店订单，而分店之间无法透明
3. 部门可以单独核算，出损益表或者三大报表

2. 组织类型
---

组织类型官方默认没有逻辑，只能用于统计。按理有：
1. 法人组织：该组织必须有一个账套，账套按照该组织信息进行合法性公示。
2. 核算组织：只用于核算，不用三大报表
3. 账套组织：财务单独核算组织，可以出三大报表
4. 其他什么事业板块、人力资源、固定资产之类的组织边界先不讨论。

组织类型是为了结合系统的组织隔离机制，能满足不同财务核算的需要。但官方功能太弱，
二开思路：
1. 增加一个字段，组织类型。取值字典引用。
2. 增加一个引用，把什么法人组织、核算组织、账套组织都输入进去。
法人组织=L
核算组织=A
账套组织=B
合并组织=P，多账套合并使用该组织，相当于虚拟组织
后续接口：
板块组织=O
人力组织=H
资产组织=F
库存组织=I
3. 增加一个字段，平衡段。把账套维度的平衡段移到这里设置。
平衡段=Y，法人、账套、合并、板块、资产、库存
平衡段=N，核算
基本上，大多数组织具有平衡段属性，当一个账套同时存在 核算组织 和 账套组织 时，要么2个账套，要么二开。

3. 平衡段
---

实体默认账套，如果平衡段=Y，组织之间会产生内部组织间分录。
1. 如果实体=公司，组织=部门，取消该选项。所有组织只能出损益表，无法做三大报表。
2. 如果某个组织（研发部/门店）突然要做三大报表，要么单独建账套给组织，要么二开，该组织单独在公司账套能生成内部组织分录。

二开思路：
1. 组织类型=账套组织（相对于核算组织），如果是该类型，账套维度的平衡段失效，一定生成内部组织分录。
2. 或者账套维度的组织项下设表“排除例外组织”，或者增设例外组织json字段（oracle不支持）。

推荐实现1，到底是否产生分录，由组织类型决定。【组织类型实在需要扩展，如果扩展好，可以去掉平衡段】

4. 部门仓库
---

无论是否平衡段，组织间仓库无法直接使用，只能调拨。
按理：
1. 非平衡段，总部仓库和组织间仓库都是总账套的仓库。
2. 各组织对各仓库的关系是平等的，也就是没有管理边界，无过滤限制。

实际：
1. 仓库的边界由“组织是否是账套组织”决定，如果组织是财务核算，就是控制。如果不是，不控制。
2. 系统默认，只要是组织，不管你是核算组织（）

配置思路：
1. 去掉仓库的过滤条件，但是某些内置的流程会报错，因为仓库是默认取本组织仓库，而不是实体所有仓库。
2. 

