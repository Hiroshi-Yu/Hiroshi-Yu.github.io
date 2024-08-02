SSL 安装
===

官方已将SSL移除核心代码，服务器实施SSL，使用nginx更方便，主要是APLN的版本太难维护了。


https://www.liaoxuefeng.com/article/0014189023237367e8d42829de24b6eaf893ca47df4fb5e000 必看

https://www.cnblogs.com/youxia/p/java002.html keytool 详解keytool，精华在末尾

http://blog.csdn.net/skykingf/article/details/51305300  细说 CA 和证书，关于各个证书解释很清楚

http://blog.csdn.net/fengwind1/article/details/52191520 keytool制作CA根证书以及颁发二级证书

https://serverfault.com/questions/488003/keytool-serveralternatename keytool做SAN

https://www.cnblogs.com/ederwin/articles/5553550.html mac的证书操作，也很详细，需要openssl基础

http://wiki.idempiere.org/en/Install_ssl_certificate id申请商业证书，不是很详细，但是很清晰

操作步骤
---

iDempiere安装的时候，会创建一个keystore文件，位于$iDempiereHome$\jettyhome\etc\keystore。
该keystore 包含一个 keypair，如下图：
1. 私钥key：与keypair密码一样，都是myPassword
2. 公钥证书：包含了刚才输入的 CN 通用名，组织，国家 等信息。其中CN 是你的域名或者ip地址，表示使用者的名字。

如果证书存在“多名称”用途，比如：主机名字，ip或者其他标识符，那这个证书还需要“多CN”方案。
微软使用多CN名称来完成，比如CN=1,CN=2,CN=3,OU.......
也就是 x509证书v3 的拓展SAN" Subject Alternative Names"来输入更多“使用者”或者“IP”地址，如下图。 
参考：https://en.wikipedia.org/wiki/Subject_Alternative_Name


其中 Chrome 也不嫌乱，升级到58之后，SSL的证书检查必须使用 SAN ，证书使用多逗号的CN将无法信任。
参考：https://blogs.sap.com/2017/05/03/fix-chrome-missing_subjectaltname-error-with-sap-netweaver-application-server-abap/

注意，keystore的keypair的Alia name=idempiere，替换掉原keypair，名字也要保持一致，以供签名jar文件使用。当然，新建一个mykey或者新做一个keystore，供网站SSL认证，那就随便命名，jetty的SSL配置文件按照新名字来。

![](https://static.oschina.net/uploads/space/2018/0422/212922_ES74_2720480.png)

![](https://static.oschina.net/uploads/space/2018/0422/212336_6z4D_2720480.png)

![](https://static.oschina.net/uploads/space/2018/0422/212348_IwGE_2720480.png)

![](https://static.oschina.net/uploads/space/2018/0422/212410_UDUK_2720480.png)

![](https://static.oschina.net/uploads/space/2018/0422/212403_ufj0_2720480.png)

![](https://static.oschina.net/uploads/space/2018/0422/212424_Ufr6_2720480.png)

![](https://static.oschina.net/uploads/space/2018/0422/212416_pCLT_2720480.png)

![](https://static.oschina.net/uploads/space/2018/0422/213459_vcaQ_2720480.jpg)

![SSL部署自签名证书](https://static.oschina.net/uploads/space/2017/1210/222543_6Zx2_2720480.png)

关键字(待续)
---

CA
root key
intermediate CAs：与 root cert 和 intermediate cert 组成证书链

安装CA到系统：双击 /root/ca/intermediate/certs/ca-chain.cert.pem 将证书安装到系统中，目的是让本机信任这个 CA，将其当作一个权威 CA，安装 root pem 或者 intermediate chain pem 都是可以的，它们都具备验证能力。如果不执行这一步，浏览器依然会提示net:ERR_CERT_AUTHORITY_INVALID

![](https://static.oschina.net/uploads/space/2017/1210/190425_TmgG_2720480.png)

认证方式
---

SSL使用证书来创建安全连接，一般有两种验证方式。
第一种是客户端认证服务器端的提供的证书，一般的HTTPS网站都采用这种方式。
第二种是客户端和服务器端都要认证对方的证书，一般是APP应用和它的服务器可能会采用这样的方式。

证书（无客户端服务端之分）保存着ip信息、证书过期时间、证书所有者地址信息等。

单向认证
1、客户端保存着服务端的证书并信任该证书即可
2、https一般是单向认证，这样可以让绝大部分人都可以访问你的站点。

双向认证
1、先决条件是有两个或两个以上的证书，一个是服务端证书，另一个或多个是客户端证书。
2、服务端保存着客户端的证书并信任该证书，客户端保存着服务端的证书并信任该证书。这样，在证书验证成功的情况下即可完成请求响应。
3、双向认证一般企业应用对接。

jetty中，keystoreFile, 服务端证书位置，truststoreFile，信任的客户端证书位置，如下图。
默认idempiere jetty使用单向认证，访问服务器地址会自动下载证书，表示“我是某个网站”，但该证书不是系统承认的信任根证书，浏览器肯定会报错。只有你安装该证书到信任根目录才不报错。

![jetty-ssl-context.xml的配置](https://static.oschina.net/uploads/space/2017/1210/192226_HZ52_2720480.png)

自签名证书
---

系统证书维护：certmgr.msc

自签名证书，需要将证书添加到操作系统的证书管理器（certmgr.msc）的信任跟目录下。
如果不添加，所有浏览器都不信任，但是还是加密访问，chrome可F12查看security显示。

firefox使用about:config>security.enterprise_roots.enabled表示使用系统证书管理。

https://wenku.baidu.com/view/a00e65cc55270722192ef7e2.html chrome的证书透明政策

https://stackoverflow.com/questions/7580508/getting-chrome-to-accept-self-signed-localhost-certificate

https://www.cnblogs.com/awpatp/p/6478460.html 忽略chrome的证书报错

http://blog.csdn.net/YQXLLWY/article/details/52399935 就是common name的问题

ALPN问题
---

window7 64位
4.1.0.v20170505-0431
问题：ssl 8443虽然启动成功，但是浏览器说ssl版本不对，报错
ERR_SSL_VERSION_OR_CIPHER_MISMATCH

服务器启动缺省参数 :
-Djetty.home=jettyhome 
-Djetty.etc.config.urls=etc/jetty.xml,etc/jetty-selector.xml,etc/jetty-ssl.xml,etc/jetty-https.xml,etc/jetty-deployer.xml

改为:
-Xbootclasspath/p:alpn-boot.jar
-Dorg.osgi.framework.bootdelegation=sun.security.ssl,org.eclipse.jetty.alpn 
-Djetty.home=jettyhome 
-Djetty.etc.config.urls=etc/jetty.xml,etc/jetty-deployer.xml,etc/jetty-ssl.xml,etc/jetty-ssl-context.xml,etc/jetty-http.xml,etc/jetty-alpn.xml,etc/jetty-http2.xml,etc/jetty-https.xml

如果ssl alpn没有启动
五月 10, 2017 12:04:07 上午 org.eclipse.jetty.server.AbstractConnector doStart
信息: Started ServerConnector@e288054{HTTP/1.1,[http/1.1]}{localhost:8080}  【http启动成功】
五月 10, 2017 12:04:07 上午 org.eclipse.jetty.server.AbstractConnector doStart
信息: Started ServerConnector@3afbff94{SSL,[ssl, http/1.1]}{localhost:8443}   【ssl启动，但没有alpn握手】
ssl alpn正确启动
May 09, 2017 11:46:56 PM org.eclipse.jetty.server.AbstractConnector doStart
INFO: Started ServerConnector@7a879be0{HTTP/1.1,[http/1.1, h2c, h2c-17, h2c-16, h2c-15, h2c-14]}{Nirranjan.local:8090}       【http启动成功】
May 09, 2017 11:46:56 PM org.eclipse.jetty.server.AbstractConnector doStart
INFO: Started ServerConnector@5ab23b1e{SSL,[ssl, alpn, h2, h2-17, h2-16, h2-15, h2-14, http/1.1]}{Nirranjan.local:4443}        【ssl，alpn启动成功】
浏览器【F12-安全】显示
The connection to this site is encrypted and authenticated using a strong protocol (TLS 1.2), a strong key exchange (ECDHE_RSA with P-256), and a strong cipher (AES_256_GCM).

安装证书
---

SSL证书

系统setup安装的时候，Java内置的证书工具 %JAVA_HOME%/bin/keytool.exe 会提示输入证书一些信息，默认后idempiere生成的keystore文件，默认证书别名：idempiere，默认证书库密码：myPassword。
安装最后，idempiere会拷贝文件到%IDEMPIERE_HOME%\jettyhome\etc内供jetty使用。

参考：http://blog.csdn.net/tony1130/article/details/5134318 （大神写得真详细）

加密某个网站
---

去掉url-pattern，就是全部ssl

%id_home%\plugins\org.adempiere.webstore_4.*\WEB-INF\web.xml

 <security-constraint>
    <web-resource-collection>
      <web-resource-name>SSL pages</web-resource-name>     【页面pattern】
      <url-pattern>/login.jsp</url-pattern>
      <url-pattern>/loginServlet</url-pattern>
      <url-pattern>/checkOutServlet</url-pattern>
      <url-pattern>/orderServlet</url-pattern>
    </web-resource-collection>

<user-data-constraint>
  <transport-guarantee>CONFIDENTIAL</transport-guarantee>  【SSL设置】
</user-data-constraint>
  </security-constraint> 

