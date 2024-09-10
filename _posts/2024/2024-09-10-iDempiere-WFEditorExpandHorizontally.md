---
title: 工作流编辑器的水平展开
date:  2024-09-10 +0800
categories: [iDempiere学习] #上级文文档，下级文档
tags: [Google Group研究][iDempiere开发]     # TAG
media_subpath : /assets/img/in-post/2024/2024-09-10/
description: 工作流编辑器的水平展开
image:
    path: modular.png
---

## 工作流编辑器的水平展开

### 信息源

Google Group: [Workflow Editor doesn't expand horizontally](https://groups.google.com/g/idempiere/c/STQQCsi-K0o)
 
![googleGroupMessage](googleGroupMessage.png)
_原文_

### 思路

目前的工作流编辑器是能显示4列，无法水平自动展开，行是可以自动扩展的。

经过确认，4列这个控制的确是代码中写死了的，在系统中应该是没有办法简单的做自动的水平扩展（如果需要自动的话，改动的代码量有点大）

### 对应方法

---
**类：org.adempiere.webui.apps.wf.WFNodeContainer**

变更前：
```
    private int noOfColumns = 4;
```

变更后：
```
public final static int COLUMNS = 5;
private int noOfColumns = COLUMNS;
```

---
**类：org.adempiere.webui.apps.wf.WFEditor**

**方法：	private void load(int workflowId, boolean reread) **

变更前：
```
	for(int c = 0; c < 4; c++) {
```

变更后：
```
	for(int c = 0; c < WFNodeContainer.COLUMNS; c++) {
```

### 效果

![move1](move1.png){:.shadow width="250"}
_拖拽中_

![move1](move1.png){:.shadow width="250"}
_拖拽完成_

### 其他

正如描述的
> I'm quite sure that some years ago, the editor was able to expand horizontally.
> At the moment, it seems the number of colums is hardcoded to 4.
我看了下Request的工作流，的确是五列的，不知道什么时候变成了现在这个样子，不过我个人觉得四列显示其实美观性实用性都没有什么问题。

![old](old.png)
_请求的工作流_

