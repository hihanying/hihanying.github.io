---
layout:     post                   
title:      JavaWeb05-jQuery
subtitle:   1．jQuery快速入门2．jQuery语法详解3．jQuery核心函数4．jQuery对象/JavaScript对象5．jQuery选择器6．jQuery 文档处理7．jQuery事件8．jQuery动画效果9．jQuery的遍历10.AJAX
date:       2019-11-07       
author:     HY                 
header-img: img/post-bg-2015.jpg
catalog: true                  
tags:                   
    - JavaWeb
    - jQuery
	- AJAX
---

# jQuery

- jQuery 是一个 JavaScript 库。
- jQuery 极大地简化了 JavaScript 编程。
- jQuery 很容易学习。

## 1. jQuery 简介

jQuery的字面意思其实就是JavaScript和查询（Query），即用于辅助开发JavaScript的库。jQuery是继prototype之后的又一个优质的Javascript库，属于开源编程语言。

### jQuery的缺点

1. 不能向后兼容。每一个新版本不能兼容早期的版本。 
2. 插件兼容性不是太好，与上一点类似。 
3. 在同一页面上使用多个插件时，很容易碰到冲突现象，尤其是这些插件依赖相同事件或selector时最为明显。 
4. 在大型框架中，jQuery核心代码库对动画和特效的支持相对较差。但是实际上这不是一个问题。目前在这方面有一个单独的jQuery UI项目和众多插件来弥补此点。

### jQuery版本支持

目前jQuery有三个大版本：

- 1.x：兼容 IE6，7，8，使用最为广泛的，官方只做BUG维护，功能不再新增。因此一般项目来说，使用1.x版本就可以了，最终版本：1.12.4 (2016年5月20日)
- 2.x：不兼容 IE6，7，8 ，很少有人使用，官方只做BUG维护，功能不再新增。如果不考虑兼容低版本的浏览器可以使用2.x，最终版本：2.2.4 (2016年5月20日)
- 3.x：不兼容 IE6，7，8 ，只支持最新的浏览器。除非特殊要求，一般不会使用3.x版本的，很多老的jQuery插件不支持这个版本。目前该版本是官方主要更新维护的版本。最新版本：3.4.1

可以通过条件注释在使用 IE6，7，8 浏览器的时候只允许包含 jQuery 1.9。注释代码如下：

```HTML
<!--[if lt IE 9]> 
    <script src="jquery-1.9.0.js"></script> 
<![endif]--> 
<!--[if gte IE 9]><!--> 
    <script src="jquery-2.0.0.js"></script> 
<!--<![endif]-->
```

> **html 条件注释：**
>
> | 符号 | 范例                     | 说明                                                       |
> | ---- | ------------------------ | ---------------------------------------------------------- |
> | !    | [if !IE]                 | NOT运算符。                                                |
> | lt   | [if lt IE 5.5]           | 小于运算符。第一个参数小于第二个参数返回true。             |
> | lte  | [if lte IE 6]            | 小于或等于运算。第一个参数是小于或等于第二个参数返回true。 |
> | gt   | [if gt IE 5]             | 大于运算符。第一个参数大于第二个参数返回true。             |
> | gte  | [if gte IE 7]            | 大于或等于运算。第一个参数是大于或等于第二个参数返回true。 |
> | ( )  | [if !(IE 7)]             | 子表达式。                                                 |
> | &    | [if (gt IE 5)&(lt IE 7)] | AND运算符。如果所有的子表达式计算结果为true，返回true      |
> | \|   | [if (IE 6)\|(IE 7)]      | OR运算符。返回true，如果子表达式计算结果为true。           |

### jQuery的使用

1. 从官网 [jquery.com](http://jquery.com/download/) 下载 jQuery 库，右键下面两个链接，选择“连接另存为”
   - `Download the compressed, production jQuery 3.4.1` 对应 `jquery-3.4.1.min.js` ，用来导包
   - `Download the uncompressed, development jQuery 3.4.1` 对应 `jquery-3.4.1.js` ，用来查看
2. 将`jquery-3.4.1.min.js`文件复制到项目中的`/js`目录中
3. 在html文件的`<head>`标签内配置后即可使用
    ```html
    <script src="js/jquery-3.4.1.min.js"></script>
    ```

> **jquery-xxx.js 与 jquery-xxx.min.js区别**
>
> 1. jquery-xxx.js：开发版本。给程序员看的，有良好的缩进和注释。体积大一些
> 2. jquery-xxx.min.js：生产版本。程序中使用，没有缩进。体积小一些。程序加载更快
>
> **替代方案**
>
> 如果不希望下载并存放 jQuery，可以通过 CDN（内容分发网络） 引用。百度、又拍云、新浪、谷歌和微软的服务器都存有 jQuery 。国内建议使用百度、又拍云、新浪等国内CDN地址，国外可以使用谷歌和微软。
>
> Baidu CDN:
>
> <head>
>     <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
> </head>

### JQuery 和 JS

- 区别：
	1. JQuery对象在操作时，更加方便。
	2. JQuery对象和js对象方法不通用的。

	```html
	<script>
	    // 1. 通过js方式来获取名称叫div的所有html元素对象
	    var divs = document.getElementsByTagName("div");
	    alert(divs.length);//可以将其当做数组来使用
	    // 2. 对divs中所有的div 让其标签体内容变为"aaa"
	    for (var i = 0; i < divs.length; i++) {
	        //divs[i].innerHTML = "aaa";
	        $(divs[i]).html("ccc");
	    }
	</script>
	```

	```html
	<script>
	    // 1. 通过jq方式来获取名称叫div的所有html元素对象
	    var $divs = $("div");
	    alert($divs.length);//也可以当做数组使用
	    // 2. 对divs中所有的div，使用jq方式让其标签体内容变为"bbb"  
	    $divs.html("bbb");
	    //$divs.innerHTML = "bbb";
	</script>
	```
- 转换
	* jq -> js : `js = jq对象[索引] 或者 jq对象.get(索引)`
	* js -> jq : `jq = $(js对象)`

## 2. jQuery 语法

jQuery 语法是通过选取 HTML 元素，并对选取的元素执行某些操作。

### 基础语法

 `$(selector).action()`

- 美元符号定义 jQuery
- 选择符（selector）"查询"和"查找" HTML 元素
- jQuery 的 action() 执行对元素的操作

实例

- `$(this).hide()` ：隐藏当前元素
- `$("p").hide()`：隐藏所有段落
- `$("p .test").hide()`：隐藏所有 class="test" 的段落
- `$("#test").hide()`：隐藏所有 id="test" 的元素

### 文档就绪事件

jQuery 函数应该位于一个 document ready 函数中：

```html
$(document).ready(function(){
	// 开始写 jQuery 代码...
}); 
```

这是为了防止文档在完全加载（就绪）之前运行 jQuery 代码。如果在文档没有完全加载之前就运行函数，操作可能失败。例如：

- 试图隐藏一个不存在的元素
- 获得未完全加载的图像的大小

简洁写法（与以上写法效果相同）:

```html
$(function(){
	// 开始写 jQuery 代码...
}); 
```

## 3. jQuery 选择器

- jQuery 选择器对 HTML 元素组或单个元素进行操作。
- jQuery 选择器基于元素的 id、类、类型、属性、属性值等"查找"（或选择）HTML 元素。
- jQuery 中所有选择器都以美元符号开头：$()。

### 基础选择器

**元素选择器**

jQuery 元素选择器基于元素名选取元素。

在页面中选取所有 <p> 元素：`$("p")`

**#id 选择器**

jQuery #id 选择器通过 HTML 元素的 id 属性选取指定的元素。

页面中元素的 id 应该是唯一的，要在页面中选取唯一的元素需要通过 #id 选择器。

通过 id 选取元素语法如下：`$("#test")`

**.class 选择器**

jQuery 类选择器可以通过指定的 class 查找元素。

语法如下：`$(".test")`

**并集选择器**

jQuery 并集选择器获取多个选择器选中的所有元素

语法： $("选择器1,选择器2....") 

**CSS 选择器**

jQuery CSS 选择器可用于改变 HTML 元素的 CSS 属性。

下面的例子把所有 p 元素的背景颜色更改为红色：`$("p").css("background-color","red");`

### 层级选择器

1. 后代选择器
	* 语法： $("A B ") 选择A元素内部的所有B元素		
2. 子选择器
	* 语法： $("A > B") 选择A元素内部的所有B子元素

### 属性选择器

1. 属性名称选择器 
	* 语法：` $("A[属性名]")` 包含指定属性的选择器
2. 属性选择器
	* 语法：` $("A[属性名='值']")` 包含指定属性等于指定值的选择器
3. 复合属性选择器
	* 语法： `$("A[属性名='值'][]...")` 包含多个属性条件的选择器

> 匹配属性值开头结尾
>
> * `$("A[属性名^='str'][]...")` ：包含指定属性值以‘str’开头的选择器
> * `$("A[属性名$='str'][]...")` ：包含指定属性值以‘str’结尾的选择器
> * `$("A[属性名*='str'][]...")` ：包含指定属性值包含‘str’的选择器

### 过滤选择器

1. 首元素选择器 
	* 语法： ：first 获得选择的元素中的第一个元素
2. 尾元素选择器 
	* 语法： ：last 获得选择的元素中的最后一个元素
3. 非元素选择器
	* 语法： ：not(selector) 不包括指定内容的元素
4. 偶数选择器
	* 语法： ：even 偶数，从 0 开始计数
5. 奇数选择器
	* 语法： ：odd 奇数，从 0 开始计数
6. 等于索引选择器
	* 语法： ：eq(index) 指定索引元素
7. 大于索引选择器 
	* 语法： ：gt(index) 大于指定索引元素
8. 小于索引选择器 
	* 语法： ：lt(index) 小于指定索引元素
9. 标题选择器
	* 语法： ：header 获得标题（h1~h6）元素，固定写法

### 表单过滤选择器

1. 可用元素选择器 
	* 语法： ：enabled 获得可用元素
2. 不可用元素选择器 
	* 语法： ：disabled 获得不可用元素
3. 选中选择器 
	* 语法： ：checked 获得单选/复选框选中的元素
4. 选中选择器 
	* 语法： ：selected 获得下拉框选中的元素

### 更多实例

| 语法                     | 描述                                              |
| :----------------------- | :------------------------------------------------ |
| $("*")                   | 选取所有元素                                      |
| $(this)                  | 选取当前 HTML 元素                                |
| $("p.intro")             | 选取 class 为 intro 的 <p> 元素                   |
| $("p:first")             | 选取第一个 <p> 元素                               |
| $("ul li:first")         | 选取第一个 <ul> 元素的第一个 <li> 元素            |
| $("ul li:first-child")   | 选取每个 <ul> 元素的第一个 <li> 元素              |
| $("[href]")              | 选取带有 href 属性的元素                          |
| $("a[target='_blank']")  | 选取所有 target 属性值等于 "_blank" 的 <a> 元素   |
| $("a[target!='_blank']") | 选取所有 target 属性值不等于 "_blank" 的 <a> 元素 |

## 4. jQuery 事件

###  jQuery 事件方法

**什么是事件？**

页面对不同访问者的响应叫做**事件**。**事件处理程序**指的是当 HTML 中发生某些事件时所调用的方法。

实例：

* 在元素上移动鼠标。
* 选取单选按钮
* 点击元素

常见 DOM 事件：

| 鼠标事件   | 键盘事件 | 表单事件 | 文档/窗口事件 |
| :--------- | :------- | :------- | :------------ |
| click      | keypress | submit   | load          |
| dblclick   | keydown  | change   | resize        |
| mouseenter | keyup    | focus    | scroll        |
| mouseleave |          | blur     | unload        |

**jQuery 事件语法**

在 jQuery 中，大多数DOM 事件都有一个等效的 jQuery 方法。如果用事件方法，不传递回调函数，则会触发浏览器默认行为。

```html
<script>
    //页面中指定一个点击事件，通过一个事件函数实现事件
    $("p").click(function(){        
        // action goes here!!
    });
</script>
```

**jQuery 事件方法**

| 事件方法     | 描述                                                         |
| ------------ | :----------------------------------------------------------- |
| click()      | 当按钮点击事件被触发时会调用一个函数。该函数在用户点击 HTML 元素时执行。 |
| dblclick()   | 当双击元素时，会发生 dblclick 事件。                         |
| mouseenter() | 当鼠标指针离开元素时，会发生 mouseleave 事件。               |
| mousedown()  | 当鼠标指针移动到元素上方，并按下鼠标按键时，会发生 mousedown 事件。 |
| mouseup()    | 当在元素上松开鼠标按钮时，会发生 mouseup 事件。              |
| mouseleave() | 当鼠标指针离开元素时，会发生 mouseleave 事件。               |
| hover()      | hover()方法用于模拟光标悬停事件。当鼠标移动到元素上时，会触发指定的第一个函数(mouseenter);当鼠标移出这个元素时，会触发指定的第二个函数(mouseleave)。 |
| focus()      | 当元素获得焦点时，发生 focus 事件。                          |
| blur()       | 当元素失去焦点时，发生 blur 事件。当通过鼠标点击选中元素或通过 tab 键定位到元素时，该元素就会获得焦点。 |

> **模拟光标悬停案例**
>
> ```html
> $("#p1").hover(
> 	function(){
>     	alert("You entered p1!");
>     },
>     function(){
>     	alert("Bye! You now leave p1!");
> });
> ```

**比较keypress、keydown与keyup**

* keydown：在键盘上按下某键时发生，一直按着则会不断触发（opera浏览器除外），它返回的是键盘代码;
* keypress：在键盘上按下一个按键，并产生一个字符时发生, 返回ASCII码。注意: shift、alt、ctrl等键按下并不会产生字符，所以监听无效，换句话说，只有按下能在屏幕上输出字符的按键时keypress事件才会触发。若一直按着某按键则会不断触发。
* keyup：用户松开某一个按键时触发，与keydown相对，返回键盘代码.

**jQuery on绑定事件 / off接触事件**

* `jq对象.on("事件名称",回调函数)`

* `jq对象.off("事件名称")`

  * 如果off方法不传递任何参数，则将组件上的所有事件全部解绑
  ```html
  <script>
      $(function () {
          //1.使用on给按钮绑定单击事件  click
          $("#btn").on("click",function () {
              alert("我被点击了。。。")
          }) ;
          //2. 使用off解除btn按钮的单击事件
          $("#btn2").click(function () {
              //$("#btn").off("click");//解除btn按钮的单击事件
              $("#btn").off();//将组件上的所有事件全部解绑
          });
      });
  </script>
  ```

**jQuery on绑定事件 / off接触事件**

* `jq对象.toggle(fun1,fun2...)`

  * 当单击jq对象对应的组件后，会执行fun1，第二次点击会执行fun2.....
  * 注意：1.9版本 .toggle() 方法删除，jQueryMigrate（迁移）插件可以恢复此功能。

  ```html
  <script src="../js/jquery-migrate-1.0.0.js" type="text/javascript" charset="utf-8"></script>
  <script type="text/javascript">
      $(function () {
          //获取按钮，调用toggle方法
          $("#btn").toggle(function () {
              //改变div背景色backgroundColor 颜色为 green
              $("#myDiv").css("backgroundColor","green");
          },function () {
              //改变div背景色backgroundColor 颜色为 pink
              $("#myDiv").css("backgroundColor","pink");
          });
      });
  </script>
  ```
  ```html
  <body>
      <input id="btn" type="button" value="事件切换">
      <div id="myDiv" style="width:300px;height:300px;background:pink">
          点击按钮变成绿色，再次点击红色
      </div>
  </body>
  ```

## 5. jQuery 效果

### 隐藏和显示

**hide() 和 show()**

```
$(selector).hide(speed,callback);
$(selector).show(speed,callback);
```

* 
  可选的 speed 参数规定隐藏/显示的速度，可以取以下值："slow"、"fast" 或毫秒。
* 可选的 callback 参数是隐藏或显示完成后所执行的函数名称。

**jQuery toggle()**

通过 jQuery，您可以使用 toggle() 方法来切换 hide() 和 show() 方法。 

```
$(selector).toggle(speed,callback);
// 例如，显示被隐藏的元素，并隐藏已显示的元素：
$("button").click(function(){
  $("p").toggle();
});
```

* 可选的 speed 参数规定隐藏/显示的速度，可以取以下值："slow"、"fast" 或毫秒。
* 可选的 callback 参数是 toggle() 方法完成后所执行的函数名称。
* 可选的 callback 参数，具有以下三点说明：
  1. $(selector)选中的元素的个数为n个，则callback函数会执行n次
  2. callback 函数名后加括号，会立刻执行函数体，而不是等到显示/隐藏完成后才执行
  3. callback 既可以是函数名，也可以是匿名函数

### 淡入淡出

在在jQuery中可以通过四个方法来实现元素的淡入淡出，这四个方法分别是：fadeIn()、fadeOut()、fadeToggle() 以及 fadeTo()。



**jQuery fadeIn() 用于淡入已隐藏的元素**

```
$(selector).fadeIn(speed,callback);
// 实例
$("button").click(function(){
  $("#div1").fadeIn();
  $("#div2").fadeIn("slow");
  $("#div3").fadeIn(3000);
});
```

* 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。.
* 可选的 callback 参数是 fading 完成后所执行的函数名称。

**jQuery fadeOut() 方法用于淡出可见元素**

```
$(selector).fadeOut(speed,callback);
```

**jQuery fadeToggle() 方法可以在 fadeIn() 与 fadeOut() 方法之间进行切换。**

```
$(selector).fadeToggle(speed,callback);
```

**jQuery fadeTo() 方法允许渐变为给定的不透明度（值介于 0 与 1 之间）。**

```
$(selector).fadeTo(speed,opacity,callback);
```

* 必需的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
* 必需的 opacity 参数将淡入淡出效果设置为给定的不透明度（值介于 0 与 1 之间）。
* 可选的 callback 参数是该函数完成后所执行的函数名称。

### 滑动

**jQuery slideDown() 方法用于向下滑动元素。**

```
$(selector).slideDown(speed,callback);
```

* 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
* 可选的 callback 参数是滑动完成后所执行的函数名称。

**jQuery slideUp() 方法用于向上滑动元素。**

```
$(selector).slideUp(speed,callback);
```

**jQuery slideToggle() 方法可以在 slideDown() 与 slideUp() 方法之间进行切换。**

```
$(selector).slideToggle(speed,callback);
// 实例
<script> 
$(document).ready(function(){
  $("#flip").click(function(){
    $("#panel").slideToggle("slow");
  });
});
</script>
```

### 动画

要实现更加丰富的效果可以通过使用 jQuery animate() 方法自定义动画来达到目的

**jQuery animate() 方法用于创建自定义动画。**

语法：

```
 $(selector).animate({params},speed,callback);
```

* 必需的 params 参数定义形成动画的 CSS 属性。
* 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
* 可选的 callback 参数是动画完成后所执行的函数名称。

实例：操作多个属性

```
$("button").click(function(){
	$("div").animate({
        left:'250px',		//把 <div> 元素往右边移动了 250 像素
        opacity:'0.5',		//透明度变为原来的0.5
        height:'150px',		//高度变为150px
        width:'150px'
    });
}); 
```

> 默认情况下，所有 HTML 元素都有一个静态位置，且无法移动。
> 如需对位置进行操作，要记得首先把元素的 position 属性设置为 relative、fixed 或 absolute
>
> **注意：**在 jQuery 的 animate()方法中，第一个参数是一个必须参数，表示一个CSS属性和值的对象。

**实例：使用相对值，需要在值的前面加上 += 或 -=**

```
$("button").click(function(){
    $("div").animate({
        left:'250px',
        height:'+=150px',
        width:'+=150px'
    });
}); 
```

**实例： 使用预定义的值，把属性的动画值设置为 "show"、"hide" 或 "toggle"**

```
$("button").click(function(){
    $("div").animate({
    	height:'toggle'
    });
}); 
```

**实例： 使用队列功能**

这意味着如果编写多个 animate() 调用，jQuery 会创建包含这些方法调用的"内部"队列。然后逐一运行这些 animate 调用。

```
$("button").click(function(){
  var div=$("div");
  div.animate({height:'300px',opacity:'0.4'},"slow");
  div.animate({width:'300px',opacity:'0.8'},"slow");
  div.animate({height:'100px',opacity:'0.4'},"slow");
  div.animate({width:'100px',opacity:'0.8'},"slow");
  div.animate({left:'100px'},"slow");
  div.animate({fontSize:'3em'},"slow");
}); 

```

**jQuery stop() 方法用于停止动画或效果**

stop() 方法适用于所有 jQuery 效果函数，包括滑动、淡入淡出和自定义动画。

```
 $(selector).stop(stopAll,goToEnd);
```

* 可选的 stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。
* 可选的 goToEnd 参数规定是否立即完成当前动画。默认是 false。

### Callback 方法

由于 JavaScript 语句（指令）是按照次序逐一执行的，动画之后的语句可能会产生错误或页面冲突，因为动画还没有完成。为了避免这个情况，可以以参数的形式添加 Callback 函数。

**Callback 函数在当前动画 100% 完成之后执行。**

以下实例在隐藏效果完全实现后回调函数:

```
$("button").click(function(){
    $("p").hide("slow",function(){
    	alert("The paragraph is now hidden");
    });
});
```

### 方法链接

之前，我们都是一次写一条 jQuery 语句（一条接着另一条）。

**Chaining 允许我们在一条语句中运行多个 jQuery 方法（在相同的元素上）。**

```
// "p1" 元素首先会变为红色，然后向上滑动，再然后向下滑动
$("#p1").css("color","red").slideUp(2000).slideDown(2000);
// 格式化书写
$("#p1").css("color","red")
    .slideUp(2000)
    .slideDown(2000);
```

## 6. jQuery HTML

### jQuery 设置/获取内容和属性

jQuery 中非常重要的部分，就是操作 DOM 的能力。jQuery 提供一系列与 DOM 相关的方法，这使访问和操作元素和属性变得很容易。

> **DOM = Document Object Model（文档对象模型）**
>
> DOM 定义访问 HTML 和 XML 文档的标准："W3C 文档对象模型独立于平台和语言的界面，允许程序和脚本动态访问和更新文档的内容、结构以及样式。"

**设置/获取内容**

三个简单实用的用于 DOM 操作的 jQuery 方法：

* text() - 设置或返回所选元素的文本内容
* html() - 设置或返回所选元素的内容（包括 HTML 标记）
* val() - 设置或返回表单字段的值

获取内容

```
$("#btn1").click(function(){
  alert("Text: " + $("#test").text());
});
$("#btn2").click(function(){
  alert("HTML: " + $("#test").html());
});
$("#btn1").click(function(){
  alert("Value: " + $("#test").val());
});
```

设置内容

```
$("#btn1").click(function(){
  $("#test1").text("Hello world!");
});
$("#btn2").click(function(){
  $("#test2").html("<b>Hello world!</b>");
});
$("#btn3").click(function(){
  $("#test3").val("Dolly Duck");
});
```

**设置/获取属性**

1. attr()：获取/设置元素的属性
2. removeAttr()：删除属性
3. prop()：获取/设置元素的属性
4. removeProp()：删除属性

案例：使用 jQuery 获取和设置元素的属性值

```html
<script>
    //获取属性值
    $("button").click(function(){
        alert($("#w3s").attr("href"));
    });
    //同时设置 href 和 title 属性：
    $("button").click(function(){
        $("#w3s").attr({
            "href" : "//www.w3cschool.cn/jquery",
            "title" : "jQuery 教程"
        });
    });
</script>
```

attr和prop区别？

1. 如果操作的是元素的固有属性，则建议使用prop
2. 如果操作的是元素自定义的属性，则建议使用attr

### jQuery 添加/删除元素

* jQuery append() 方法在被选元素的结尾插入内容。
* jQuery prepend() 方法在被选元素的开头插入内容。

```
$("p").append("Some appended text.");
$("p").prepend("Some prepended text.");
```

> * appendTo()：`对象1.appendTo(对象2)`将对象1添加到对象2内部，并且在末尾
>
> * prependTo()：`对象1.prependTo(对象2)`将对象1添加到对象2内部，并且在开头

* jQuery after() 方法在被选元素之后插入内容。
* jQuery before() 方法在被选元素之前插入内容。

```
$("img").after("在后面添加文本");
$("img").before("在前面添加文本");
```

> * insertAfter()：`对象1.insertAfter(对象2)`将对象2添加到对象1后边。对象1和对象2是兄弟关系
> * insertBefore()：`对象1.insertBefore(对象2)` 将对象2添加到对象1前边。对象1和对象2是兄弟关系

案例：通过 after() 方法添加若干新元素

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <script src="//libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
        <script>
            function afterText()
            {
                var txt1="<b>I </b>";                    // 使用 HTML 创建元素
                var txt2=$("<i></i>").text("love ");     // 使用 jQuery 创建元素
                var txt3=document.createElement("big");  // 使用 DOM 创建元素
                txt3.innerHTML="jQuery!";
                $("img").after(txt1,txt2,txt3);          // 在图片后添加文本
            }
        </script>
    </head>
    <body>
        <img src="/statics/images/w3c/logo.png" >
        <br><br>
        <button onclick="afterText()">之后插入</button>
    </body>
</html>
```

* jQuery remove() 方法删除被选元素及其子元素。

  * jQuery remove() 方法也可接受一个参数，允许对被删元素进行过滤。
  * 该参数可以是任何 jQuery 选择器的语法。

  ```
  $("#div1").remove();
  $("p").remove(".italic");//删除 class="italic" 的所有 <p> 元素：
  ```

* jQuery empty() 方法删除被选元素的子元素，但是保留当前对象以及其属性节点

  ```
  $("#div1").empty();
  ```

### jQuery 获取并设置 CSS

* addClass() - 向被选元素添加一个或多个类

* removeClass() - 从被选元素删除一个或多个类

* toggleClass() - 对被选元素进行添加/删除类的切换操作

```html
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("h1,h2,p").addClass("blue");	
    $("div").addClass("important");
    $("#div1").addClass("important blue");//同时添加多个类
  });
});
</script>
<style type="text/css">
.important
{
	font-weight:bold;
	font-size:xx-large;
}
.blue
{
	color:blue;
}
</style>
```

**css() 方法设置或返回被选元素的一个或多个样式属性。**

语法：

* 返回指定的 CSS 属性：`css("propertyname");`
* 设置指定的 CSS 属性：`css("propertyname","value");`

案例：

```html
<script>
    $("p").css("background-color");//返回首个匹配元素的 background-color 值
    $("p").css("background-color","yellow");//为所有匹配元素设置 background-color 值
    $("p").css({"background-color":"yellow","font-size":"200%"});//为所有匹配元素设置 background-color 和 font-size
</script>
```

### jQuery 尺寸

jQuery 提供多个处理尺寸的重要方法：

* width() 方法设置或返回元素的宽度（不包括内边距、边框或外边距）。
* height() 方法设置或返回元素的高度（不包括内边距、边框或外边距）。
* innerWidth() 方法返回元素的宽度（包括内边距）。
* innerHeight() 方法返回元素的高度（包括内边距）。
* outerWidth() 方法返回元素的宽度（包括内边距和边框）。
* outerHeight() 方法返回元素的高度（包括内边距和边框）。

**提示：**outerWidth(true) 方法返回元素的宽度（包括内边距、边框和外边距）；outerHeight(true) 方法返回元素的高度（包括内边距、边框和外边距）。

```html
<script>
    $("button").click(function(){
        var txt="";
        txt += "Width: " + $("#div1").width() + "</br>";
        txt += "Height: " + $("#div1").height();
        txt += "Inner width: " + $("#div1").innerWidth() + "</br>";
        txt += "Inner height: " + $("#div1").innerHeight(); 
        txt += "Outer width: " + $("#div1").outerWidth() + "</br>";
        txt += "Outer height: " + $("#div1").outerHeight();    
        $("#div1").html(txt);
    });
</script>
```



## 7. jQuery 遍历

下图展示了一个家族树。通过 jQuery 遍历，从被选（当前的）元素开始，在家族树中向上移动（祖先），向下移动（子孙），水平移动（同胞）。这种移动被称为对 DOM 进行遍历。

![](https://raw.githubusercontent.com/hihanying/Figurebed/master/img/20191205224514.png)

### 向上遍历 DOM 树

* **parent() 方法返回被选元素的直接父元素。**该方法只会向上一级对 DOM 树进行遍历。
* **parents() 方法返回被选元素的所有祖先元素。**它一路向上直到文档的根元素 (`<html>`)。
* **parentsUntil() 方法返回介于两个给定元素之间的所有祖先元素。**

```html
<script>
    $(document).ready(function(){
        $("span").parent();//返回每个 <span> 元素的的直接父元素
        $("span").parents();//返回所有 <span> 元素的所有祖先
        $("span").parents("ul");//返回所有 <span> 元素的所有祖先，并且它是 <ul> 元素
        $("span").parentsUntil("div");//返回介于 <span> 与 <div> 元素之间的所有祖先元素
    });
</script>
```

### 向下遍历 DOM 树

* **children() 方法返回被选元素的所有直接子元素。**该方法只会向下一级对 DOM 树进行遍历。
* **find() 方法返回被选元素的后代元素，一路向下直到最后一个后代。**

```html
<script>
    $(document).ready(function(){
        $("div").children();//返回每个 <div> 元素的所有直接子元素
        $("div").children("p.1");//返回类名为 "1" 的所有<p>元素，并且它们是<div>的直接子元素
        $("div").find("span");//返回属于 <div> 后代的所有 <span> 元素
        $("div").find("*");//返回 <div> 的所有后代
    });
</script>
```

### 在 DOM 树中水平遍历

* **siblings() 方法返回被选元素的所有同胞元素。**
* **next() 方法返回被选元素的下一个同胞元素。**该方法只返回一个元素。
* **nextAll() 方法返回被选元素的所有跟随的同胞元素。**
* **nextUntil() 方法返回介于两个给定参数之间的所有跟随的同胞元素。**开区间

```html
<script>
    $(document).ready(function(){
        $("h2").siblings();//返回 <h2> 的所有同胞元素
        $("h2").siblings("p");//返回属于 <h2> 的同胞元素的所有 <p> 元素
        $("h2").next();//返回 <h2> 的下一个同胞元素
        $("h2").nextAll();//返回 <h2> 的所有跟随的同胞元素
        $("h2").nextUntil("h6");//返回介于 <h2> 与 <h6> 元素之间的所有同胞元素
    });
</script>
```

> `prev()`, `prevAll()` 以及 `prevUntil()` 方法的工作方式与上面的方法类似，只不过方向相反而已：它们返回的是前面的同胞元素（在 DOM 树中沿着同胞元素向后遍历，而不是向前）。

### 过滤：缩小搜索元素范围

* **first() 方法返回被选元素的首个元素。**
* **last() 方法返回被选元素的最后一个元素。**
* **eq() 方法返回被选元素中带有指定索引号的元素。**索引号从 0 开始，因此首个元素的索引号是 0 而不是 1。
* **filter() 方法允许您规定一个标准。**不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回。
* **not() 方法返回不匹配标准的所有元素。**not() 方法与 filter() 相反。

```html
<script>
    $(document).ready(function(){
        $("div p").first();//选取首个 <div> 元素内部的第一个 <p> 元素
        $("div p").last();//选择最后一个 <div> 元素中的最后一个 <p> 元素
        $("p").eq(1);//选取第二个 <p> 元素（索引号 1）
        $("p").filter(".intro");//返回带有类名 "intro" 的所有 <p> 元素
        $("p").not(".intro");//返回不带有类名 "intro" 的所有 <p> 元素
    });
</script>
```

### jQuery 其他遍历方法

1. jq对象.each(callback)
				```
	jquery对象.each(function(index,element){});
	```
	
	   * index:就是元素在集合中的索引
	   * element：就是集合中的每一个元素对象
	   * this：集合中的每一个元素对象
	2. return：
		* 返回为false，则结束循环(break)。
		* 返回为true，则结束本次循环，继续下次循环(continue)
	
	```html
	<script>
	    $(function () {
	        citys.each(function (index,element) {
	            //3.1 获取li对象第一种方式 this
	            alert(this.innerHTML);
	            alert($(this).html());
	            //3.2 获取li对象第二种方式 在回调函数中定义参数 index（索引）element（元素对象）
	            alert(index+":"+element.innerHTML);
	            alert(index+":"+$(element).html());
	            //判断如果是上海，则结束循环
	            if("上海" == $(element).html()){
	                return true;
	            }
	            alert(index+":"+$(element).hml());
	        });
	    });
	</script> 
	```
	
2. `$.each(object, [callback])`

      ```html
      <script>
          $(function () {
              $.each(citys,function () {
                  alert($(this).html());
              });
          });
      </script> 
      ```

3. `for..of`：jquery 3.0 版本之后提供的方式

  ```html
  <script>
      $(function () {
          //for(元素对象 of 容器对象)
          for(li of citys){
              alert($(li).html());
          }
      });
  </script> 
  ```

## 8. jQuery AJAX

### jQuery AJAX 简介

AJAX ，即 异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。

**要解决的问题**

* AJAX 可以用于创建快速动态的网页。
* AJAX 是一种**在无需重新加载整个网页的情况下，能够更新部分网页**的技术。
  * 通过 jQuery AJAX 方法，能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON，同时能够把这些外部数据直接载入网页的被选元素中。
  * 例如：百度搜索框搜索建议功能、注册界面查看用户名是否已被注册

**网络学习资源**

*  [jQuery Ajax 参考手册](https://www.w3cschool.cn/jquery/ajax-ajax.html)： jQuery Ajax 的具体应用。
*  [AJAX 教程](https://www.w3cschool.cn/ajax/)：有关 AJAX 的知识。

### jQuery AJAX 实现

**1. 原生的JS实现方式**

HTML：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script>
            function  fun() {
                //1.创建核心对象
                var xmlhttp;
                if (window.XMLHttpRequest)
                {// code for IE7+, Firefox, Chrome, Opera, Safari
                    xmlhttp=new XMLHttpRequest();
                }
                else
                {// code for IE6, IE5
                    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
                }
                //2. 建立连接
                /* 参数：
                    1. 请求方式：GET、POST
                        * get方式，请求参数在URL后边拼接。send方法为空参
                        * post方式，请求参数在send方法中定义
                    2. 请求的URL
                    3. 同步或异步请求：true（异步）或 false（同步）
             	*/
                xmlhttp.open("GET","ajaxServlet?username=tom",true);
                //3.发送请求
                xmlhttp.send();
                //4.接受并处理来自服务器的响应结果
                xmlhttp.onreadystatechange=function()
                {//当xmlhttp对象的就绪状态改变时，触发事件onreadystatechange。
                    if (xmlhttp.readyState==4 && xmlhttp.status==200)
                    {//判断readyState就绪状态是否为4，判断status响应状态码是否为200
                        var responseText = xmlhttp.responseText;//获取服务器的响应结果
                        alert(responseText);
                    }
                }
            }
        </script>
    </head>
    <body>
        <input type="button" value="发送异步请求" onclick="fun();">
        <input>
    </body>
</html>
```

Servlet：

```java
package cn.itcast.web.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/ajaxServlet")
public class AjaxServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.获取请求参数
        String username = request.getParameter("username");
        //2.打印username
        System.out.println(username);
        //3.响应
        response.getWriter().write("hello : " + username);
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

**2. JQeury实现方式：$.ajax()**

```html
<script>
    function  fun() {
        $.ajax({
            url:"ajaxServlet1111" , // 请求路径
            type:"POST" , //请求方式
            //data: "username=jack&age=23",//请求参数
            data:{"username":"jack","age":23},
            success:function (data) {
                alert(data);
            },//响应成功后的回调函数
            error:function () {
                alert("出错啦...")
            },//表示如果请求响应出现错误，会执行的回调函数
            dataType:"text"//设置接受到的响应数据的格式
        });
    }
</script>
```

**3. JQeury实现方式：`$.get()`和`$.post()`**

```html
<script>
    function  fun() {
    $.get("ajaxServlet",{username:"rose"},function (data) {
                alert(data);
            },"text");
    }
</script>
```

```html
<script>
    function  fun() {
    $.post("ajaxServlet",{username:"rose"},function (data) {
                alert(data);
            },"text");
        }
</script>
```

**综上，推荐最后一种方式使用 AJAX 。**

### jQuery AJAX  方法

**1. jQuery load() 方法**

jQuery load() 方法是简单但强大的 AJAX 方法。load() 方法从服务器加载数据，并把返回的数据放入被选元素中。

语法

```
$(selector).load(URL,data,callback);
```

* 必需的 *URL* 参数规定您希望加载的 URL。
* 可选的 *data* 参数规定与请求一同发送的查询字符串键/值对集合。
* 可选的 *callback* 参数是 load() 方法完成后所执行的函数名称。

案例

```html
<script>
    $(document).ready(function(){
        $("button").click(function(){
            // 1. 把文件 "demo_test.txt" 的内容加载到指定的 <div> 元素中
            $("#div1").load("demo_test.txt");
            // 2. 把 "demo_test.txt" 文件中 id="p1" 的元素的内容，加载到指定的 <div> 元素中
            $("#div1").load("demo_test.txt #p1");
        });
    });
</script>
```

回调函数可以设置不同的参数：

* responseTxt - 包含调用成功时的结果内容
* statusTXT - 包含调用的状态
* xhr - 包含 [XMLHttpRequest 对象](https://www.w3cschool.cn/xml/xml-http.html)

案例：在 load() 方法完成后显示一个提示框。如果 load() 方法已成功，则显示"外部内容加载成功！"，而如果失败，则显示错误消息

```html
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").load("/statics/demosource/demo_test.txt",function(responseTxt,statusTxt,xhr){
      if(statusTxt=="success")
        alert("外部内容加载成功!");
      if(statusTxt=="error")
        alert("Error: "+xhr.status+": "+xhr.statusText);
    });
  });
});
</script>
```

> **提示：**在jQuery的load()方法中，无论AJAX请求是否成功，一旦请求完成（complete）后，回调函数（callback）立即被触发。

**2. jQuery get() 和 post() 方法详解** 

jQuery get() 和 post() 方法用于通过 HTTP GET 或 POST 请求从服务器请求数据。

**$.get() 方法通过 HTTP GET 请求从服务器上请求数据。**

```
$.get(URL,callback);
```

* 必需 URL 参数规定您希望请求的 URL。
* 可选 callback 参数是请求成功后所执行的函数名。
  * 第一个回调参数存有被请求页面的内容
  * 第二个回调参数存有请求的状态。

```html
<script>
    $("button").click(function(){
        //使用 $.get() 方法从服务器上的一个文件中取回数据
        $.get("demo_test.php",function(data,status){
            alert("数据: " + data + "\n状态: " + status);
        });
    });
</script>
```

这个 PHP 文件 ("demo_test.php") 类似这样：

```
 <?php
 echo "This is some text from an external PHP file.";
 ?>
```

**$.post() 方法通过 HTTP POST 请求从服务器上请求数据。**

```
$.post(URL,data,callback); 
```

* 必需 *URL* 参数规定您希望请求的 URL。
* 可选 *data* 参数规定连同请求发送的数据。
* 可选 *callback* 参数是请求成功后所执行的函数名。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <script src="//libs.baidu.com/jquery/1.10.2/jquery.min.js">
        </script>
        <script>
            $(document).ready(function(){
                $("button").click(function(){
                    $.post("/statics/demosource/demo_test_post.php",
          				{
                            name:"W3Cschool",
                            url:"http://www.w3cschool.cn"
                    	},
                        function(data,status){
                        	alert("数据: " + data + "状态: " + status);
                    });
                });
            });
        </script>
    </head>
    <body>
        <button>发送一个 HTTP POST 请求页面并获取返回内容</button>
    </body>
</html>
```

**提示：** 这个 PHP 文件 ("demo_test_post.php") 类似这样：

```
<?php
$name = isset($_POST['name']) ? htmlspecialchars($_POST['name']) : '';
$city = isset($_POST['city']) ? htmlspecialchars($_POST['city']) : '';
echo 'Dear ' . $name;
echo 'Hope you live well in ' . $city;
?>
```



## 9. jQuery noConflict() 方法

如何在页面上同时使用 jQuery 和其他框架？

要解决这个问题，只需要使用jQuery中的noConflict()方法，它允许你在同一个页面加载多个jQuery实例，尤其是不同版本的jQuery。

**jQuery 和其他 JavaScript 框架**

jQuery 使用 $ 符号作为 jQuery 的简写。

某些框架也使用 $ 符号作为简写（就像 jQuery），如果在用的两种不同的框架正在使用相同的简写符号，有可能导致脚本停止运行。

解决方法：

* noConflict() 方法会释放对 $ 标识符的控制，这样其他脚本就可以使用它了。
* 也可以通过全名替代简写的方式来使用 jQuery
    ```html
    <script>
        $.noConflict();
        jQuery(document).ready(function(){
            jQuery("button").click(function(){
                jQuery("p").text("jQuery is still working!");
            });
        });
    </script>
    ```

* 也可以创建自己的简写。noConflict() 可返回对 jQuery 的引用，把它存入变量，以供使用。

  ```html
  <script>
      var jq = $.noConflict();
      jq(document).ready(function(){
          jq("button").click(function(){
              jq("p").text("jQuery is still working!");
          });
      });
  </script>
  ```

* 可以把 `$` 符号作为变量传递给ready 方法这样就可以在函数内使用 `$` 符号了，而在函数外，依旧要使用 "jQuery"：

  ```html
  <script>
      $.noConflict();
      jQuery(document).ready(function($){
          $("button").click(function(){
              $("p").text("jQuery is still working!");
          });
      });
  </script>
  ```