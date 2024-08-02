IDE工具@
===

Eclipse or IntelliJ IDEA
---

PDE
---

Eclipse自带插件开发环境PDE(Plug-in Develop Environment)
http://blog.sina.com.cn/s/blog_3ee5fa930100091m.html  eclipse中plugin与feature的区别
https://www.ibm.com/developerworks/cn/linux/opensource/os-ecfeat/  使用 PDE 构建功能部件的策略

PDE 将为一个 plugin.xml 或 feature.xml 文件生成 build.xml 文件。Build.xml是一个 Ant 脚本，可以完成运行期平台的功能部件和插件所需要的不同任务。

测试结果表明如果您改变了一个 Java 源文件，相应的源文件压缩包将会更新，但是类不会被重新编译，运行期 JAR 也不会被更新。所以安全起见，建议您先将输出结果清空，以保证您在重新构建后所使用的是对应于当前源文件的运行期版本。

当一个新的文件或目录添加到功能部件或者插件时，还有必需的响应需要考虑。我们的意思是您得让您的功能部件和插件能应付可能的改变，也就是说它们分别需要一个 feature.properties 文件和一个 plugin.properties 文件。当您用包含策略时，您需要给 .properties 文件添加一个适当的属性，如果是使用排斥策略的话就不用这样做了。

不论哪种方法，如果您把一个文件添加到一个目录，那么不需要做任何其他的改动。对于新添加的文件，如果使用的是包含策略，它会被发送处理，如果使用的是排斥策略，它将不会被发送处理。这实际上是您可能应该要考虑使用不同的目录来存放不同的文件的原因。例如，您的插件所用到的所有图片所在的目录应该被包含在内，而一个开发过程中存放插件的设计讨论或文档的目录则不然。

最坏的情形是当您忘记对 build.properties 进行更新时：会发生运行期失败或生成内容存在差错的产品。如果您使用包含策略，新加入的文件或目录在打包后是不可用的，这有可能会导致您的插件不能用或者显示出 Eclipse 默认的图标（红盒子）。如果是使用排斥策略添加新文件或目录，这些文件或目录的内容在打包过程中会被包含进来。根据您的风格选择适当的方法，从而把您忘记执行更新时的风险降到最低。这将取决于您所面临的主要问题：是功能部件或插件不能运行，还是其他人本不应该看到运行期目录下的文件。

当您在开发您的工具时，您是否考虑到了需要多少个插件？答案是至少三个：一个是您的模型，也就是非 UI 的核心部分，一个是您的 UI 内容，还有一个或多个是用于提供帮助内容。如果您注意过，您会发现这是 Eclipse 本身的基本模式（jdt.core, jdt.ui, jdt.doc; debug.core, debug.ui;等等）。
这样划分的原因之一是，相对于不用于 UI 的插件来说，用于 UI 的插件在运行期需要不同的 Eclipse 组件的支持（org.eclipse.ui）。

build.properties 文件的2种策略：
包含策略----指出所需要的部分
至少到刚开始时，最简单的方法是，在构建过程中将您要打包的部分作为在运行期配置中功能部件或插件的一部分而列出来。

排斥策略----指定不需要的或私有的部分
另一种方法是把您不想打包的部分在构建过程中作为功能部件或插件的一部分列出来。这不仅要包括您不想共享的文件，还要包括构建过程中创建的文件和目录（有一些是临时的）。

feature.xml
---

http://blog.sina.com.cn/s/blog_3ee5fa930100091m.html  eclipse中plugin与feature的区别

p2
---

p2 全称是 provisioning platform ，用于替代 Eclipse 3.4 及以前版本中的 Update Manager 功能，用于管理 Eclipse 插件的安装、搜索升级等。

p2 的UI替换了原来的 Update Manager 功能的菜单 Help > Software Updates，包含了安装、升级站点管理等功能。

http://www.eclipse.org/equinox/p2/ 官方介绍
https://www.ibm.com/developerworks/cn/opensource/os-eclipse-equinox-p2/  IBM的教程
http://blog.csdn.net/luoww1/article/details/38294325 p2实战

debug
---

http://blog.csdn.net/jackpk/article/details/7655777  Eclipse Debug 应用详解

