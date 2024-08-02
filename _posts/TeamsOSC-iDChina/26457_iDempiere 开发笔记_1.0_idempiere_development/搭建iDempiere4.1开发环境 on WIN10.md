搭建iDempiere4.1开发环境 on WIN10
===

## 说明：拷贝自http://osssme.org/node/140
   目前还没有把文档修改成Markdown结构


```
参看该网址进行安装：http://wiki.idempiere.org/en/Installing_iDempiere

参看章节为第二章：Installing iDempiere for Development
```

 

1. 环境概览

Windows 10 64 bits
PostgreSQL 9.6.1
注：Linux的话，需要添加PostgreSQL contrib来支持UUID
Mercurial Client 3.7.3
OpenJDK 1.8.0_91
Eclipse IDE for Java EE Developers 4.6.1 Neon.1
Mercurial Eclipse Plugin 2.2.0
Buckminster 4.5 (WARNING: special MODIFIED version)
 

2. 安装操作系统
    详细省略，本次安装使用的版本为10.0.14393

 

3. 安装数据库

    下载地址：https://www.postgresql.org/download/
    Windows版下载地址：http://www.enterprisedb.com/products-services-training/pgdownload#windows

    版本：9.6.1 for Win x86-64

    下载文件名：postgresql-9.6.1-1-windows-x64.exe

 

4. 安装Mercurial

    本次使用的是SourceTree

    下载地址：https://www.sourcetreeapp.com/

    点击：Also available for Windows

 

5. 安装JDK

    本次使用的是Oracle Java SE Development Kit 8

    下载JDK：http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2...

    文件名：jdk-8u112-windows-x64.exe

 

6. 安装Eclipse

    本次试用的是Eclipse 4.6 Neon

    选择 Eclipse IDE for Java EE Developers 64 bit

    下载地址：https://www.eclipse.org/downloads/eclipse-packages/

    文件名：eclipse-jee-neon-1a-win32-x86_64.zip

   

7. 安装Eclipse插件 - MercurialEclipse 2.2

    打开Eclipse，选择菜单Help > Eclipse Marketplace

    在查询框输入MercurialEclipse 2.2后点击GO按钮查询

    找到MercurialEclipse 2.2后点击Install按钮安装

    这里需要同意该插件的许可后方可进行安装

    安装完成后重启Eclipse

 

8. 安装Eclipse插件 - Buckminster 4.5

    这里其实有一个iDempiere的传奇故事，如果做完这个文档还有体力的话，我会在最后聊一聊这个传奇故事。

    打开Eclipse，选择菜单Help > Install New Software

    点击Add按钮后输入如下信息后点击OK按钮

     Fill Name: Buckminster 4.5

     Fill Location：https://netcologne.dl.sourceforge.net/project/idempiere/binary.file/jarfile/bm-p2/bucky-updates-4.5

     （备注：如果地址失效，确认如下地址的实际镜像位置https://sourceforge.net/projects/idempiere/files/binary.file/jarfile/bm-p2/bucky-updates-4.5/)

     找到相关插件后选择如下几个插件功能

     Buckminster - Core

     Buckminster - Maven support

     Buckminster - PDE support

     点击Next，Next后开始安装，

     这里需要同意该插件的许可后方可进行安装

     安装完成后重启Eclipse

 

 9. 下载代码

     打开SourceTree，点击克隆/新建，输入iDempiere源码库地址及本地仓库地址

     iDempiere源码库地址：https://bitbucket.org/idempiere/idempiere

     注：由于可能花费比较多的时间，有以下几个替代方案

     方案1： 先下载库之后再更新到最新版本

                 http://sourceforge.net/projects/idempiere/files/v4.1/source-repo/idempie...

                 解压缩后hg pull -u

     方案2：逐步更新

                先只下载第一个Patch，然后一点一点更新

                hg clone -r 1 https://bitbucket.org/idempiere/idempiere
                hg pull -r 100 -u
                hg pull -r 200 -u
                hg pull -r 300 -u
                hg pull -r 400 -u
                hg pull -r 500 -u
                不断地加号就可以，只是目前最大到11558左右，虽然比较烦，但总比一下下载到一半死掉要好一些。

     方案3：方案2的发展版，目前只有Linux版的批处理脚本

                http://wiki.idempiere.org/en/Hg_clone_idempiere_over_broken_line

 

10. 配置Eclipse开发环境

      1、打开Eclipse，变更Workspace
      选择菜单File > Switch Workspace > Other...
      指定刚才下载源码的目录文件夹为目标Workspace。

      2、指定源码根目录为targetPlatform文件夹（其实导入Buckminster时会自动创建该目标平台）。
      选择菜单Window > Preferences > Plug-in Development > Target Platform > Add...
      在第一个对话框选择Nothing:Start with an empty target definition，点Next按钮
      在第二个对话框的Name处输入iDempiere target Platform后点Add...按钮
          －－ 选择Directory后点Next按钮
          －－ 在Location中输入${workspace_loc}/targetPlatform
          －－ 点Finish按钮
      － 点Finish按钮
 

11. Eclipse的iDempiere初始化（强烈建议翻墙）

 

     # 该步骤之前可以尝试做的事情

        由于即使是翻墙还是有一些JAR包很难下载，尝试把所有JAR包都汇总起来，感觉上似乎在初始化的时候快了一点。

        网址：https://bitbucket.org/iDChina/idempiere_jar

        hg库地址：https://bitbucket.org/iDChina/idempiere_jar

 

      首先，点击菜单Window > Preferences > Ant > Runtime > Properties > Add External...

      选择文件org.adempiere.sdk.feature > materialize.properties

    

      然后，点击菜单Window > Preferences > General > Workspace

      在最下面扎到Text file encoding后选择Other: UTF-8

 

      接着，点击菜单File > Import > Buckminster > Materialize from Buckminster MSPEC, CQUERY or BOM，点击Next按钮

      选择本地代码库下的org.adempiere.sdk-feature/adempiere.mspec

      等待...... 按照中国的网速，可以下楼买杯果汁了

      页面可以使用后点击完成按钮

      继续更加漫长的等待...... 官网说可以去喝杯咖啡，最好是哥伦比亚咖啡。我也是这样觉得的，当然我们不反对二锅头或者大红袍。

     

      等待的结果其实也就是拼人品的结果，如果没有ErrorLog并且初始化成功，左侧的项目树变得慢慢的话，恭喜你。

      如果没有成功也别气馁，临时借个VPN翻个墙吧。

      替代方案：

 

      初始化成功后请务必选择所有的项目后右击后选择Refresh

      最后，Project > Build All

 

      当然，也许你会发现有的项目存在错误，一般情况下应该是某些jar包没有下载下来导致的，这个时候，在出问题的项目中找到copyjars.xml文件。

      之后鼠标右击这个文件，选择Run As > 1. Ant Build

      重新Refresh all project，Build All.

 

12. iDempiere的数据库初始化

      Postgresql 9.6.1提供了pgAdmin 4 v1.1来进行图形化数据库管理，感觉高大上了一些。但是没有找到通过pgAdmin打开psql的方法，这里还是使用命令行来进行数据库相关的配置。

      首先，找到iDempiere的数据库dump文件，位置：org.adempiere.server-feature\data\seed\Adempiere_pg.jar

      解压缩该文件后得到Adempiere_pg.dmp文件

 

      打开命令行工具，然后cd到Postgresql的bin目录

      -- 创建数据库

      createdb --template=template0 -E UNICODE -O adempiere -U adempiere idempiere

      -- 创建角色 （由于dump文件中指定了角色adempiere，所以只能用这个明细，需要修改的话，要直接手工修改dump文件）

      psql -U postgres -c "CREATE ROLE adempiere SUPERUSER LOGIN PASSWORD 'adempiere'"

      -- 导入数据库

      psql -d idempiere -U adempiere -f 本地iDempiere代码库下的org.adempiere.server-feature\data\seed\Adempiere_pg.dmp

 

13. iDempiere执行环境配置

      打开Eclipse菜单Run ->> Debug Configurations...

     选择Eclipse Application下的Install.app

      然后按Debug按钮，后续按正常安装方法配置安装信息。

 

14. 执行iDempiere

      打开Eclipse菜单Run ->> Debug Configurations...

      选择Eclipse Application下的server.product

       然后按Debug按钮

 

       打开浏览器，轻车熟路的输入http://localhost:8080/

       注：localhost这部分要和执行环境配置时指定的APP服务器保持一致。

