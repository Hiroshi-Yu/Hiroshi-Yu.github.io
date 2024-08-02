Jasper报表
===

> ###未测试！未测试！未测试！2017-9-29
> 

概述
---

参考：
http://wiki.idempiere.org/en/Creating_reports_using_JasperReports
http://globalqss.com/idempiere/2.1_20141110/schemaspy
iDempiere已集成Jasper报表作为复杂报表开发工具，只需使用ireport工具编写即可。
你可以定制官方的org.adempiere.report.jasper模块，比如http://wiki.idempiere.org/de/JasperReportsClassic。

数据库表
---

如果是从fact_acct（分录明细表）直接汇总生成报表的话，肯定无法接受。
所以Fact_Acct_Balance这个视图就出现了，它记录分录的每日余额以加快报表的计算，但还是剩70%的原始数据，远远达不到减少计算的效果。
且每次计算报表前，还要更新这个用处不大的余额表，这导致了直接从fact_acct出财务报告非常慢。
![表](http://static.oschina.net/uploads/space/2016/0425/024442_UZRL_2720480.png)

另外一些表和视图：
![视图](http://static.oschina.net/uploads/space/2016/0425/024502_nuvq_2720480.png)

自带报表：
类名称: org.compiere.report.FinStatement（待确认，我无法在bitbucket找到源码）
表名称: T_ReportStatement
![计算结果](http://static.oschina.net/uploads/space/2016/0425/024521_ytW6_2720480.png)

iReport报表
---

####报表编写####
1. 下载最新的ireport编辑器（Jasper Studio没有测试）。
2. 一些用户安全的检查和准备工作。
3. 创建一个数据源。
4. 创建一个参数“RECORD_ID”测试一下。
5. 创建一个数据库查询
  - 输入或者拖放$P{RECORD_ID} 
  - 选择不同的字段，并排列
  - 设置页头区和明细区
  - 设置页脚区

####报表发布####
1. 绝对路径
2. 相对路径

####报表连接####
1. 系统管理员
2. 【System Admin】-【General Rules】-【Process & Report】
3. 新建一个Report记录
4. 输入你编写的*.jasper文件名

其他问题
---

1. 默认参数
参考org.adempiere.report.jasper/src/org/adempiere/report/jasper/ReportStarter.java
比如RECORD_ID, AD_PINSTANCE_ID, AD_CLIENT_ID, AD_ROLE_ID, AD_USER_ID, CURRENT_LANG

2. subreport子页
子页问题，请参考

3. 分页
sql ****** limit 20 offset 0 报错syntax error at or near ""limit""
解决办法：
sql ****** offset 0 fetch first 20 rows only

4. 字体
id的jasper库负责生成pdf。

==jre字体介绍==
http://docs.oracle.com/javase/8/docs/api/java/awt/Font.html java官方字体说明
http://www.docin.com/p-649145711.html
Java 2 平台可以区分两种字体：物理字体和逻辑字体。 
物理 字体是实际的字体库，包含字形数据和表，这些数据和表使用字体技术（如 TrueType 或 PostScript Type 1）将字符序列映射到字形序列。Java 2 平台的所有实现都支持 TrueType 字体；对其他字体技术的支持是与实现相关的。物理字体可以使用字体名称，如 Helvetica、Palatino、HonMincho 或任意数量的其他字体名称。通常，每种物理字体只支持有限的书写系统集合，例如，只支持拉丁文字符或只支持日文和基本拉丁文。可用的物理字体集合随配置的不同而不同。要求特定字体的应用程序可以使用 createFont 方法来捆绑这些字体，并对其进行实例化。 

逻辑 字体是由必须受所有 Java 运行时环境支持的。 Java 平台所定义的五种字体系列：Serif、SansSerif、Monospaced、Dialog 和 DialogInput。这些逻辑字体不是实际的字体库。而是由 Java 运行时环境将逻辑字体名称映射到物理字体。映射关系与实现和通常语言环境相关，因此它们提供的外观和规格各不相同。通常，为了覆盖庞大的字符范围，每种逻辑字体名称都映射到几种物理字体。

java jre字体配置文件为：%JAVA_HOME%/jre/fontconfig.properties.src （搜这个可以搜一堆）
http://www.cnblogs.com/super119/archive/2011/01/03/1924534.html java字体配置

字体搜索顺序：alphabetic,chinese-ms936,symbol ，这样英文字符、中文字符、符号都能按字体显示。

按照搜索顺序肯定可以找到一个字体，但是如果该字体文件不包含中文编码，java app运行后中文会显示"口口"。
你可以添加ttf字体到jre的字体目录，%JAVA_HOME%/jre/lib/fonts，

参考：
1. http://www.cnblogs.com/hahahjx/articles/3436019.html 给jre安装字体
2. http://corefonts.sourceforge.net 为linux类系统安装Microsoft's TrueType字体
3. https://idempiere.atlassian.net/browse/IDEMPIERE-1512 字体安装）

==Jasper相关==
All the jasper rendering in zk is done in the server.
1. 你可以修改jasper的字符配置

Jasper自带字体：/org.adempiere.report.jasper.library/lib/jasperreports-fonts-6.3.0.jar
这个包为jasper库自带的字体，只包含一种DejaVu字体包，包含3种样式，共12种字体。
sans：无衬线字体（也就是SansSerif，笔画黑体样式）
Serif：衬线字体（笔画带装饰边角样式）
mono：等宽字体（字符等宽样式）
Jasper的配置文件在jar目录：jasperreports_extension.properties
字符集定义位于jar的目录：net\sf\jasperreports\fonts\fonts.xml
你可以修改该文件，加入你想要的中文字体到jar包中，并发布。
http://blog.csdn.net/zxcnlmx/article/details/30241847

2. 你可以增加jasper的字体jar包

http://jasperreports.sourceforge.net/sample.reference/fonts/#fontextensions jasper官方文档
https://groups.google.com/d/msg/idempiere/ev8aEpBXwHs/kXc6nhr_CwAJ 添加字体到jasper库
http://jingyan.baidu.com/article/4d58d5411ed0de9dd4e9c0ae.html 添加字体到jasper库

- Step 1: Export Font you will use in Report with file extension is jar 
(Using Jasper Report IDE: Options -> Fonts -> "Export as extension")

- Step 2: Regist this font to jasper library in iDempiere Project
  + Copy Jar font file to jasper library project: net.sf.jasperreports....library
  + Regist jar font to osgi bundle of net.sf.jasperreports....library.
 => Rebuild iDempiere project & Review reult.

参考：
http://wiki.adempiere.net/ZH/Case-Study-01-Journal-22#.E5.85.B3.E4.BA.8EPDF.E4.B8.AD.E6.96.87.E9.97.AE.E9.A2.98.E7.9A.84.E8.A1.A5.E5.85.85 ad的实践
http://wiki.idempiere.org/en/Making_fonts_available_to_your_JasperReports 官方教程

=关于DejaVu字体
字体网站如：https://dejavu-fonts.github.io/
DejaVu字体是在 Bitstream Vera 的基础上扩展出来的，在原作（原作只支持几个拉丁文字）基础上扩充了好几种文字和符号，有国际音标、希腊文字、西里尔字母、亚美尼亚文、希伯来文、阿拉伯文、N'Ko 文字（这个不知道怎么翻译，听说用来拼写非洲语言）、泰文、老挝文、格鲁吉亚文、加拿大土著文字、欧甘文、上下标（Unicode 收了单独的上下标符号）、类字母符号、数字形式（包括罗马数字符号）、算术符号、技术用符号、带圈儿的字母数字、方块、几何图形、Miscellaneous Symbols（杂项符号）、Dingbats（叮呗符号）、盲文、提非纳文字、易经六十四卦、太玄经符号、傈僳文、古意大利文字（由希腊字母衍生出来，是拉丁文字的前身）、骨牌符号、扑克符号、音乐符号、Emoji。有三种授权：Bitstream Vera Fonts Copyright、Arev Fonts Copyright、公有领域。

=关于思源宋体

很显然，DejaVu字体包不会包含中文，支持中文的开源字体有：”思源宋体“

https://www.zhihu.com/question/24639343 了解”思源宋体“
https://github.com/adobe-fonts/source-han-serif/tree/release/  ”字体源码“

2017年4月3日，Google联合Adobe发布了全新的开源字体：思源宋体（Source Han Serif/Noto Serif CJK），旨在为东亚人口提供统一的字体，并针对屏幕显示进行了优化，提高各种设备的阅读体验。思源宋体总共拥有七种粗细字重，包括ExtraLight、Light、 Normal、Regular、Medium、Bold和Heavy，完全支持日文、韩文、繁体中文和简体中文,共计458745个字型，每一种字重包括65535个字形。

Noto字体家族的目标是支持世界上的所有语言。于是Google从2011年开始内部就启动Noto专案，在团队统筹下可以在不同语系中使用相同字型设计来获得最大和谐度。Google 在网站里提到Noto 字型家族一共涵盖800 种语言及110,000 个字元，势必会让Android 及ChromeOS 更加方便。整个Noto字体家族包括中日韩部分都免费开放给所有人使用。

==ireport相关==
1. 兼容性问题
不同版本的ireport或者jasper studio输出jrxml可能会导致字体异常，需要测试
https://groups.google.com/d/msg/idempiere/6oIYPdl8yzU/yPHb0_ny6HcJ  ireport不同版本问题
http://community.jaspersoft.com/wiki/ireport-fonts 字体说明

2. 设置问题
你ireport设置了某种中文字体，比如宋体，但是你服务器的jasper库没有宋体，那怎么办？
需要测试，建议按照jasper库的字符集来做设置ireport的字符集。

  http://blog.csdn.net/gong0585/article/details/40047971 ireport添加微软雅黑字体
  Font name:    宋体 (中文字体) 
  PDF font name:   STSong-Light 
  PDF Encoding:  UniGB-UCS2-H(Chinese Siplified) 
  PDF Embeded: √ （注意，嵌入字体将导致文件增大，但是各平台都能显示）
  可能问题：一个是加粗等效果没有作用，还一个是英文字母和数字的字体很不好看。

3. 没有加入中文字符
http://blog.csdn.net/lgh1117/article/details/52204822 添加iTextAsian-2.1.jar，iTextAsianCmaps-2.1.jar
http://blog.csdn.net/lgh1117/article/details/52204645 添加iTextAsian.jar和iTextAsianCmaps.jar

4. iText版本导致乱码
http://blog.sina.com.cn/s/blog_717c2b0f0100v1wl.html

  参考
  - org.compiere.print.layout.StringElement.paint
  - https://idempiere.atlassian.net/browse/IDEMPIERE-2737 （字体输出问题，未解决）

5. 图片
图片资源，请参考

![思源宋体版本选择（我选Subset OTF，7个字体，65M，后缀otf，JasperReport认otf）](https://static.oschina.net/uploads/space/2017/0929/185247_f8Xo_2720480.jpg)

