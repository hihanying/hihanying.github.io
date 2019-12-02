---
layout:     post                   
title:      JavaWeb08-Servlet
subtitle:   1．Servlet体系2．Servlet生命周期3．Servlet的XML配置和注解配置4．ServletConfig&ServletContext5．请求&响应6．HttpServletRequest对象和HttpServletResponse对象的API7．重定向&转发8．中文乱码解决方案9．项目路径问题
date:       2019-11-15
author:     HY
header-img:img/post-bg-2015.jpg
catalog:true
tags:
    - JavaWeb
    - Servlet
    - IDEA
---

# Servlet：  server applet

## 1. Servlet 简介

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。Servlet 有权限访问所有的 Java API，包括访问企业级数据库的 JDBC API。 

Servlet 架构

![](https://raw.githubusercontent.com/hihanying/Figurebed/master/img/tupian1.jpg)

* 概念：运行在服务器端的小程序
    * Servlet就是一个接口，定义了Java类被浏览器访问到(tomcat识别)的规则。
    * 将来我们自定义一个类，实现Servlet接口，复写方法。

## 2. Servlet 生命周期

1. 被创建：执行 `init()` 方法，只执行一次

   * Servlet什么时候被创建？
     * 默认情况下， Servlet 创建于用户第一次调用对应于该 Servlet 的 URL 时
     * 可以通过`web.xml`的`<servlet>`标签配置执行Servlet的创建时机：
       1. 第一次被访问时创建：`<load-on-startup>`的值为负数
       2. 在服务器第一次启动时创建：`<load-on-startup>`的值为0或正整数
   * Servlet的 `init()` 方法，只执行一次，说明一个Servlet在内存中只存在一个对象，Servlet是单例的
     * 多个用户同时访问时，可能存在线程安全问题。
     * 解决：尽量不要在Servlet中定义成员变量。即使定义了成员变量，也不要对修改值

2. 提供服务：执行`service ()` 方法，执行多次

   * 每次访问Servlet时，`service ()` 方法都会被调用一次。

   *  Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端。 

   *  service() 方法由容器调用，只需要根据来自客户端的请求类型来重载 doGet() 或 doPost() 即可。 

     * `doGet()` 方法：GET 请求来自于一个 URL 的正常请求，或者来自于一个未指定 METHOD 的 HTML 表单，它由 `doGet()` 方法处理。
     * `doPost()` 方法：POST 请求来自于一个特别指定了 METHOD 为 POST 的 HTML 表单，它由 `doPost()` 方法处理。

3. 被销毁：执行`destroy()` 方法，只执行一次

   * Servlet被销毁时执行，服务器关闭时，Servlet被销毁
   * 只有服务器正常关闭时，才会执行`destroy()` 方法。
   * `destroy()` 方法在Servlet被销毁之前执行，一般用于释放资源，例如关闭数据库连接、停止后台线程、把 Cookie 列表或点击计数器写入到磁盘等。

## 3. Servlet 快速入门

### 3.1  创建JavaEE项目

1. 选择项目的SDK：jdk-9.0.4
2. 选择Java EE 版本：Java EE 8
3. 选择应用服务器：Tomcat 8.5.31
4. 勾选：Web Application
5. 勾选：Create web.xml

### 3.2 实现Servlet接口

在`src/web`中定义一个类ServletDemo1，实现Servlet接口

```java
package web;

import javax.servlet.*;
import java.io.IOException;

public class ServletDemo1 implements Servlet {
    private String message;
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("init...");
        message = "Hello World";
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("service...");
        servletResponse.getWriter().write(message);
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {
        System.out.println("destroy...");
    }
}
```

### 3.3 配置Servlet

**第1种方法：通过`web.xml`配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--配置Servlet -->
    <servlet>
        <servlet-name>demo1</servlet-name>
        <servlet-class>web.ServletDemo1</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>demo1</servlet-name>
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>
</web-app>
```

> **执行原理：**
>
> 1. 当服务器接受到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
> 2. 查找web.xml文件，根据`<url-pattern>`找到`<servlet-name>`，进而找到对应的`<servlet-class>`全类名
> 4. tomcat会将字节码文件加载进内存，并且创建其对象，调用其方法

**第2种方法：通过注解配置**

Servlet3.0以上版本支持

在实现Servlet接口的类上使用@WebServlet注解

```java
package web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/demo2")
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("ServletDemo2");
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }
}
```

## 4. Servlet 体系结构	

```java
public interface Servlet
public abstract class GenericServlet extends Object implements Servlet, ServletConfig, Serializable
public abstract class HttpServlet extends GenericServlet
```

* Servlet 接口： 定义了3个与生命周期相关的方法，init()，service()，destroy()
* GenericServlet 抽象类：将Servlet接口中其他的方法做了默认空实现，只将service()方法作为抽象，将来定义Servlet类时，可以继承GenericServlet，实现service()方法即可
* HttpServlet 抽象类：对http协议的一种封装，简化操作
	1. 定义类继承HttpServlet
	2. 复写doGet/doPost方法



## 5. Servlet 读取表单数据

### 5.1 浏览器传递信息方法

浏览器使用两种方法可将这些信息传递到 Web 服务器，分别为 GET 方法和 POST 方法。 

#### GET 方法

GET 方法向页面请求发送已编码的用户信息。页面和已编码的信息中间用 ? 字符分隔，如：`http://www.test.com/hello?key1=value1&key2=value2`

- 如果您要向服务器传递的是密码或其他的敏感信息，请不要使用 GET 方法。
- GET 方法有大小限制：请求字符串中最多只能有 1024 个字符。
- 这些信息使用 QUERY_STRING 头传递，并可以通过 QUERY_STRING 环境变量访问，Servlet 使用 **doGet()** 方法处理这种类型的请求。

#### POST 方法

- 向后台程序传递信息的比较可靠的方法是 POST 方法。
- POST 方法把这些信息作为一个单独的消息。
- Servlet 使用 doPost() 方法处理这种类型的请求。



###  5.2 Servlet 读取表单数据

Servlet 处理表单数据，这些数据会根据不同的情况使用不同的方法自动解析：

- **getParameter()：**您可以调用 request.getParameter() 方法来获取表单参数的值。
- **getParameterValues()：**如果参数出现一次以上，则调用该方法，并返回多个值，例如复选框。
- **getParameterNames()：**如果您想要得到当前请求中的所有参数的完整列表，则调用该方法。

**Servlet 读取表单数据实例**

1. 创建 `login.html`
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <form action="/Part00/ServletDemo3" method="get">
            用户名:<input type="text" name="username"> <br>
            密码:<input type="password" name="password"><br>

            <input type="submit" value="登录">

        </form>
    </body>
    </html>
    ```

2. 创建类 ServletDemo 继承 HttpServlet

   ```java
   package web;
   import javax.servlet.ServletException;
   import javax.servlet.annotation.WebServlet;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   
   @WebServlet("/ServletDemo")
   public class ServletDemo extends HttpServlet {
       protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
   		// 从resquest中得到表单数据
           System.out.println("ServletDemo3");
           String username = request.getParameter("username");
           String password = request.getParameter("password");
   		// 将表单内容输出到网页上
           response.setContentType("text/html");
           response.getWriter().write(username+": "+password);
       }
   
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           this.doPost(request, response);
       }
   }
   ```



## 6. HTTP 教程

HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写，是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议。

- HTTP基于TCP/IP通信协议来传递数据（HTML 文件，图片文件，查询结果等）。
- HTTP是基于客户端/服务端（C/S）的架构模型，通过一个可靠的链接来交换信息，是一个无状态（每次请求之间相互独立，不能交互数据）的请求/响应协议。
- HTTP默认端口号为80 。
- HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。

### 6.1 HTTP 消息结构

- 一个HTTP"客户端"是一个应用程序（Web浏览器或其他任何客户端），通过连接到服务器达到向服务器发送一个或多个HTTP的请求的目的。
- 一个HTTP"服务器"同样也是一个应用程序（通常是一个Web服务，如Apache Web服务器或IIS服务器等），通过接收客户端的请求并向客户端发送HTTP响应数据。

**客户端请求消息**

客户端发送HTTP请求到服务器，请求消息包括：请求行（request line）、请求头（header）、空行和请求数据四个部分组成，下图给出了请求报文的一般格式。

![](https://raw.githubusercontent.com/hihanying/Figurebed/master/img/20191127202014.png)

**请求消息实例：**

```
POST /login.html	HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:60.0) Gecko/20100101 Firefox/60.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://localhost/login.html
Connection: keep-alive
Upgrade-Insecure-Requests: 1

username=root
```

**服务器响应消息**

HTTP响应也由四个部分组成，分别是：状态行、响应头、空行和响应正文。

### 6.2 HTTP 请求

**常见的请求头**

| 头信息              | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| Accept              | 这个头信息指定浏览器或其他客户端可以处理的 MIME 类型。值 **image/png** 或 **image/jpeg** 是最常见的两种可能值。 |
| Accept-Charset      | 这个头信息指定浏览器可以用来显示信息的字符集。例如 ISO-8859-1。 |
| Accept-Encoding     | 这个头信息指定浏览器知道如何处理的编码类型。值 **gzip** 或 **compress** 是最常见的两种可能值。 |
| Accept-Language     | 这个头信息指定客户端的首选语言，在这种情况下，Servlet 会产生多种语言的结果。例如，en、en-us、ru 等。 |
| Authorization       | 这个头信息用于客户端在访问受密码保护的网页时识别自己的身份。 |
| Connection          | 这个头信息指示客户端是否可以处理持久 HTTP 连接。持久连接允许客户端或其他浏览器通过单个请求来检索多个文件。值 **Keep-Alive** 意味着使用了持续连接。 |
| Content-Length      | 这个头信息只适用于 POST 请求，并给出 POST 数据的大小（以字节为单位）。 |
| Cookie              | 这个头信息把之前发送到浏览器的 cookies 返回到服务器。        |
| Host                | 这个头信息指定原始的 URL 中的主机和端口。                    |
| If-Modified-Since   | 这个头信息表示只有当页面在指定的日期后已更改时，客户端想要的页面。如果没有新的结果可以使用，服务器会发送一个 304 代码，表示 **Not Modified** 头信息。 |
| If-Unmodified-Since | 这个头信息是 If-Modified-Since 的对立面，它指定只有当文档早于指定日期时，操作才会成功。 |
| Referer             | 这个头信息指示所指向的 Web 页的 URL。例如，如果您在网页 1，点击一个链接到网页 2，当浏览器请求网页 2 时，网页 1 的 URL 就会包含在 Referer 头信息中。作用：防盗链、统计工作 |
| User-Agent          | 这个头信息识别发出请求的浏览器或其他客户端版本信息，并可以向不同类型的浏览器返回不同的内容。 |

**根据HTTP标准，HTTP请求可以使用多种请求方法。**

HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法。

HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。

| 序号 | 方法    | 描述                                                         |
| :--- | :------ | :----------------------------------------------------------- |
| 1    | GET     | 请求指定的页面信息，并返回实体主体。请求参数在请求行中，在 url 后由`？`连接，多个参数使用`&`连接。 |
| 2    | HEAD    | 类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头 |
| 3    | POST    | 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。 |
| 4    | PUT     | 从客户端向服务器传送的数据取代指定的文档的内容。             |
| 5    | DELETE  | 请求服务器删除指定的页面。                                   |
| 6    | CONNECT | HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。     |
| 7    | OPTIONS | 允许客户端查看服务器的性能。                                 |
| 8    | TRACE   | 回显服务器收到的请求，主要用于测试或诊断。                   |

### 6.3 HTTP 响应

HTTP请求头提供了关于请求，响应或者其他的发送实体的信息。

| 应答头           | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| Allow            | 服务器支持哪些请求方法（如GET、POST等）。                    |
| Content-Encoding | 文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压 缩文档能够显著地减少HTML文档的下载时间。Java的GZIPOutputStream可以很方便地进行gzip压缩，但只有Unix上的 Netscape和Windows上的IE 4、IE 5才支持它。因此，Servlet应该通过查看Accept-Encoding头（即request.getHeader("Accept- Encoding")）检查浏览器是否支持gzip，为支持gzip的浏览器返回经gzip压缩的HTML页面，为其他浏览器返回普通页面。 |
| Content-Length   | 表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。如果你想要利用持久连接的优势，可以把输出文档写入 ByteArrayOutputStram，完成后查看其大小，然后把该值放入Content-Length头，最后通过 byteArrayStream.writeTo(response.getOutputStream()发送内容。 |
| Content-Type     | 表示后面的文档属于什么MIME类型。Servlet默认为text/plain，但通常需要显式地指定为text/html。由于经常要设置Content-Type，因此HttpServletResponse提供了一个专用的方法setContentType。 |
| Date             | 当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦。 |
| Expires          | 应该在什么时候认为文档已经过期，从而不再缓存它？             |
| Last-Modified    | 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件 GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置。 |
| Location         | 表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302。 |
| Refresh          | 表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过setHeader("Refresh", "5; URL=http://host/path")让浏览器读取指定的页面。 注 意这种功能通常是通过设置HTML页面HEAD区的＜META HTTP-EQUIV="Refresh" CONTENT="5;URL=http://host/path"＞实现，这是因为，自动刷新或重定向对于那些不能使用CGI或Servlet的 HTML编写者十分重要。但是，对于Servlet来说，直接设置Refresh头更加方便。  注意Refresh的意义是"N秒之后刷 新本页面或访问指定页面"，而不是"每隔N秒刷新本页面或访问指定页面"。因此，连续刷新要求每次都发送一个Refresh头，而发送204状态代码则可 以阻止浏览器继续刷新，不管是使用Refresh头还是＜META HTTP-EQUIV="Refresh" ...＞。  注意Refresh头不属于HTTP 1.1正式规范的一部分，而是一个扩展，但Netscape和IE都支持它。 |
| Server           | 服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置。 |
| Set-Cookie       | 设置和页面关联的Cookie。Servlet不应使用response.setHeader("Set-Cookie", ...)，而是应使用HttpServletResponse提供的专用方法addCookie。参见下文有关Cookie设置的讨论。 |
| WWW-Authenticate | 客户应该在Authorization头中提供什么类型的授权信息？在包含401（Unauthorized）状态行的 应答中这个头是必需的。例如，response.setHeader("WWW-Authenticate", "BASIC realm=＼"executives＼"")。 注意Servlet一般不进行这方面的处理，而是让Web服务器的专门机制来控制受密码保护页面的访问（例如.htaccess）。 |

### 6.4 HTTP 状态码

当浏览器接收并显示网页前，此网页所在的服务器会返回一个包含HTTP状态码的信息头（server header）用以响应浏览器的请求。

**常见的HTTP状态码：**

- 200 - 请求成功
- 301 - 资源（网页等）被永久转移到其它URL
- 404 - 请求的资源（网页等）不存在
- 500 - 内部服务器错误

**HTTP状态码分类**

HTTP状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型，后两个数字没有分类的作用。HTTP状态码共分为5种类型：

| 分类 | 分类描述                                       |
| :--- | :--------------------------------------------- |
| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| 2**  | 成功，操作被成功接收并处理                     |
| 3**  | 重定向，需要进一步的操作以完成请求             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |

##  7. Servlet Request

### 7.1 获取请求行数据

| 方法                         | 描述                                                    |
| ---------------------------- | ------------------------------------------------------- |
| String getMethod()           | 返回请求的 HTTP 方法的名称：GET、POST 或 PUT。          |
| String getContextPath()      | 返回指示请求上下文的请求 URI （虚拟目录）部分。         |
| String getServletPath()      | 返回调用 JSP 的请求的 URL 的一部分（Servlet 路径）。    |
| String getQueryString()      | 返回包含在路径后的请求 URL 中的查询字符串（请求参数）。 |
| String getRequestURI()       | 返回请求的统一资源标识符。                              |
| StringBuffer getRequestURL() | 返回请求的统一资源定位符。                              |
| String getProtocol()         | 返回请求协议的名称和版本。                              |
| String getRemoteAddr()       | 返回发送请求的客户端的互联网协议（IP）地址。            |

### 7.2 获取请求头数据

| 方法                                 | 描述                                           |
| ------------------------------------ | ---------------------------------------------- |
| String getHeader(String name)        | 以字符串形式返回指定的请求头的值。             |
| Enumeration<String> getHeaderNames() | 返回一个枚举，包含在该请求中包含的所有的头名。 |

### 7.3 获取请求体数据

| 方法                                | 描述                                                      |
| ----------------------------------- | --------------------------------------------------------- |
| BufferedReader getReader()          | 获取字符输入流，只能操作字符数据                          |
| ServletInputStream getInputStream() | 使用 ServletInputStream，以二进制数据形式检索请求的主体。 |

### 7.4 获取请求参数通用方式

在获取参数前，设置request的编码`request.setCharacterEncoding("utf-8")`防止出现中文乱码问题。

| 方法                                     | 描述                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| Enumeration getParameterNames()          | 返回一个 String 对象的枚举，包含在该请求中包含的参数的名称。 |
| String[] getParameterValues(String name) | 返回一个字符串对象的数组，包含所有给定的请求参数的值，如果参数不存在则返回 null。 |
| String getParameter(String name)         | 以字符串形式返回请求参数的值，或者如果参数不存在则返回 null。 |
| Map<String,String[]> getParameterMap()   | 返回所有参数的map集合                                        |

### 7.5 请求转发共享数据

```java
request.getRequestDispatcher("/ServletDemo").forward(request,response);
```

特点：

1. 浏览器地址栏路径不发生变化
2. 只能转发到当前服务器内部资源中
3. 转发是一次请求

**共享数据**

| 方法                                      | 描述             |
| ----------------------------------------- | ---------------- |
| void setAttribute(String name,Object obj) | 存储数据         |
| Object getAttitude(String name)           | 通过键获取值     |
| void removeAttribute(String name)         | 通过键移除键值对 |

**案例**

```java
//ServletDemo2
package web;
import ...
    @WebServlet("/ServletDemo2")
    public class ServletDemo2 extends HttpServlet {
        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            System.out.println("ServletDemo2");
            String username = request.getParameter("username");
            String password = request.getParameter("password");
            request.setAttribute("username",username);
            request.setAttribute("password",password);
request.getRequestDispatcher("/ServletDemo3").forward(request,response);
        }
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            this.doPost(request, response);
        }
    }
```



```java
//ServletDemo3
package web;
import ...
@WebServlet("/ServletDemo3")
public class ServletDemo3 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("ServletDemo3");
        String username = (String)request.getAttribute("username");
        String password = (String)request.getAttribute("password");
        response.setContentType("text/html");
        response.getWriter().write(username+": "+password);
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```



## 8. Servlet Response

###  8.1 响应内容格式

响应通常包括一个状态行、一些响应报头、一个空行和文档。 一个典型的响应如下所示：

```
HTTP/1.1 200 OK
Content-Type: text/html
Header2: ...
...
HeaderN: ...
  (Blank Line)
<!doctype ...>
<html>
<head>...</head>
<body>
...
</body>
</html>
```

状态行包括 HTTP 版本（在本例中为 HTTP/1.1）、一个状态码（在本例中为 200）和一个对应于状态码的短消息（在本例中为 OK）。

下表总结了从 Web 服务器端返回到浏览器的最有用的 HTTP 1.1 响应报头：

| 头信息              | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| Allow               | 这个头信息指定服务器支持的请求方法（GET、POST 等）。         |
| Cache-Control       | 这个头信息指定响应文档在何种情况下可以安全地缓存。可能的值有：**public、private** 或 **no-cache** 等。Public 意味着文档是可缓存，Private 意味着文档是单个用户私用文档，且只能存储在私有（非共享）缓存中，no-cache 意味着文档不应被缓存。 |
| Connection          | 这个头信息指示浏览器是否使用持久 HTTP 连接。值 **close** 指示浏览器不使用持久 HTTP 连接，值 **keep-alive** 意味着使用持久连接。 |
| Content-Disposition | 这个头信息可以让您请求浏览器要求用户以给定名称的文件把响应保存到磁盘。 |
| Content-Encoding    | 在传输过程中，这个头信息指定页面的编码方式。                 |
| Content-Language    | 这个头信息表示文档编写所使用的语言。例如，en、en-us、ru 等。 |
| Content-Length      | 这个头信息指示响应中的字节数。只有当浏览器使用持久（keep-alive）HTTP 连接时才需要这些信息。 |
| Content-Type        | 这个头信息提供了响应文档的 MIME（Multipurpose Internet Mail Extension）类型。 |
| Expires             | 这个头信息指定内容过期的时间，在这之后内容不再被缓存。       |
| Last-Modified       | 这个头信息指示文档的最后修改时间。然后，客户端可以缓存文件，并在以后的请求中通过 **If-Modified-Since** 请求头信息提供一个日期。 |
| Location            | 这个头信息应被包含在所有的带有状态码的响应中。在 300s 内，这会通知浏览器文档的地址。浏览器会自动重新连接到这个位置，并获取新的文档。 |
| Refresh             | 这个头信息指定浏览器应该如何尽快请求更新的页面。您可以指定页面刷新的秒数。 |
| Retry-After         | 这个头信息可以与 503（Service Unavailable 服务不可用）响应配合使用，这会告诉客户端多久就可以重复它的请求。 |
| Set-Cookie          | 这个头信息指定一个与页面关联的 cookie。                      |



### 8.2 设置响应方法

| 方法                                      | 描述                                                     |
| ----------------------------------------- | -------------------------------------------------------- |
| void setStatus(int sc)                    | 为该响应设置状态码。                                     |
| void setHeader(String name, String value) | 设置一个带有给定的名称和值的响应报头。                   |
| void sendRedirect(String location)        | 使用指定的重定向位置 URL 发送临时重定向响应到客户端。    |
| void setContentType(String type)          | 如果响应还未被提交，设置被发送到客户端的响应的内容类型。 |

### 8.3 常见应用

**1. 完成重定向**

```java
//1. 设置状态码为302
response.setStatus(302);
//2. 设置响应头location
response.setHeader("location","/day15/responseDemo2");
//3. 简单的重定向方法
response.sendRedirect("/day15/responseDemo2");
```

转发和重定向的区别

| 重定向（redirect）                      | 转发（forward）                         |
| --------------------------------------- | --------------------------------------- |
| 地址栏发生变化                          | 转发地址栏路径不变                      |
| 重定向可以访问其他站点(服务器)的资源    | 转发只能访问当前服务器下的资源          |
| 两次请求。不能使用request对象来共享数据 | 一次请求，可以使用request对象来共享数据 |

**2. 服务器输出字符/字节数据到浏览器**

输出字符

```java
//0.设置编码
response.setContentType("text/html;charset=utf-8");
//1.获取字符输出流
PrintWriter pw = response.getWriter();
//2.输出数据
pw.write("你好啊啊啊 response");
```
输出字节

```java
//0.设置编码
response.setContentType("text/html;charset=utf-8");
//1.获取字节输出流
ServletOutputStream sos = response.getOutputStream();
//2.输出数据
sos.write("你好".getBytes("utf-8"));
```

**3. 验证码**

网页显示


```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script>
            window.onload = function(){
                //1.获取图片对象
                var img = document.getElementById("checkCode");
                //2.绑定单击事件
                img.onclick = function(){
                    //加时间戳
                    var date = new Date().getTime();
                    img.src = "/Part01/CheckCode?"+date;
                }
            }
        </script>
    </head>
    <body>
        <img id="checkCode" src="/day15/CheckCode" />
        <a id="change" href="">看不清换一张？</a>
    </body>
</html>
```

服务器servlet

```java
package web.servlet;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

@WebServlet("/CheckCode")
public class CheckCode extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("CheckCode");

        int width = 100;
        int height = 30;
        int codeNums = 4;
        int lineNums = 10;
        //1. 创建一对象，在内存中图片(验证码图片对象)
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        //2. 画背景边框
        Graphics graphics = image.getGraphics();
        graphics.setColor(Color.PINK);
        graphics.fillRect(0, 0, width, height);
        graphics.setColor(Color.blue);
        graphics.drawRect(0, 0, width-1, height-1);
        //3. 写验证码
        String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghigklmnopqrstuvwxyz0123456789";
        Random random = new Random();
        for (int i = 0; i < codeNums; i++) {
            int index = random.nextInt(str.length());
            graphics.drawString(str.charAt(index)+"",width/5*(i+1)-5, height/2+5);
        }
        //4. 画干扰线
        graphics.setColor(Color.yellow);
        for (int i = 0; i < lineNums; i++) {
            graphics.drawLine(  random.nextInt(width), random.nextInt(height),
                                random.nextInt(width), random.nextInt(height));
        }
        //5. 输出图形
        ImageIO.write(image, "jpg",response.getOutputStream());
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```





