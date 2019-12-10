---
layout:     post                   
title:      JavaWeb05-JSON
subtitle:   JSON 简介、语法、使用
date:       2019-12-07
author:     HY
header-img:img/post-bg-2015.jpg
catalog:true
tags:
    - JavaWeb
    - JavaScript
    - JSON
---

# JSON

## 1. JSON 简介

## 什么是 JSON ？

JSON 指的是 JavaScript 对象表示法（**J**ava**S**cript **O**bject **N**otation），是一种轻量级的文本数据交换格式，独立于编程语言。它类似于于XML。

### JSON 与 XML

**与 XML 相同之处**

* JSON 是纯文本
* JSON 具有"自我描述性"（人类可读）
* JSON 具有层级结构（值中存在值）
* JSON 可通过 JavaScript 进行解析
* JSON 数据可使用 AJAX 进行传输

**与 XML 不同之处**

* 没有结束标签
* 更短
* 读写的速度更快
* 能够使用内建的 JavaScript eval() 方法进行解析
* 使用数组
* 不使用保留字

**为什么使用 JSON？**

对于 AJAX 应用程序来说，JSON 比 XML 更快更易使用

## 2. JSON语法

JSON 的语法基本上可以视为 JavaScript 语法的一个子集，包括以下内容：

* 数据使用名/值对表示。
  * JSON 值可以是：数字（整数或浮点数）、字符串（在双引号中）、布尔值（true 或 false）、数组（在方括号中）、对象（在花括号中）、null
* 使用大括号保存对象，每个名称后面跟着一个 ':'（冒号），名/值对使用 ,（逗号）分割。
* 使用方括号保存数组，数组值使用 ,（逗号）分割。

JSON 支持以下两种数据结构：

* 名/值对集合： 这一数据结构由不同的编程语言支持。
* 有序的值列表： 包括数组，列表，向量或序列等等。

下面是一个简单的示例：

```json
{
    "book": [
        {
            "id":"01",
            "language": "Java",
            "edition": "third",
            "author": "Herbert Schildt"
        },
        {
            "id":"07",
            "language": "C++",
            "edition": "second",
            "author": "E.Balagurusamy"
        }]
}
```

**JSON 文件**

* JSON 文件的文件类型是 ".json"
* JSON 文本的 MIME 类型是 "application/json"

## 3. JSON使用

**JSON经常应用到的场景**

在后台应用程序中将响应数据封装成JSON格式，传到前台页面之后，需要将JSON格式转换为JavaScript对象，然后在网页中使用该数据。

**把 JSON 文本转换为 JavaScript 对象**

第一种方法：JavaScript 函数 eval() 可用于将 JSON 文本转换为 JavaScript 对象。

eval() 函数使用的是 JavaScript 编译器，可解析 JSON 文本，然后生成 JavaScript 对象。必须把文本包围在括号中，这样才能避免语法错误：    

```html
<script>
// 创建一个包含 JSON 语法的 JavaScript 字符串
var txt = '{ "employees" : [' +      
'{ "firstName":"John" , "lastName":"Doe" },' +        
'{ "firstName":"Anna" , "lastName":"Smith" },' +       
'{ "firstName":"Peter" , "lastName":"Jones" } ]}';
//使用eval() 函数解析 JSON 文本生成 JavaScript 对象
var obj = eval ("(" + txt + ")");
//在网页中使用 JavaScript 对象
document.getElementById("fname").innerHTML = obj.employees[1].firstName
document.getElementById("lname").innerHTML = obj.employees[1].lastName
</script>
<!--------------------------------------------------------------------->
<p>
First Name: <span id="fname"></span><br />
Last Name: <span id="lname"></span><br />
</p>
```

第二种方法：JSON 解析器

使用 JSON 解析器将 JSON 转换为 JavaScript 对象是更安全的做法。JSON 解析器只能识别 JSON 文本，而不会编译脚本。

## 4. 在 Java 中使用 JSON

### JSON 和 Java 实体映射

JSON 实体映射从左侧到右侧为解码或解析，实体映射从右侧到左侧为编码。

| JSON          | Java              |
| ------------- | ----------------- |
| string        | java.lang.String  |
| number        | java.lang.Number  |
| true \| false | java.lang.Boolean |
| null          | null              |
| array         | java.util.List    |
| object        | java.util.Map     |

解码时，*java.util.List* 的默认具体类是 *org.json.simple.JSONArray*，*java.util.Map* 的默认具体类是 *org.simple.JSONObject*。

### 在 Java 中编码 JSON























