Model Validator
===

> ###Event Handler
> Build on top of the OSGi Event Admin Framework, the iDempiere platform enable the use of OSGi **event handler** to replace the use of AD_ModelValidator table and the ModelValidator interface. The IEventManager service is provided by the org.adempiere.base bundle.
> 
> As described in other Plug-In development tutorials, using the OSGi factory approach is very simple. First, you create a new class which implements the IModelValidatorFactory interface and it's method. In the newModelValidatorInstance(classname) methode, add an if-statement where you check if the proviced class name fits your model validator. If it does, return a new instance of your model validator. If not, return null. 
> 
> The next step is to create a component definition with a unique name and the path to your class. Don't forget to add a property called "service.ranking" which is a integer property. Give it a value of 100 or so to make sure it is loaded before the default model validator factory form idempiere. 
> 
> On the service tab of the component definition, add the IModelValidatorFactory interface to the provided services. 
> 
> Make sure that your component definition is added to your plug-ins manifest.mf. There should be an entry like "Service-Component: mycomponent.xml".
> 
> http://wiki.idempiere.org/en/Developing_Plug-Ins_-_Model_Events

What is a model validator?  

模型验证（Model Validator）用于定义表变更、单据状态、特殊事件的触发动作。
作为org.compiere.model.ModelValidator的实现，它包含：
1.modelchange：比如data changed, added or deleted
2.docValidate：比如complete, void 等
3.其他方法:比如SaveProperties, Login, FactValidate等

用法：
1. 需要修改或者增加订单逻辑（比如检查发货目的地址的物流方式），
2. 直接修改核心代码模块类MClass Mordle，将导致每次升级都需要单独合并此更改。【尽量不要】
3. 使用Model Validators完成自定义逻辑，将保证核心代码的一致性。

总之，不要直接修改官方的模型类（model classes)，而使用模型验证（Model Validators）来触发自定义逻辑。对于自定义表，使用Model class或者Validator，依据自身开发规范选择。

注意：
1. 也不要对官方表生成X_类，对于自定义列的取值或者赋值，可在PO中完成操作。
2. AD的表也有Table Script Validator来定义事件的规则。

参考：
http://wiki.adempiere.net/ModelValidator
http://wiki.adempiere.net/Extensions_Friendly_Proposal
http://wiki.idempiere.org/en/Developing_Plug-Ins_-_ModelValidator

modelchange事件
---

1. 数据插入
  * TYPE_BEFORE_NEW
  * TYPE_AFTER_NEW
2. 数据改变
  * TYPE_BEFORE_CHANGE
  * TYPE_AFTER_CHANGE
3. 数据删除 
  * TYPE_BEFORE_DELETE
  * TYPE_AFTER_DELETE

docValidate事件
---

1. BEFORE
2. AFTER CLOSE
3. REACTIVATE
4. REVERSECORRECT
5. REVERSEACCRUAL
6. COMPLETE
7. PREPARE
8. VOID
9. POST

其他事件
---

1. 登录事件：用户登录系统之后
2. 过账事件：分录过账之前
3. 加载首选事件：加载首选项之后
4. 保存首选事件：保存首选项之后

步骤
---

参考：http://wiki.adempiere.net/ModelValidator#Steps_to_create_and_register_a_model_validator

