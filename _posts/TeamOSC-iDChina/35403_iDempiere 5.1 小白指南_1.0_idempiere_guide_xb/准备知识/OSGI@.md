OSGI@
===

In ADempiere we had the "classpath hell". The core, patches, extensions and customizations etc. were all in one single classpath namespace. That made it very hard to care about the versions of all the jarfiles that are used from different parts of the code. OSGi solves that problem by separating namespaces for classpaths into different plugins. In OSGi talk plugins are called "bundles".

The different plugins allow to separate and isolate different things. You know who is responsible for which part of the code.

OSGi interfaces work by using implementations that are done as an OSGi interface and use Factory classes to find them through the OSGi layer. There is an entry "service ranking" for every implementation.

JIRA是一个典型的osgi web应用，可惜不是开源。

模块化之争
---

http://www.infoq.com/cn/news/2017/05/no-jigsaw?utm_source=news_about_jcp&utm_medium=link&utm_campaign=jcp

Equinox
---

鲨鱼

MANIFEST.MF
---

鲨鱼

ogsi开发web应用
---

http://blog.csdn.net/xo_zhang/article/details/9192341

基于OSGI开发B/S应用有两种方式：

1）在OSGI框架中嵌入Http服务器

Run Configuration，导入以下bundle

org.eclipse.equinox.http.jetty
org.eclipse.equinox.http.servlet
org.eclipse.osgi
org.eclipse.osgi.services
javax.servlet
org.apache.commons.logging
org.mortbay.jetty

2）在Servlet容器中嵌入OSGI框架（略）

osgi打包
---

http://zhoufu24.iteye.com/blog/131787

Bundle的生命周期
---

http://blog.csdn.net/liubag/article/details/22825217 Bundle的生命周期

1 当用户执行install命令后，Bundle就被安装在OSGi平台上并进入INSTALLED状态。

2 当我们执行start命令时，OSGi平台会首先执行resolve操作。如果操作成功，Bundle就会进入RESOLVED状态。一个Bundle进入RESOLVED状态表示Bundle所有需要的条件都满足了，包括以下三个方面：
1）Java运行环境满足或者超出Bundle的运行需求；
2）Bundle所声明导入的包都由所依赖的插件导出了并且版本一致；
3）Bundle所依赖的插件都可以访问并且处于RESOLVED状态，或者能够与该Bundle同时处于RESOLVED状态。

3 无论是在INSTALLED状态还是RESOLVED状态，执行refresh或者update操作都会使Bundle进入INSTALLED状态。

4 Bundle进入RESOLVED状态后，start命令会使Bundle进入Starting状态。在这个状态中，Bundle Activator的start()方法会被执行，用于分配或获取需要的资源。这个过程应该尽量的短，使得Bundle能够快速启动起来。当启动操作执行完毕后，Bundle会自动进入到ACTIVE状态。

5 在ACTIVE状态的Bundle如果被执行stop命令，Bundle就会进入Stopping状态。在这个状态中，Bundle Activator的stop()方法会被执行，用于释放在Bundle启动时所分配或者获取的资源。当停止操作完成后，Bundle会自动进入RESOLVED状态。

6 无论是在INSTALLED状态还是RESOLVED状态，执行uninstall操作都会使Bundle进入UNINSTALLED状态。

![](https://static.oschina.net/uploads/space/2017/0827/044525_quuD_2720480.jpg)

