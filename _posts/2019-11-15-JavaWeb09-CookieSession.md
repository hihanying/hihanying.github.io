---
layout:     post                   
title:      JavaWeb09-CookieSession
subtitle:   1．Cookie机制2．Cookie创建&使用3．Session原理4．Session的获取及使用5．Session失效6．Session作为域对象的API7．Session活化&钝化
date:       2019-11-15
author:     HY
header-img:img/post-bg-2015.jpg
catalog:true
tags:
    - JavaWeb
    - Cookie
    - Session
---

# Servlet Cookies 处理

Cookies 是存储在客户端计算机上的文本文件，并保留了各种跟踪信息。 



## 1. Cookies 剖析



**Cookies 使用包括三个步骤：**

- 服务器脚本向浏览器发送一组 Cookies。例如：姓名、年龄或识别号码等。

  Cookies 通常设置在 HTTP 头信息中。设置 Cookie 的 Servlet 会发送如下的头信息：

  ```shell
  HTTP/1.1 200 OK
  Date: Fri, 04 Feb 2000 21:03:38 GMT
  Server: Apache/1.3.9 (UNIX) PHP/4.0b3
  Set-Cookie: name=xyz; expires=Friday, 04-Feb-07 22:03:38 GMT; 
                   path=/; domain=w3cschool.cn
  Connection: close
  Content-Type: text/html
  ```

  Set-Cookie 头包含了一个名称值对、一个 GMT 日期、一个路径和一个域。名称和值会被 URL 编码。

  

- 浏览器将这些信息存储在本地计算机上，以备将来使用。

  - expires 字段是一个指令，告诉浏览器在给定的时间和日期之后"忘记"该 Cookie。
  - 如果浏览器被配置为存储 Cookies，它将会保留此信息直到到期日期。

- 当下一次浏览器向 Web 服务器发送任何请求（用户的浏览器指向任何匹配该 Cookie 的路径和域的页面）时，浏览器会把这些 Cookies 信息发送到服务器，服务器将使用这些信息来识别用户。

  ```shell
  GET / HTTP/1.0
  Connection: Keep-Alive
  User-Agent: Mozilla/4.6 (X11; I; Linux 2.2.6-15apmac ppc)
  Host: zink.demon.co.uk:1126
  Accept: image/gif, */*
  Accept-Encoding: gzip
  Accept-Language: en
  Accept-Charset: iso-8859-1,*,utf-8
  Cookie: name=xyz
  ```

  Servlet 就能够通过请求方法 `request.getCookies()`访问 Cookie，该方法将返回一个 Cookie 对象的数组。

## 2. Cookies 方法

以下是在 Servlet 中操作 Cookies 时可使用的有用的方法列表。

| 方法                                   | 描述                                                         |
| :------------------------------------- | :----------------------------------------------------------- |
| public void setDomain(String pattern)  | 该方法设置 cookie 适用的域，例如 w3cschool.cn。              |
| public String getDomain()              | 该方法获取 cookie 适用的域，例如 w3cschool.cn。              |
| public void setMaxAge(int expiry)      | 该方法设置 cookie 过期的时间（以秒为单位）。如果不这样设置，cookie 只会在当前 session 会话中持续有效。 |
| public int getMaxAge()                 | 该方法返回 cookie 的最大生存周期（以秒为单位），默认情况下，-1 表示 cookie 将持续下去，直到浏览器关闭。 |
| public String getName()                | 该方法返回 cookie 的名称。名称在创建后不能改变。             |
| public void setValue(String newValue)  | 该方法设置与 cookie 关联的值。                               |
| public String getValue()               | 该方法获取与 cookie 关联的值。                               |
| public void setPath(String uri)        | 该方法设置 cookie 适用的路径。如果您不指定路径，与当前页面相同目录下的（包括子目录下的）所有 URL 都会返回 cookie。 |
| public String getPath()                | 该方法获取 cookie 适用的路径。                               |
| public void setSecure(boolean flag)    | 该方法设置布尔值，表示 cookie 是否应该只在加密的（即 SSL）连接上发送。 |
| public void setComment(String purpose) | 该方法规定了描述 cookie 目的的注释。该注释在浏览器向用户呈现 cookie 时非常有用。 |
| public String getComment()             | 该方法返回了描述 cookie 目的的注释，如果 cookie 没有注释则返回 null。 |



## 3. 通过 Servlet 设置 Cookies

通过 Servlet 设置 Cookies 包括三个步骤：

**(1) 创建一个 Cookie 对象：**调用带有 cookie 名称和 cookie 值的 Cookie 构造函数，cookie 名称和 cookie 值都是字符串。

```java
Cookie cookie = new Cookie("key","value");
```

请记住，无论是名字还是值，都不应该包含空格或以下任何字符：

```java
[ ] ( ) = , " / ? @ : ;
```

**(2) 设置最大生存周期：**使用 setMaxAge 方法来指定 cookie 能够保持有效的时间（以秒为单位）。下面将设置一个最长有效期为 24 小时的 cookie。

```java
cookie.setMaxAge(60*60*24); 
```

**(3) 发送 Cookie 到 HTTP 响应头：**使用 **response.addCookie** 来添加 HTTP 响应头中的 Cookies，如下所示：

```java
response.addCookie(cookie);
```

## 4. 通过 Servlet 读取 Cookies

要读取 Cookies，您需要通过调用 HttpServletRequest 的 getCookies( ) 方法创建一个 javax.servlet.http.Cookie 对象的数组。然后循环遍历数组，并使用 getName() 和 getValue() 方法来访问每个 cookie 和关联的值。

```java
Cookie[] cs = request.getCookies();
//获取数据，遍历Cookies
if(cs != null){
    for (Cookie c : cs) {
        String name = c.getName();
        String value = c.getValue();
        System.out.println(name+":"+value);
    }
}
```

## 5. 通过 Servlet 删除 Cookies

删除 Cookies 是非常简单的。如果想删除一个 cookie，只需要按照以下三个步骤进行：

- 读取一个现有的 cookie，并把它存储在 Cookie 对象中。
- 使用 **setMaxAge()** 方法设置 cookie 的年龄为零，来删除现有的 cookie。
- 把这个 cookie 添加到响应头。



## 6. Cookies 细节

**一次可不可以发送多个cookie?**

可以创建多个Cookie对象，使用response调用多次addCookie方法发送cookie即可。

```java
//1.创建Cookie对象
Cookie c1 = new Cookie("msg","hello");
Cookie c2 = new Cookie("name","zhangsan");
//2.发送Cookie
response.addCookie(c1);
response.addCookie(c2);
```
**cookie在浏览器中保存多长时间？**

- 默认情况下，当浏览器关闭后，Cookie数据被销毁
- 持久化存储：`setMaxAge(int seconds)`
  - 正数：将Cookie数据写到硬盘的文件中，持久化存储。参数指定cookie存活时间，单位是秒。
  - 负数：默认值
  - 零：删除cookie信息

**cookie能不能存中文？**

* 在 tomcat 8 之前 cookie中不能直接存储中文数据，需要将中文数据转码---一般采用URL编码(%E3)，方法如下：
	
	```java
	String str = java.net.URLEncoder.encode("中文");//编码
	String str = java.net.URLDecoder.decode("编码后的字符串");// 解码
	```
* 在tomcat 8 之后，cookie支持中文数据。特殊字符还是不支持，建议使用URL编码存储，URL解码解析

**cookie共享问题？**

1. 假设在一个tomcat服务器中，部署了多个web项目，那么在这些web项目中cookie能不能共享？
	* 默认情况下cookie不能共享

	* `setPath(String path)`：设置cookie的获取范围。默认情况下，设置当前的虚拟目录
		* 如果要共享，则可以将path设置为"/"


2. 不同的tomcat服务器间cookie共享问题？
	
	`setDomain(String path)`：如果设置一级域名相同，那么多个服务器之间cookie可以共享，例如`setDomain(".baidu.com")`，那么tieba.baidu.com和news.baidu.com中cookie可以共享



## 7. Cookies 特点和作用

**特点**

  1. cookie存储数据在客户端浏览器
  2. 浏览器对于单个cookie 的大小有限制(4kb) 以及 对同一个域名下的总cookie数量也有限制(20个)

**作用**

1. cookie一般用于存出少量的不太敏感的数据
2. 在不登录的情况下，完成服务器对客户端的身份识别





