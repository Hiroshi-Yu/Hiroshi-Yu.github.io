---
title: 本网站文章的书写帮助（Markdown+Chirpy）
date:  2024-08-08 +0800
categories: [社区管理, 帮助] #上级文文档，下级文档
tags: [社区管理]     # TAG
media_subpath : /assets/img/in-post/2024/2024-08-08/
description: 为本网站做文档贡献时的文章书写方法汇总
image:
    path: idempiere11_img_back.jpg
comments: false # 关闭评论    
math: true # 加载数学功能
Mermaid: true #图表生成工具
---

## 概要

想参与到文档书写的话，可以在通过在github上提交pull request的方式参与进来。
文档格式使用Markdown，同时支持Jekyll+Chirpy主题的一些格式优化语法。

> 这里仅显示效果，语法请在github上参看原文。
{: .prompt-tip }

---

## 1.Github仓的相关信息

#### **文章管理位置** - 文件夹：`_posts\[年]\[年-月-日]_[你的文章名].md`{: .filepath}

   > 例: 本文的命名方法：`_posts\2024\2024-08-08-help-markdown.md`{: .filepath}
{: .prompt-tip }

#### **图片的管理位置** - 文件夹：`/assets/img/in-post/[年]/[年-月-日]/`{: .filepath}

   > 例: 本文的图片管理位置：`/assets/img/in-post/2024/2024-08-08/`{: .filepath}
{: .prompt-tip }

---

## 2.文档语法

#### 眉批(Front Matter)
在文档的最前端定义的信息，决定了这个文档的各类属性。

```yaml
    ---
    title: 社区文章的书写帮助（Markdown+Chirpy）
    date: 2024-08-08 +0800
    categories: [社区管理, 帮助] #上级文文档，下级文档
    tags: [社区管理]     # TAG names should always be lowercase，可以有无数个标签
    description: . # 描述
    toc: false #
    author: # 作者信息
      name: Full Name
      link: https://example.com
    toc: false # 关闭目录
    comments: false # 关闭评论
    math: true # 加载数学功能
    mermaid: true # 启用Mermaid
    pin: true # 置顶帖子
    media_subpath : /assets/img/in-post/2024/2024-08-08/
    image:
        path: idempiere11_img_back.jpg
    ---
```

```markdown
    不希望显示行号时，在代码块后面另起一行添加下述内容
    {: .nolineno }
```
{: .nolineno }

#### 图片


```markdown
    ![图片测试](idempiere11_img_back.jpg){:.shadow width="250"}
    _图片说明下标1_
    ![图片测试](idempiere11_img_back.jpg){: width="500" height="250" }
    _图片说明下标2_
```

![图片测试](idempiere11_img_back.jpg){:.shadow width="250"}
_图片说明下标1_
![图片测试](idempiere11_img_back.jpg){: width="500" height="250" }
_图片说明下标2_

#### 提示
  > 提示 - \{: .prompt-tip }
{: .prompt-tip }

  > 信息 - \{: .prompt-info }
{: .prompt-info }

  > 警告 - \{: .prompt-warning }
{: .prompt-warning }

  > 危险 - \{: .prompt-danger }
{: .prompt-danger }

#### Task List
* [ ] task 1
* [x] task 2

#### 代码块
  ```java
  int main()
  {
      return 0;
  }
  ```

  > 内联代码
`hello，world!`

#### 表格

| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |

#### 数学公式

> 需要在头信息中添加以下信息
> 
> math: true # 加载数学功能
{: .prompt-tip }

1. 内联公式
$x+y$

2. 公式块
$$
\begin{Bmatrix}
   a & b \\
   c & d
\end{Bmatrix}
$$
$$
\begin{CD}
   A @>a>> B \\
@VbVV @AAcA \\
   C @= D
\end{CD}
$$

#### HTML元素定义
使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑

## 参考网站
- [Chirpy官网语法讲解](https://chirpy.cotes.page/posts/write-a-new-post/#table-of-contents)
- [Markdown语法及测试](https://www.cnblogs.com/olimi/p/16173745.html)
