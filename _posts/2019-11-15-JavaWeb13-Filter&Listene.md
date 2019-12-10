---
layout:     post                   
title:      JavaWeb13-Filter&Listener
subtitle:   1．Filter原理及配置2．Filter生命周期3．Filter链4．Filter登录验证5．Listener原理6．WEB中八大监听器的介绍7．ServletContextListener的应用场景
date:       2019-11-07
author:     HY
header-img:img/post-bg-2015.jpg
catalog:true
tags:
    - JavaWeb
    - Filter
    - Listener
---

# Filter

## 1. Filter 简述

`Filter `是 `javax.servlet` 包下的 `Interface` ，它对资源（Servlet或静态内容）的请求或响应执行过滤任务。

**解决什么样的问题**

在客户端的 Request 到服务器之前进行拦截，检查请求信息，以做出允许访问/修改请求信息后访问/阻止访问及转发的决定。

**使用**

* 定义一个类，实现接口 Filter，导入的是javax.servlet包
* 配置拦截路径的两种方式：web.xml和注解
* 复写方法：init、doFilter、destroy

**案例**

```java
import javax.servlet.*;//要导入的包
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

@WebFilter("/*")	//此处可以配置拦截路径
public class FilterDemo implements Filter {
    public void destroy() {
    }
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        chain.doFilter(req, resp);	// 放行指令 
    }
    public void init(FilterConfig config) throws ServletException {
    }
}
```

## 2. Filter 配置

**拦截路径配置**

* 具体资源路径：`/index.jsp` 
     * 只有访问index.jsp资源时，过滤器才会被执行
* 拦截目录：`/user/*`
     * 访问/user下的所有资源时，过滤器都会被执行
* 后缀名拦截：`*.jsp`
     * 访问所有后缀名为jsp资源时，过滤器都会被执行
* 拦截所有资源：`/`
     * 访问所有资源时，过滤器都会被执行

**设置dispatcherTypes属性**

* REQUEST：默认值。浏览器直接请求资源
* FORWARD：转发访问资源
*  INCLUDE：包含访问资源
* ERROR：错误跳转资源
* ASYNC：异步访问资源

**web.xml 配置**

```xml
<filter>
    <filter-name>demo</filter-name>
    <filter-class>web.filter.FilterDemo</filter-class>
</filter>
<filter-mapping>
    <filter-name>demo</filter-name>
    <url-pattern>/*</url-pattern><!-- 拦截路径 -->
    <dispatcher>REQUEST</dispatcher><!-- dispatcherTypes属性 -->
</filter-mapping>
```
**注解配置**

```java
@WebFilter(value = "/index.jsp", dispatcherTypes = DispatcherType.FORWARD)
@WebFilter(value = "/index.jsp", dispatcherTypes = DispatcherType.REQUEST)
@WebFilter(value = "/index.jsp", dispatcherTypes = {DispatcherType.FORWARD, DispatcherType.REQUEST})
```



## 3. Filter 执行

**过滤器设计执行流程**

1. 客户端的 Request 访问服务器
2. 过滤器对 Request 进行拦截，执行过滤器处理
3. 过滤器处理完后放行，请求到达Servlet/JSP，Servlet处理
4. Servlet处理完后，再回到过滤器, 最后在由tomcat服务器相应用户

**过滤器相关 API** 

* public interface FilterConfig：获取初始化参数信息
  * `getInitParameter(java.lang.String.name)`  该方法获取指定名称的值
  * `getInitParameterNames()`  用获取所有参数的名称，放进集合Enumeration
* public interface FilterChain：过滤器调用链，用以调用下一个过滤器或放行
  * `void doFilter(ServletRequest request, ServletResponse response);` 执行下一个过滤器或放行

**过滤器生命周期方法**

```java
public void init(FilterConfig filterConfig) throws ServletException
```

在服务器启动后，会创建 Filter 对象，然后仅调用一次 init 方法，用于加载资源。

```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws java.io.IOException, ServletException
```

每当请求/响应经过 Filter 时都会调用 Filter 的 doFilter 方法。执行多次。

```java
public void destroy()
```

如果服务器是正常关闭，则会执行destroy方法。只执行一次。用于完成诸如关闭过滤器使用的文件或数据库连接池等清除任务来释放资源。

**过滤器链**

* 如果有两个过滤器，过滤器1和过滤器2，执行顺序：过滤器1 → 过滤器2 → 资源执行 → 过滤器2 → 过滤器1 
  
* 过滤器先后顺序问题：
	1. 注解配置：按照类名的字符串比较规则比较，值小的先执行。如： AFilter 和 BFilter，AFilter先执行。
	2. 按照 web.xml 配置执行： `<filter-mapping>`中定义在前面的过滤器先执行

**案例**：对所有资源的访问进行拦截，如果没有登陆，则跳转至登陆页面

```java
@WebFilter("/*")
public class LoginFilter implements Filter {
    public void destroy() {
    }
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        // 1. 获取资源的请求路径
        HttpServletRequest request = (HttpServletRequest) req;
        String requestURI = request.getRequestURI();
        // 2. 登陆页面请求的必须资源放行
        if (   requestURI.contains("/login.jsp") 
            || requestURI.contains("/loginServlet")
            || requestURI.contains("/css/")
            || requestURI.contains("/fonts/")
            || requestURI.contains("/checkCodeServlet")
            || requestURI.contains("/js/")) {
            chain.doFilter(req, resp);
        } else { //3. 其他资源放行与否由用户是否已经登陆决定
            Object user = request.getSession().getAttribute("user");
            if (user != null) {//“user”是否为空可以作为用户登陆与否的判断
                chain.doFilter(req, resp);
            } else {
                request.setAttribute("login_msg", "您尚未登陆，请先登录");
                request.getRequestDispatcher("/login.jsp").forward(request,resp);
            }
        }
    }
    public void init(FilterConfig config) throws ServletException {
    }
}
```

# Listener

## 1. Listener 简述

**Listener 用来解决什么样的问题？**

Listener 可以用来检测网站的在线人数，统计网站的访问量等！

**什么是 Listener ？**

Listener 就是一个实现特定接口的普通java程序，这个程序专门用于监听另一个java对象的方法调用或属性改变，当被监听对象发生上述事件后，监听器某个方法将立即被执行。

**Listener 怎么工作的？**

监听器涉及三个组件：事件源，事件对象，事件监听器

![](https://raw.githubusercontent.com/hihanying/Figurebed/master/img/20191207164927.png)

当`事件源`发生某个动作的时候，它会调用`事件监听器`的方法，并在调用事件监听器方法的时候把`事件对象`传递进去。我们在`监听器`中就可以通过事件对象获取得到`事件源`，从而对`事件源`进行操作！

## 2. Servlet Listener

在Servlet规范中定义了多种类型的监听器，它们用于监听的事件源分别 `ServletContext`，`HttpSession`和`ServletRequest`这三个域对象。

Servlet规范针对这三个对象上的操作，又把多种类型的监听器划分为三种类型：

* 监听域对象自身的创建和销毁的事件监听器。
* 监听域对象中的属性的增加和删除的事件监听器。
* 监听绑定到HttpSession域中的某个对象的状态的事件监听器。

监听器配置方法

1. web.xml

   ```xml
   <listener>
       <listener-class>web.Listener</listener-class>
   </listener>
   ```

   * 指定初始化参数`<context-param>`

2. 注解：@WebListener


### 示例：统计网站在线人数

> **原理：**每当有一个访问连接到服务器时，服务器就会创建一个session来管理会话。那么我们就可以通过统计session的数量来获得当前在线人数。所以这里用到的是HttpSessionListener。

1. 创建监听器类，实现 HttpSessionListener 接口，并重写监听器类中的方法。代码如下：
    ```java
    @WebListener()
    public class Listener implements HttpSessionListener {
        public int sessionCount = 0;
        // HttpSessionListener implementation
        public void sessionCreated(HttpSessionEvent se) {
            sessionCount++;
            se.getSession().getServletContext().setAttribute("sessionCount", sessionCount);
        }
        public void sessionDestroyed(HttpSessionEvent se) {
            sessionCount--;
            se.getSession().getServletContext().setAttribute("sessionCount", sessionCount);
        }
    }
    ```

2. 在`login.jsp`中的<body>标签中加入如下代码：

   ```html
   <div>当前共有 ${sessionCount} 人同时在线</div>
   ```

有Bug，Session的默认失效时间是30分钟，所以30分钟内登陆过的都会被算进去
