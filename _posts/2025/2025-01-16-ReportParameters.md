---
title: 如何隐藏报表中的参数显示
date:  2025-1-16 +0800
categories: [iDempiere开发] #上级文文档，下级文档
tags: [iDempiere开发]     # TAG
description: 如何隐藏报表中的参数显示
media_subpath : /assets/img/in-post/2025/2025-01-16/
---

## 提问源
标题：Report - parameters
内容：Is there any way to remove the parameters used in the report_view?

官方论坛 [Report - parameters](https://groups.google.com/g/idempiere/c/REh5fiFUgbQ)

## 对应方法

从理论上讲，不显示Report参数这个需求是错误的，为了今后自己想调整显示内容时便于查找对应的代码，做了简单的调查。

对应效果：
![去除参数信息效果图](reportparameters.png)

对应代码如下：
类： org.idempiere.print.renderer.HTMLReportRenderer
方法：createHTML

把下面的代码删除

  ```java
    if (parameterTable != null) {
        paraWrapId = cssPrefix + "-para-table-wrap";
        w.print("<details id='" + paraWrapId + "' open=true style='cursor:pointer'>");
        w.print("<summary style='cursor:pointer'>"+Msg.getMsg(Env.getCtx(), "Parameter")+"</summary>");
        w.print(compress(parameterTable.toString(), minify));
        
        tr tr = new tr();
        tr.setClass("tr-parameter");
        
        MQuery query = reportEngineQuery;
        if (reportEngineQuery.getReportProcessQuery() != null)
            query = reportEngineQuery.getReportProcessQuery();
        for (int r = 0; r < query.getRestrictionCount(); r++)
        {
            if (r > 0) {
                tr = new tr();
                tr.setClass("tr-parameter");
            }
            
            td td = new td();
            tr.addElement(td);
            td.addElement(query.getInfoName(r));
            
            td = new td();
            tr.addElement(td);
            td.addElement(query.getInfoOperator(r));
            
            td = new td();
            tr.addElement(td);
            td.addElement(query.getInfoDisplayAll(r));
            
            w.print(compress(tr.toString(), minify));
        }
                            
        w.print("</table>");
        w.print("</details>");
    }
  ```

替换成如下代码
  ```java
    if (parameterTable != null) {
        paraWrapId = ""; 
    }
  ```
