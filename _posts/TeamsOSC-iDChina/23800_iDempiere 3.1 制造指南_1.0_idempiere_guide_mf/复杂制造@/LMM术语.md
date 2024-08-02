LMM术语
===

概述
---

BOM
---

与系统简单BOM设置相比，LMM模块的BOM结构多了BOM版本。
产品--BOM--BOM Line
BOM类型：关键字段
BOM用途：关键字段

Make-to-Kit -> Create a Manufacturing Order, Receipt for the finish product and issue the Components automatically
Current Active -> Only one active BOM will be allowed for a product
Make-to-Order – > Components can have attribute set instance, otherwise it will not allow
Product Configure -> No Implementation
Repair -> No Implementation

Following BOM Use options are available

Master -> No Implementation
Engineering -> No Implementation
Manufacturing -> Create a Manufacturing Order, Receipt the finish product and issue the Components automaticaly
Planning -> No Implementation
Quantity -> No Implementation

Resource
---

资源类型：工厂plant，车间workcenter，产线line，工位station
资源：包含不可用规则，资源产品（会计），分配记录。

Workflow
---

工作流可对应常见的”工艺路线“概念。本节引用”工作流“来定义相关过程。
实际业务中的数据对应关系为：
1. 每个产品必须有一个工作流。（生产工单必须有1个产品+1个工作流<有多个更好>）
2. 工作流可分为资源类工作流（资源类型，资源特点），工艺类工作流（客户工序类型）

问题：如何选择工作流（资源，产品，工序）
1. 外包型制造
2. 工艺简单+自制
3. 工艺复杂+外包
4. 排程颗粒

问题：如何建立标准工时(排程因子)
1. 厂级标准工时：内协/外协排程
2. 车间标准工时：粗排程
3. 线别标准工时：最常见的排程级别
4. 工位标准工时：SOP级工时

![资源类工作流](http://static.oschina.net/uploads/space/2016/1103/164002_YbCI_2720480.png)

![工序类工作流](http://static.oschina.net/uploads/space/2016/1103/171459_9sb9_2720480.png)

工作流节点时间解释：
1. 排队时间：Queue，等待节点开始的时间。
2. 准备时间：Setup，节点生产的安排或者设置或者准备时间。
3. 加工时间：Work，节点的生产执行时间。
4. 等待时间：Wait，半成品等待入库或者下一个节点时间。
5. 转移时间：Move，半成品传送的时间。
6. 持续时间：Duration，节点之间的延迟时间delay time。
7. 持续时间限制：Duration Limit,节点延迟时间的最大值限制，超过可以设置动作。
8. 持续时间单位：Duration unit，时间单位。


场景：每个产品都会指定一个工作流，id是如何实现的
默认：LMM-计划管理-产品计划-新增产品计划-新增产品计划明细，指定工作流。

场景：工厂计划员关心车间级工作流，车间调度员关心线级工作流
默认：id工作流不支持工作流嵌套，节点动作预留子工作流接口，但需要二次开发。

场景：标准工时数据必须在创建工单之前输入，否则计划员无法排程
默认：LMM模块在SO订单准备后，即同步新建MO订单。在此之前新建工作流中的工时数据将决定排程颗粒。

场景：对于外协工厂，通常需要在SO订单中，指定加工工序，如何完成该定义
默认：自带SO订单无法在明细中指定加工工作流，需要添加字段“加工工序”。
1. 新建产品，新建BOM版本，新建BOM子件。
2. 新建SO订单，下拉新建一个加工工序（加工工作流），输入加工节点的计划工时，以方便生成MO进行粗排程。
3. 新建BOM版本的工程工艺路线（工程工作流，线级工作流），

销售用加工工作流：每个订单一个工作流（SMT-THT-DIP）

产品
 |--BOM版本--工程工作流(资源=工厂)
                |__工序1（资源=车间）

方案1：使用单独标准工时表，订单BOM的工作流节点拷贝工时数据
1. 新建产品（bom）。
2. 新建BOM版本，指定BOM版本（工程BOM）
3. 新建父产品标准工时记录，包含产品，资源和时间信息
4. 订单BOM生成后，新建生产工作流，拷贝标准工时表记录。（同资源同工序）

标准工时表：id，产品id，资源id，工序id，bom_id，5个时间字段，3个delay字段，人数

方案2：直接使用工作流记录标准工时
1. 新建产品（bom）。
2. 新建BOM版本，新建BOM子件产品。
3. 子产品的部件类型，决定了加工工序逻辑
4. 新建bom版本（工程BOM）的工作流（标准工作流），分配子件产品到节点（1子件-->多个节点）
5. 单独的分配Form更好的完成工作。


MO
---

包含：
Product：做什么
BOM：用什么做
Resource：在哪里做
Workflow：制造路线，包含工序（设备、人力、产品、标准工时）

