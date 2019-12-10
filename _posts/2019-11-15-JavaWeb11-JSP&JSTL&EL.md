---
layout:     post                   
title:      JavaWeb10-JSP
subtitle:   JSP、EL、JSTL
date:       2019-11-15
author:     HY
header-img:img/post-bg-2015.jpg
catalog:true
tags:
    - JavaWeb
    - JSP
    - EL
    - JSTL
---





# JSP 基础



## 1. JSP 简介

JSP全称 Java Server Pages，是一种动态网页开发技术。它使用JSP标签在HTML网页中插入Java代码，主要用于实现Java web应用程序的用户界面部分。JSP本质上就是一个Servlet。

**servlet和jsp的区别**

1. ervlet在Java代码中可以通过HttpServletResponse对象动态输出HTML内容。

2. JSP是在静态HTML内容中嵌入Java代码，然后Java代码在被动态执行后生成HTML内容。

**servlet和jsp各自的特点**

1. Servlet虽然能够很好地组织业务逻辑代码，但是在Java源文件中，因为是通过字符串拼接的方式生成动态HTML内容，这样就容易导致代码维护困难、可读性差。

2. JSP虽然规避了Servlet在生成HTML内容方面的劣势，但是在HTML中混入大量、复杂的业务逻辑。

**通过MVC双剑合璧**

MVC模式，是Model-View-Controller的简称，是软件工程中的一种软件架构模式，分为三个基本部分，分别是：模型（Model）、视图（View）和控制器（Controller）：

- Controller——负责转发请求，对请求进行处理
- View——负责界面显示
- Model——业务功能编写（例如算法实现）、数据库设计以及数据存取操作实现

MVC模式在Web开发中有很大的优势，它完美规避了JSP与Servlet各自的缺点，让Servlet只负责业务逻辑部分，而不会生成HTML代码；同时JSP中也不会充斥着大量的业务代码，这样能大提高了代码的可读性和可维护性。MVC模式的处理过程如下：

1. Web浏览器发送HTTP请求到服务端，然后被Controller(Servlet)获取并进行处理（例如参数解析、请求转发）
2. Controller(Servlet)调用核心业务逻辑——Model部分，获得结果
3. Controller(Servlet)将逻辑处理结果交给View（JSP），动态输出HTML内容
4. 动态生成的HTML内容返回到浏览器显示

## 2. JSP 处理

以下步骤表明了Web服务器是如何使用JSP来创建网页的：

- 浏览器发送一个HTTP请求给服务器。
- Web服务器识别出这是一个对JSP网页的请求，并且将该请求传递给JSP引擎。通过使用URL或者.jsp文件来完成。
- JSP引擎从磁盘中载入JSP文件，然后将它们转化为servlet。这种转化只是简单地将所有模板文本改用println()语句，并且将所有的JSP元素转化成Java代码。
- JSP引擎将servlet编译成可执行类，并且将原始请求传递给servlet引擎。
- Web服务器的某组件将会调用servlet引擎，然后载入并执行servlet类。在执行过程中，servlet产生HTML格式的输出并将其内嵌于HTTP response中上交给Web服务器。
- Web服务器以静态HTML网页的形式将HTTP response返回到浏览器中。
- Web浏览器处理HTTP response中动态产生的HTML网页。

以上提及到的步骤可以用下图来表示：

![](https://raw.githubusercontent.com/hihanying/Figurebed/master/img/20191202192234.png)

> 一般情况下，JSP引擎会检查JSP文件对应的servlet是否已经存在，并且检查JSP文件的修改日期是否早于servlet。如果JSP文件的修改日期早于对应的servlet，那么容器就可以确定JSP文件没有被修改过并且servlet有效。

## 3. JSP 语法

**脚本程序**

脚本程序可以包含任意量的Java语句、变量、方法或表达式，只要它们在脚本语言中是有效的。

脚本程序的语法格式：`<% 代码片段 %>`

**JSP声明**

一个声明语句可以声明一个或多个变量、方法，供后面的Java代码使用。在JSP文件中，您必须先声明这些变量和方法然后才能使用它们。

JSP声明的语法格式：

`<%! declaration; [ declaration; ]+ ... %>`

程序示例：

```jsp
<%! int i = 0; %>
<%! int a, b, c; %>
<%! Circle a = new Circle(2.0); %> 
```

**JSP表达式**

- JSP表达式先被转化成String，然后插入到表达式出现的地方。
- 可以在一个文本行中使用表达式而不用去管它是否是HTML标签。
- 表达式元素中不能使用分号来结束表达式

JSP表达式的语法格式：`<%= 表达式 %>`

**JSP注释**

JSP注释的语法格式：

```
<%-- 这里可以填写 JSP 注释 --%>
```

不同情况下使用注释的语法规则：

| **语法**       | 描述                                                        |
| :------------- | :---------------------------------------------------------- |
| <%-- 注释 --%> | JSP注释，注释内容不会被发送至浏览器甚至不会被编译，推荐使用 |
| <!-- 注释 -->  | HTML注释，通过浏览器查看网页源代码时可以看见注释内容        |
| <\%            | 代表静态 <%常量                                             |
| %\>            | 代表静态 %> 常量                                            |
| \'             | 在属性中使用的单引号                                        |
| \"             | 在属性中使用的双引号                                        |

> JSP语法还包括JSP指令、JSP行为、JSP隐含对象、控制流语句、JSP运算符、JSP常量

## 4. JSP指令

JSP指令用来设置整个JSP页面相关的属性，如网页的编码方式和脚本语言。

语法格式如下：

```jsp
<%@ directive attribute="value" %>
```

指令可以有很多个属性，它们以键值对的形式存在，并用逗号隔开。

**Page指令**

Page指令为容器提供当前页面的使用说明。一个JSP页面可以包含多个page指令。

Page指令的语法格式：

```jsp
<%@ page attribute="value" %>
```

下表列出与Page指令相关的属性：

| **属性**           | **描述**                                                     |
| :----------------- | :----------------------------------------------------------- |
| buffer             | 指定out对象使用缓冲区的大小                                  |
| autoFlush          | 控制out对象的 缓存区                                         |
| **contentType**    | 指定当前JSP页面的MIME类型和字符编码，等同于response.setContentType() |
| **errorPage**      | 指定当JSP页面发生异常时需要转向的错误处理页面                |
| **isErrorPage**    | 指定当前页面是否可以作为另一个JSP页面的错误处理页面          |
| extends            | 指定servlet从哪一个类继承                                    |
| **import**         | 导入要使用的Java类                                           |
| info               | 定义JSP页面的描述信息                                        |
| isThreadSafe       | 指定对JSP页面的访问是否为线程安全                            |
| language           | 定义JSP页面所用的脚本语言，默认是Java                        |
| session            | 指定JSP页面是否使用session                                   |
| isELIgnored        | 指定是否执行EL表达式                                         |
| isScriptingEnabled | 确定脚本元素能否被使用                                       |

**Include指令**

JSP可以通过include指令来包含其他文件。被包含的文件可以是JSP文件、HTML文件或文本文件。包含的文件就好像是该JSP文件的一部分，会被同时编译执行。

Include指令的语法格式如下：

```jsp
<%@ include file="relative url" %>
```

Include指令中的文件名实际上是一个相对的URL。如果您没有给文件关联一个路径，JSP编译器默认在当前路径下寻找。

**Taglib指令**

JSP API允许用户自定义标签，一个自定义标签库就是自定义标签的集合。

Taglib指令引入一个自定义标签集合的定义，包括库路径、自定义标签。

Taglib指令的语法：

```jsp
<%@ taglib uri="uri" prefix="prefixOfTag" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
```

- `uri`属性确定标签库的位置
- `prefix`属性指定标签库的前缀（自定义）。

## 5. JSP 隐式对象

JSP隐式对象是JSP容器为每个页面提供的Java对象，开发者可以直接使用它们而不用显式声明。JSP隐式对象也被称为**预定义变量**。

JSP所支持的九大隐式对象：

| 对象        | 真实类型            | 描述                                    |
| :---------- | :------------------ | --------------------------------------- |
| request     | HttpServletRequest  | 请求对象                                |
| response    | HttpServletResponse | 响应对象                                |
| out         | PrintWriter         | 用于把结果输出至网页上                  |
| session     | HttpSession         | 跟踪在各个客户端请求间的会话            |
| application | ServletContext      | 所有用户间共享数据                      |
| config      | ServletConfig       | Servlet的配置对象                       |
| pageContext | PageContext         | 提供对JSP页面所有对象以及命名空间的访问 |
| page        |                     | 类似于Java类中的this关键字              |
| Exception   | Exception           | 代表发生错误的JSP页面中对应的异常对象   |

# JSP 高级

## 1. EL（Expression Language）

JSP表达式语言（EL）使得访问存储在JavaBean中的数据变得非常简单。JSP EL既可以用来创建算术表达式也可以用来创建逻辑表达式。在JSP EL表达式内可以使用整型数，浮点数，字符串，常量true、false，还有null。

**语法**

```jsp
${expr}
```

其中，expr指的是表达式。在JSP EL中通用的操作符是"."和"[]"。这两个操作符通过内嵌的JSP对象访问各种各样的JavaBean属性。

**作用**

替换和简化jsp页面中java代码的编写，当JSP编译器在属性中见到"${}"格式后，它会产生代码来计算这个表达式，并且产生一个替代品来代替表达式的值。

**注意**

jsp默认支持el表达式。如果要忽略el表达式，设置jsp中page指令中`isELIgnored="true" `忽略当前jsp页面中所有的el表达式；也可以使用`\${表达式}`忽略当前这个el表达式

**基础操作符**

EL表达式支持大部分Java所提供的算术和逻辑操作符：

| **操作符** | **描述**                         |
| :--------- | :------------------------------- |
| .          | 访问一个Bean属性或者一个映射条目 |
| []         | 访问一个数组或者链表的元素       |
| ( )        | 组织一个子表达式以改变优先级     |
| +          | 加                               |
| -          | 减或负                           |
| *          | 乘                               |
| / or div   | 除                               |
| % or mod   | 取模                             |
| == or eq   | 测试是否相等                     |
| != or ne   | 测试是否不等                     |
| < or lt    | 测试是否小于                     |
| > or gt    | 测试是否大于                     |
| <= or le   | 测试是否小于等于                 |
| >= or ge   | 测试是否大于等于                 |
| && or and  | 测试逻辑与                       |
| \|\| or or | 测试逻辑或                       |
| ! or not   | 测试取反                         |
| empty      | 测试是否空值                     |

**函数**

JSP EL允许在表达式中使用函数。这些函数必须被定义在自定义标签库中。函数的使用语法如下：

```
${ns:func(param1, param2, ...)}
```

- ns指的是命名空间（namespace）
- func指的是函数的名称
- param1指的是第一个参数，param2指的是第二个参数，以此类推。

比如，有函数fn:length，在JSTL库中定义，可以像下面这样来获取一个字符串的长度：

```
${fn:length("Get my length")}
```

要使用任何标签库中的函数，您需要将这些库安装在服务器中，然后使用`<taglib>`标签在JSP文件中包含这些库。

**EL隐含对象**

JSP EL支持下表列出的隐含对象：

| **隐含对象**     | **描述**                      |
| :--------------- | :---------------------------- |
| pageScope        | page 作用域                   |
| requestScope     | request 作用域                |
| sessionScope     | session 作用域                |
| applicationScope | application 作用域            |
| param            | Request 对象的参数，字符串    |
| paramValues      | Request对象的参数，字符串集合 |
| header           | HTTP 信息头，字符串           |
| headerValues     | HTTP 信息头，字符串集合       |
| initParam        | 上下文初始化参数              |
| cookie           | Cookie值                      |
| **pageContext**  | 当前页面的pageContext         |

- pageContext可以获取jsp其他八个内置对象，例如：

  ```jsp
  ${pageContext.request.contextPath}动态获取虚拟目录
  ${pageContext.request.queryString}访问request对象传入的查询字符串
  ```

- pageScope，requestScope，sessionScope，applicationScope变量用来访问存储在各个作用域层次的变量。例如，如果需要显式访问在applicationScope层的box变量，可以这样来访问：`applicationScope.box`。

- param和paramValues对象用来访问参数值。例如，访问一个名为order的参数，可以这样使用表达式：`${param.order}`，或者`${param["order"]}`。

- header和headerValues对象用来访问信息头。例如，要访问一个名为user-agent的信息头，可以这样使用表达式：`${header.user-agent}`，或者​`${header["user-agent"]}`。

## 2. JSTL

JavaServer Pages Tag Library（JSTL）：JSP标准标签库

**作用**

用于简化和替换jsp页面上的java代码	

**使用**

1. 导入jstl相关jar包

   - 从Apache的标准标签库中下载的二进包：[jakarta-taglibs-standard-current.zip]([http://archive.apache.org/dist/jakarta/taglibs/standard/binaries/](https://archive.apache.org/dist/jakarta/taglibs/standard/binaries/))。
   - 解压，将jakarta-taglibs-standard-1.1.1/lib/下的两个jar文件：standard.jar和jstl.jar文件拷贝到/WEB-INF/lib/下。
   - list as  libraries …

2. 使用taglib指令引入核心标签库

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   ```

3. 使用标签

**常用的JSTL核心标签**

| 标签                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`<c:out>`](https://www.w3cschool.cn/jsp/jstl-core-out-tag.html) | 用于在JSP中显示数据，就像`<%= ... >`                         |
| [`<c:set>`](https://www.w3cschool.cn/jsp/jstl-core-set-tag.html) | 用于保存数据                                                 |
| [`<c:remove>`](https://www.w3cschool.cn/jsp/jstl-core-remove-tag.html) | 用于删除数据                                                 |
| [`<c:catch>`](https://www.w3cschool.cn/jsp/jstl-core-catch-tag.html) | 用来处理产生错误的异常状况，并且将错误信息储存起来           |
| [`<c:if>**`](https://www.w3cschool.cn/jsp/jstl-core-if-tag.html)** | **与我们在一般程序中用的if一样**                             |
| [`<c:choose>**`](https://www.w3cschool.cn/jsp/jstl-core-choose-tag.html)** | **本身只当做`<c:when>`和`<c:otherwise>`的父标签**            |
| [`<c:when>`](https://www.w3cschool.cn/jsp/jstl-core-choose-tag.html) | `<c:choose>`的子标签，用来判断条件是否成立                   |
| [`<c:otherwise>`](https://www.w3cschool.cn/jsp/jstl-core-choose-tag.html) | `<c:choose>`的子标签，接在`<c:when>`标签后，当`<c:when>`标签判断为false时被执行 |
| [`<c:import>`](https://www.w3cschool.cn/jsp/jstl-core-import-tag.html) | 检索一个绝对或相对 URL，然后将其内容暴露给页面               |
| [`<c:forEach>`](https://www.w3cschool.cn/jsp/jstl-core-foreach-tag.html) | **基础迭代标签，接受多种集合类型**                           |
| [`<c:forTokens>`](https://www.w3cschool.cn/jsp/jstl-core-foreach-tag.html) | 根据指定的分隔符来分隔内容并迭代输出                         |
| [`<c:param>`](https://www.w3cschool.cn/jsp/jstl-core-param-tag.html) | 用来给包含或重定向的页面传递参数                             |
| [`<c:redirect>`](https://www.w3cschool.cn/jsp/jstl-core-redirect-tag.html) | 重定向至一个新的URL.                                         |
| [`<c:url>`](https://www.w3cschool.cn/jsp/jstl-core-url-tag.html) | 使用可选的查询参数来创造一个URL                              |

**其他标签**

- 格式化标签
- SQL 标签
- XML 标签
- JSTL 函数

