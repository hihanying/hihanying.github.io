---
layout:     post                   
title:      JavaWeb07-Tomcat
subtitle:   1．HTTP协议2．Tomcat服务器搭建3．Tomcat目录结构解析4．Tomcat端口配置5．Tomcat启动&停止6．Tomcat&IDEA整合7．IDEA配置优化
date:       2019-11-15
author:     HY
header-img:img/post-bg-2015.jpg
catalog:true
tags:
    - JavaWeb
    - Tomcat
    - IDEA

---



[Toc]

# web相关

## 1. 软件架构
- C/S：客户端/服务器端
- B/S：浏览器/服务器端

## 2. 资源分类
- 静态资源：一般客户端发送请求到web服务器，web服务器从内存在取到相应的文件，返回给客户端，客户端解析并渲染显示出来。如： html,css,JavaScript
- 动态资源：一般客户端请求的动态资源，先将请求交于web容器，web容器连接数据库，数据库处理数据之后，将内容交给web服务器，web服务器返回给客户端解析渲染处理。如：servlet/jsp,php,asp....

## 3. 网络通信三要素
- IP地址：电子设备(计算机)在网络中的唯一标识。
	-  特殊的IP地址：127.0.0.1 本机IP地址 
- 端口：应用程序在计算机中的唯一标识。
    - 有效端口：0~65536。其中1-1024是系统保留的端口
    - 常见的端口：http:80、https:443、ftp:21，mysql: 3306、nginx: 80、tomcat: 8080
- 传输协议：规定了数据传输的规则
    - TCP（ 传输控制协议 ）：面向连接、可靠协议、三次握手、传输速度慢
    - UDP（ 用户数据报协议 ）：面向无连接、不可靠协议、面向报文、传输速度快
- 通讯网络步骤：确定对端IP地址，确定应用程序端口，确定通信协议

## 4. web服务器软件
* 服务器：安装了服务器软件的计算机， 在网络中为其它客户机提供计算或者应用服务。 
* 服务器软件：接收用户的请求，处理请求，做出响应
* web服务器软件：处理从客户端发出的请求，做出响应，如JAVA中的Tomcat容器 
	* 在web服务器软件中，可以部署web项目，让用户通过浏览器来访问这些项目

## 5. 常见的java相关的web服务器软件
* webLogic：oracle公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的。
* webSphere：IBM公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的。
* JBOSS：JBOSS公司的，大型的JavaEE服务器，支持所有的JavaEE规范，收费的。
* Tomcat：Apache基金组织，中小型的JavaEE服务器，仅仅支持少量的JavaEE规范servlet/jsp。开源的，免费的。

## 6. Java平台 
-  **Java SE** 是 jdk jvm 以及自带的api合集的具体实现，提供Java编程语言的核心功能。 
-  **Java EE** 是构建于Java SE平台之上，提供一组API和运行环境来开发和运行大规模的，多层的，可扩展的，可靠的和安全的**网络**应用程序，一共规定了13项大的规范。
-  **Java ME** 是一套运行专门为**嵌入式**设备设计的API接口规范。比如机顶盒这种程序。


# Tomcat

Tomcat 是由 Apache 软件基金会下属的 Jakarta 项目开发的一个 **Servlet 容器**，按照 Sun Microsystems 提供的技术规范开发出来，Tomcat 8 实现了对 Servlet 3.1 和 JavaServer Page 2.3（JSP）的支持，并提供了作为 Web 服务器的一些特有功能，如 Tomcat 管理和控制平台、安全域管理和 Tomcat 附加组件等。 

使用servlet和JSP页面来构建Web应用程序，并选择Tomcat服务器进行学习和开发。 

## 1. Tomcat 安装

### 1.1 基本使用

1. 下载：[Apache Tomcat®](http://tomcat.apache.org/)
   - 左侧Download列表下选择相应的版本
   - 依次选择：版本号→Binary Distributions→ Core →64-bit Windows zip (pgp, sha512)
2. 安装：解压压缩包即可
   
    * 注意：安装目录建议不要有中文和空格
3. 卸载：删除目录即可
4. 启动：
    * bin/startup.bat ,双击运行该文件即可
    * 访问：浏览器输入：http://localhost:8080 回车访问自己
    * 可能遇到的问题：
         * 黑窗口一闪而过：
           * 原因： 没有正确配置JAVA_HOME环境变量
             * 解决：正确配置JAVA_HOME环境变量
		* 启动报错：
             1. 通过命令行`netstat -ano`找到占用的端口号，并且找到对应的进程，杀死该进程
             2. 找到`conf/server.xml`文件，修改自身的端口号
                 ```xml
                 <Connector port="8888" protocol="HTTP/1.1"
                            connectionTimeout="20000"
                            redirectPort="8445" />
                 ```
                 > 一般会将tomcat的默认端口号修改为80。80端口号是http协议的默认端口号。好处：在访问时，就不用输入端口号。
5. 关闭
    1. 正常关闭：
        * bin/shutdown.bat
        * ctrl+c
    2. 强制关闭：
        * 点击启动窗口的×

### 1.2 目录结构

- **/bin** - Tomcat 脚本（可执行文件）存放目录（如启动、关闭脚本）。
    -  `*.sh` 文件用于 Unix 系统； 
    - `*.bat` 文件用于 Windows 系统。
- **/lib** - Tomcat 依赖`jar`包目录。
- **/conf** - Tomcat 配置文件目录。
- **/logs** - Tomcat 默认日志目录。
- **/webapps** - webapp 运行的目录。
- **/work** - 存放运行时的数据。

## 2. 部署项目的方式

### 2.1  Context描述文件部署

在`conf\Catalina\localhost`创建任意名称的xml文件。在文件中编写

```xml
<Context path="项目的访问地址，可以不配置" docBase="项目的存储地址根目录" ></Context>
```

通过Context文件描述清楚项目的访问路径和地址，tomcat在启动的时候会解析这个Context文件，创建一个Context对象。

- Context文件的存储路径默认路径（通过server文件配置）为：`tomcat/conf/<Engine name属性名称>/<Host name属性名称>` 。由于tomcat的Engine和Host都有默认的名称，所以默认的存储路径为：`tomcat/conf/Catalina/localhost/*.xml`（xml文件可以随意命名）
- 如果想将Context描述文件放到其它位置而非默认位置，那么可以通过`server.xml`中的Host标签的xmlBase属性指定 ，例如
    ```xml
    <Host xmlBase="/home/app/context" name="localhost"  appBase="webapps"
          unpackWARs="true" autoDeploy="true">
    ```

### 2.2 Web目录部署

直接将项目放到 Host标签指定的appBase（webapps） 目录下即可。

* 项目的访问路径：虚拟目录
* 简化部署：将项目打成一个war包，再将war包放置到webapps目录下，war包会自动解压缩

### 2.3 配置`conf/server.xml`文件
在<Host>标签体中配置

```xml
<Context docBase="D:\hello" path="/hehe" />
```

## 3. IDEA中配置Tomcat

1. 点击菜单栏 Run，选择“Edit Configurations... ”
2. 点击左侧“+”号，选择“Tomcat Server”，选择“Local”（没有找到可以点击最后一行 x items more） 
3. 点击 Server -> Application server -> Configuration，找到本地 Tomcat 服务器，再点击 OK 。 
4. 进入 Deployment 选项卡，点击右侧“+”号“artifact...”添加项目，在下侧“Application context”中设置访问路径

**工作空间项目和Tomcat部署的web项目**

* tomcat真正访问的是“tomcat部署的web项目”，"tomcat部署的web项目"对应着"工作空间项目" 的web目录下的所有资源
* WEB-INF目录下的资源不能被浏览器直接访问。



