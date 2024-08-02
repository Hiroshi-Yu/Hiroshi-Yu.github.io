Java@
===

没有框架，java如何实现？
软文：Jave EE都快死了？     【没有不死的东西，就当作学dos吧】
http://www.oschina.net/news/79460/it-is-absolutely-that-java-ee-will-die-soon


编码
---

《深入理解计算机系统》，《tcp/ip详解 卷一、二、三》，《数据结构与算法》，《大话设计模式》

Java8
---

==java proxy类==
http://www.cnblogs.com/flyoung2008/archive/2013/08/11/3251148.html
一个典型的动态代理创建对象过程可分为以下四个步骤：
1、通过实现InvocationHandler接口创建自己的调用处理器 IvocationHandler handler = new InvocationHandlerImpl(...);
2、通过为Proxy类指定ClassLoader对象和一组interface创建动态代理类
Class clazz = Proxy.getProxyClass(classLoader,new Class[]{...});
3、通过反射机制获取动态代理类的构造函数，其参数类型是调用处理器接口类型
Constructor constructor = clazz.getConstructor(new Class[]{InvocationHandler.class});
4、通过构造函数创建代理类实例，此时需将调用处理器对象作为参数被传入
Interface Proxy = (Interface)constructor.newInstance(new Object[] (handler));
为了简化对象创建过程，Proxy类中的newInstance方法封装了2~4，只需两步即可完成代理对象的创建。
生成的ProxySubject继承Proxy类实现Subject接口，实现的Subject的方法实际调用处理器的invoke方法，而invoke方法利用反射调用的是被代理对象的的方法（Object result=method.invoke(proxied,args)）

https://www.zhihu.com/question/20794107 Java 动态代理作用
③ 这也是为什么Spring这么受欢迎的一个原因
Spring容器代替工厂，Spring AOP代替JDK动态代理，让面向切面编程更容易实现。
在Spring的帮助下轻松添加，移除动态代理，且对源代码无任何影响。

JVM
---

《深入理解Java虚拟机》
http://www.cnblogs.com/smyhvae/p/3788534.html java JDK开发环境搭建及环境变量配置
http://www.cnblogs.com/hqji/p/6582365.html JVM的栈、堆、方法区
http://qindongliang.iteye.com/blog/2199633  JVM之垃圾回收

http://blog.csdn.net/jiangguilong2000/article/details/47975457 JDK8引进的JVM参数变化记录

1.PermGen空间被移除了，取而代之的是Metaspace
以前：-XX:PermSize=64m -XX:MaxPermSize=128m 
现在：-XX:MetaspaceSize=64m -XX:MaxMetaspaceSize=128m 否则起不来

2.CompressedClassSpaceSize = 1073741824 (1024.0MB) 多出了这块，
CompressedClassSpaceSize的调优只有当-XX:+UseCompressedClassPointers开启了才有效-XX:CompressedClassSpaceSize=1G
由于这个大小在启动的时候就固定了的，因此最好设置得大点。没有使用到的话不要进行设置JVM后续可能会让这个区可以动态的增长。不需要是连续的区域，只要从基地址可达就行；可能会将更多的类元信息放回到元空间中；未来会基于PredictedLoadedClassCount的值来自动的设置该空间的大小。

JDK or Openjdk
---

http://playkid.blog.163.com/blog/static/56287260201372113842153/ jdk,jre,jvm关系
JDK（Java Development Kit）是针对Java开发员的产品，是整个Java的核心，包括了Java运行环境JRE、Java工具和Java基础类库。Java Runtime Environment（JRE）是运行JAVA程序所必须的环境的集合，包含JVM标准实现及Java核心类库。JVM是Java Virtual Machine（Java虚拟机）的缩写，是整个java实现跨平台的最核心的部分，能够运行以Java语言写作的软件程序。

![](https://static.oschina.net/uploads/space/2017/0825/155258_0pFr_2720480.jpg)

J2EE / Jave EE
---

https://baike.baidu.com/item/Java%20EE 百科解释JavaEE5
https://www.oschina.net/news/77276/java-e-e-8-0 JavaEE8 还没有出来（2017-8-25）
http://www.oschina.net/news/80874/java-ee-users-need-more-rest-survey-shows 2017的技术调查

==历史==
Sun公司在1998年发表JDK1.2版本的时候， 使用了新名称Java 2 Platform，即“Java2平台”，修改后的JDK称为Java 2 Platform Software Develping Kit，即J2SDK。并分为标准版(Standard Edition，J2SE), 企业版(Enterprise Edition，J2EE)，微型版(MicroEdition，J2ME)。J2EE便由此诞生。
2005年6月，JavaOne大会召开，SUN公司公开Java SE 6。此时，Java的各种版本已经更名以取消其中的数字“2”：J2EE更名为Java EE，J2SE更名为Java SE，J2ME更名为Java ME。
Java2平台包括标准版（J2SE）、企业版（J2EE）和微缩版（J2ME）三个版本。

J2EE（Java 2 Platform, Enterprise Edition）是一个为大企业主机级的计算类型而设计的Java平台。Sun微系统（与其工业伙伴一起，例如IBM）设计了J2EE，以此来简化在受客户级环境下的应用开发。

J2EE是一套全然不同于传统应用开发的技术架构，包含许多组件，主要可简化且规范应用系统的开发与部署，进而提高可移植性、安全与再用价值。
J2EE核心是一组技术规范与指南，其中所包含的各类组件、服务架构及技术层次，均有共同的标准及规格，让各种依循J2EE架构的不同平台之间，存在良好的兼容性，解决过去企业后端使用的信息产品彼此之间无法兼容，企业内部或外部难以互通的窘境。
J2EE组件和“标准的” Java类的不同点在于：它被装配在一个J2EE应用中，具有固定的格式并遵守J2EE规范，由J2EE服务器对其进行管理。J2EE规范是这样定义J2EE组件的：
客户端应用程序和applet是运行在客户端的组件；
Java Servlet和Java Server Pages (JSP) 是运行在服务器端的Web组件；
Enterprise Java Bean (EJB )组件是运行在服务器端的业务组件。

==EE5规范==
JDBC
（Java Data Base Connectivity,java数据库连接）是一种用于执行SQL语句的Java API，可以为多种关系数据库提供统一访问，它由一组用Java语言编写的类和接口组成。JDBC提供了一种基准，据此可以构建更高级的工具和接口，使数据库开发人员能够编写数据库应用程序。
与之相对应的是微软公司开发的ODBC(Open Database Connectivity）它建立了一组规范，并提供了一组对数据库访问的标准API（应用程序编程接口）。这些API利用SQL来完成其大部分任务。ODBC本身也提供了对SQL语言的支持，用户可以直接将SQL语句送给ODBC。 

JNDI
(Java Naming and Directory Interface,Java命名和目录接口)是SUN公司提供的一种标准的Java命名系统接口，JNDI提供统一的客户端API，通过不同的访问提供者接口JNDI服务供应接口(SPI)的实现，由管理者将JNDI API映射为特定的命名服务和目录系统，使得Java应用程序可以和这些命名服务和目录服务之间进行交互。
在没有JNDI之前：开发的时候，在连接数据库代码中需要对JDBC驱动程序类进行应用，通过一个URL连接到数据库。但是这样存在问题，比如我要改一种数据库，是不是要更换驱动，更换URL。每次都要进行这些配置和管理。
在有了JNDI之后：可以在J2ee容器中配置JNDI参数，定义一个数据源，在程序中，通过数据源名称引用数据源从而访问后台数据库。在程序中定义一个上下文类，然后用content.lookup("java:数据源名称")就可以成功引入数据源了。

EJB
EJB(Enterprise JavaBean)是sun的JavaEE服务器端组件模型，设计目标与核心应用是部署分布式应用程序。用通俗的话来理解，就是把已经打包好的东西放到服务器中去执行，这样是凭借了java跨平台的优势，利用EJB技术部署分布式系统可以不限于特定的平台。EJB定义了服务器端组件是如何被编写以及提供了在组件和管理它们的服务器和组件间的标准架构协议.

RMI
RMI(Remote Method Invocation，远程方法调用）是Java的一组拥护开发分布式应用程序的API。RMI使用Java语言接口定义了远程对象，它集合了Java序列化和Java远程方法协议(Java Remote Method Protocol)。我的理解是：能够让在某个 Java 虚拟机上的对象调用另一个 Java 虚拟机中的对象上的方法。可以用此这个方法调用的任何对象必须实现该远程接口。

Java IDL/CORBA
Java IDL技术在Java平台上添加了CORBA(Common Object Request Broker Architecture)功能，提供了基于标准的互操作能力和连接性。Java IDL技术使得分布式的Java Web应用能够通过使用工业标准的IDL和IIOP(Internet Inter-ORB Protocol)来透明地调用远程网络服务的操作。运行时组件(Runtime Components)包括了一个用于分布式计算且使用IIOP通信的Java ORB.我对这个规范的理解，它也是借用了java的集成，让新旧系统集成，或是客户端跨平台的使用。

JSP
JSP全名为Java Server Pages，中文名叫java服务器页面，其根本是一个简化的Servlet设计，它是由Sun Microsystems公司倡导、许多公司参与一起建立的一种动态网页技术标准。JSP的定义让我想到做BS项目时候的ASP.NET技术。JSP页面也是用HTML和JS的交互，服务器在页面被客户端所请求以后对这些Java代码进行处理，然后将生成的HTML页面返回给客户端的浏览器。

Java Servlet
一种J2EE组件，servlet可被认为是运行在服务器端的applet，Servlets提供了基于组件、平台无关的方法用以构建基本Web的应用程序。Servlet必须部署在Java servlet容器才能使用，为了在web容器里注册上面的Servlet，为应用建一个web.xml入口文件。servlets全部由Java写成并且生成HTML。

XML
（Extensible Markup Language）可扩展标记语言，标准通用标记语言的子集，是一种用于标记电子文件使其具有结构性的标记语言。近年来，随着 Web的应用越来越广泛和深入，人们渐渐觉得HTML不够用了，HTML过于简单的语法严重地阻碍了用它来表现复杂的形式。尽管HTML推出了一个又一个新版本，已经有了脚本、表格、帧等表达功能，但始终满足不了不断增长的需求。
有人建议直接使用SGML 作为Web语言，这固然能解决HTML遇到的困难。但是SGML太庞大了，用户学习和使用不方便尚且不说，要全面实现SGML的浏览器就非常困难，于是自然会想到仅使用SGML的子集，使新的语言既方便使用又实现容易。正是在这种形势下，Web标准化组织W3C建议使用一种精简的SGML版本——XML应运而生了。

JMS
JMS即Java消息服务（Java Message Service）应用程序接口是一个Java平台中关于面向消息中间件（MOM）的API，用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。用一个很形象的例子，如果有人请我吃饭，她给我打电话占线，她决定先去占个位置，但是如果没有短信技术，那么是不是我就不知道她给我的消息了呢？为了保证这样的异步通信，我可以看到短信，准时去赴约。JMS就是提供了这样一个面向消息的中间件。它们提供了基于存储和转发的应用程序之间的异步数据发送，即应用程序彼此不直接通信，而是与作为中介的MOM 通信。MOM提供了有保证的消息发送，应用程序开发人员无需了解远程过程调用（PRC）和网络/通信协议的细节。

JTA
JTA，即Java Transaction API，JTA允许应用程序执行分布式事务处理——在两个或多个网络计算机资源上访问并且更新数据。JDBC驱动程序的JTA支持极大地增强了数据访问能力。我得理解是，利用了事务处理，可以让数据等到同步的更新，技术上可以支持多个服务器的分布式访问。

JTS
组件事务监视器（component transaction monitor）按照事务性对象的调用方法定义。这样可以使得资源透明被征用。

JavaMail
JavaMail API提供了一种独立于平台和独立于协议的框架来构建邮件和消息传递应用程序。JavaMail API可以为使用一个可选包Java SE平台也包括在java EE平台.The JavaMail API provides a platform-independent and protocol-independent framework to build mail and messaging applications. The JavaMail API is available as an optional package for use with the Java SE platform and is also included in the Java EE platform.——Oracle官网。我的理解：是一个提供给使用Java平台的开发者处理电子邮件有关的编程接口。

JAF
JAF 是一个平台，是基于java平台的一个扩展，它的好处是：让你利用标准的平台服务，决定一个任意类型的数据，封装并访问它。发现可用的操作，并适用于实体bean来执行操作。JavaBeans Activation Framework (JAF) is a standard extension to the Java platform that lets you take advantage of standard services to: determine the type of an arbitrary piece of data; encapsulate access to it; discover the operations available on it; and instantiate the appropriate bean to perform the operation(s).
我的理解：JavaMail利用JAF来处理MIME编码的邮件附件。MIME的字节流可以被转换成Java对象，或者转换自Java对象。大多数应用都可以不需要直接使用JAF。

![](https://static.oschina.net/uploads/space/2017/0825/155956_2uFN_2720480.png)

EJB
---

http://dev.yesky.com/ejb/ EJB专区
http://book.51cto.com/art/201008/220990.htm EJB概述
http://book.51cto.com/art/201008/220991.htm EJB的意义
http://book.51cto.com/art/201008/220992.htm EJB的历史
jsf , jpa , ejb , jta , jms , jndi , jms , cache

在J2EE里，Enterprise Java Beans(EJB)称为Java 企业Bean，是Java的核心代码，分别是会话Bean（Session Bean），实体Bean（Entity Bean）和消息驱动Bean（MessageDriven Bean）。在EJB3.0推出以后，实体Bean被单独分了出来，形成了新的规范JPA。

EJB( 业务逻辑代码 ) 表示了与特定商业领域（例如银行、零售等行业）相适应的逻辑。它由运行在业务逻辑层的 enterprise bean 处理。一个 enterprise bean 可以从客户端接受数据，对它进行处理，并将其发送到企业信息系统层以作存储；同时它也可以从存储器获取数据，处理后将其发送到客户端应用程序。Session bean 描述了与客户端的一个短暂的会话。当客户端的执行完成后，session bean 和它的数据都将消失；与之相对应的是一个 entity bean 描述了存储在数据库表中的一行持久稳固的数据，如果客户端终止或者服务结束，底层的服务会负责 entity bean 数据的存储。Message-driven bean 结合了 session bean 和 Java信息服务（JMS）信息监听者的功能，它允许一个商业组件异步地接受 JMS消息。

回顾EJB出现以前的Java应用开发，大部分开发者直接用JSP页面，再加上少量Java Bean就可以完成整个应用，所有的业务逻辑、数据库访问逻辑都直接写在JSP页面中。系统开发前期，开发者不会意识到有什么问题，但随着开发进行到后期，应用越来越大，开发者需要花费大量时间去解决非常常见的系统级问题，反而无暇顾及真正需要解决的业务逻辑问题。
https://baike.baidu.com/item/EJB/144195?fr=aladdin

对于EJB来说，它提供了一种良好的组件封装，EJB容器负责处理如事务、访问控制等系统级问题，而EJB开发者则集中精力去实现业务逻辑；对页面开发者而言，EJB的存在无须关心，EJB的实现无须关心，他们只要调用EJB的方法即可。
http://blog.csdn.net/li_xiao_ming/article/details/52793879

EJB的复杂不在于声明事务，相反声明事务是EJB最大的亮点，EJB的复杂在于它和容器捆绑太死，以至于在开发中的测试上非常困难。 测试的方便性直接影响到开发的效率，Spring比之EJB最大的提升就是易测试上，其次就是使用POJO声明事务。 

Spring是从劳动中来到劳动中去的框架，而非EJB这种皇家血统的。
http://blog.csdn.net/hanpompy/article/details/7622251

a. EJB实现原理： 就是把原来放到客户端实现的代码放到服务器端，并依靠RMI进行通信。
b. RMI实现原理 ：就是通过Java对象可序列化机制实现分布计算。
c. 服务器集群： 就是通过RMI的通信，连接不同功能模块的服务器，以实现一个完整的功能。
http://www.cnblogs.com/strugglion/p/6027318.html

目前ejb已经出到了3.x了，然而国内已经几乎没有使用ejb3.x，有的也是ejb2.x，都是老系统遗留，有的是银行项目，有的是erp项目(都是大型项目)。

之前Jboss出名就是因为它支持ejb，并且支持得最好，然而现在随着ejb的使用份额下降，这几年jboss在国内的使用份额也下降了，用tomcat和其他开源服务器多了很多。

我也很少用ejb，在开发中根本不用，除非是以前为了支持一些老系统，才会接触，而且为了应付ejb的，学了ejb3.0，但是遇到的项目全部是ejb2.x，ejb3.0相比ejb2.0，简单了很多，不想ejb2.0那么繁琐，为了一个业务逻辑类，要写业务接口类，home类啥啥的，我觉得进步很大，但是在国内依然没什么人使用。

这里就谈一下我对ejb的一些看法:

1.ejb是比较重量级:实现ejb是一件很复杂的事情，J2EE规范中ejb规范的实现是一块硬骨头，而ejb实在复杂，很多开发人员都喜欢简单的东西，复杂的东西，会带来未知风险。

2.ejb的移植性低:虽然ejb是遵循J2EE规范的，但是各个厂家在实现J2EE EJB的规范的同时，也会加入自己的一些实现，例如你要让一个EJB查找一个数据源，这个数据源如何配置，几乎所有的应用服务器厂商都不一样,weblogic，websphere，jboss，apusic都有自己的一套实现方式。我每次想到移植ejb的应用，最怕是去找这些不同点，然后一一修改相应配置。这无疑加大了开发人员的负担，java的东西，平台可移植性就是一大亮点。

3.ejb调用流程:ejb是支持远程调用，客户端值需要ejb的业务类接口，服务端值需要ejb的业务类实现，然后客户端只需要调用jndi(这个规范实现很复杂，使用比较简单,还是很多人用)的lookup方法，就可以使用业务类接口直接调用服务端实现类的方法了，这中间，应用服务器做了很多处理,基本流程都是:客户端通过jndi和业务类接口，调用对应ejb应用服务器厂商jndi的lookup服务端实现类方法，然后ejb应用服务器生成业务类的存根和代理，存根从服务端序列化到客户端，客户端调用的ejb业务接口就是调用这个存根，然后这个存根又通过rmi协议或者iiop协议发送命令到业务类的代理，代理再调用业务类实现，最终把执行结果返回到客户端。

3.ejb难以调试:看到ejb的调用流程，虽然看上去ejb让用户不用了解远程调用细节，使用简单，但是由于里面的调用过程复杂，一旦有一个环节错了，用户都难以调试，排错，开发过程中出现问题不可避免，而解决ejb的问题，解决周期要比较久。出错的时候，错误信息也千奇百怪。

4.ejb的性能问题:ejb的调用涉及太多类的序列化和反序列化，本来通过网络传输已经很慢了，还要传递对象，数据量又更大了，还要涉及了对象的序列化和反序列化，这中间有太多的开销了。

5.EJB的替换开源产品太多了:现在业务逻辑，在java上要用框架的有spring，远程调用，有webservice(apache cxf已经做得很好了，而且webservice又是通用标准)，mina(一个apache的NIO框架)，netty(现在性能最快的NIO框架，来自jboss).而且这些产品都是可移植，社区交流多，出了问题，google就找到了。
http://www.cnblogs.com/ggjucheng/archive/2012/01/06/2314067.html

这些工具既没有从形形色色的企业应用中，抽象出隐藏在不同表现后面的本质特征，也没有创造性地用Stateless Session Bean和Stateful Session Bean来描述千变万化的现实世界。工具只是工具，不出两年就会有新的后起之秀，取而代之，但思想的光辉将长久地照亮技术的未来。EJB是一种思想，更是一种理想，尽管理想和现实总是存在差距，但这不能成为我们放弃EJB的理由。一种满足企业应用分布性、扩展性、安全性和交易性要求的、方便使用的框架技术，既是EJB的理想，也是广大程序员的理想。
http://soft.yesky.com/346/3030346_2.shtml

Servlet
---

http://www.oschina.net/question/12_52027 初学 Java Web 开发，请远离各种框架，从 Servlet 开始
https://www.ibm.com/developerworks/cn/java/j-lo-servlet/ IBM写的servlet原理

下面几个知识点可以检测你是否理解了Servlet：
1、什么是ServletContext，和tomcat等web容器的关系时什么？
简单的说，我们在浏览器点击链接和按钮产生的消息不是发送给Servlet的，而是发送给web容器的(在JSP出现之前，web容器也叫Servlet容器)，web容器接收消息后不知道怎么处理，转交给我们编写的Servlet处理，那么web容器怎么和Servlet交流呢？于是就出现了Servlet接口，接口是定义一种规范的良好表达形式。 只要我们编写的Java类符合Servlet规范，那么就能被Web容器识别并被容器管理。

2、什么是Session？Session在实际工程中的应用场景。以及@SessionAttribute注解的局限性。

3、JSP是面向服务器的，它并不知道浏览器是什么鬼，是我们在写JSP时预设客户端是浏览器，JSP就是一个Servlet。JSP的常用对象和指令。

4、JSP的中文编码乱码有几种情况？各自的解决方法？提示： JSP文件的编码，浏览器的解析编码，GET请求的编码，POST的编码。

5、Servlet是一种接口规范，其中请求和响应是Servlet容器通过向方法的参数赋值HttpServletRequest或者HttpServletResponse传递的。在Struts1里面，将doGet()方法里的响应移到返回值里。在Struts2里则:
在Controller中彻底杜绝引入HttpServletRequest或者HttpServletResponse这样的原生Servlet对象。
同时将请求参数和响应数据都从响应方法中剥离到了Controller中的属性变量。

http://www.iteye.com/blogs/subjects/springmvc-explore spring mvc的实现

JSP
---

IOC / DI / AOP

```
IoC(Inversion of Controller) 控制反转(概念)
    DI(Dependency Inject) 依赖注入(IoC概念中的一种类型实现)
        通过依赖声明自动实例化依赖的类(通常通过反射实现)
        Container 容器 
        存储实例化对象 单例的一种实现工具
        ServiceProvider 服务提供者
        一次实例化一批(也可能是一个) 需要使用的类
        并可做一个容器中对象的别名绑定
        Factory 工厂
        一个实例化类的对象 通过上层(框架)实例化
```

编译
---

http://liufor.com/2016/05/30/java-basics-compiler-and-build/ 编辑器介绍

