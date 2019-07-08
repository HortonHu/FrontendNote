# 基础
## 使用jQuery
### 从本地加载
```
<head>
<script type="text/javascript" src="jquery.js"></script>
</head>
```

### 使用CDN加载
```
使用 Google 的 CDN
<head>
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs
/jquery/1.4.0/jquery.min.js"></script>
</head>

使用 Microsoft 的 CDN
<head>
<script type="text/javascript" src="http://ajax.microsoft.com/ajax/jquery
/jquery-1.4.min.js"></script>
</head>
```

## 语法
定义 `$(selector).action()`
- 美元符号定义 jQuery
- 选择符`（selector）`“查询”和“查找” HTML 元素
- jQuery 的 `action()` 执行对元素的操作

Example
- `$(this).hide()` - 隐藏当前元素
- `$("p").hide()` - 隐藏所有段落
- `$(".test").hide()` - 隐藏所有 `class="test"` 的所有元素
- `$("#test").hide()` - 隐藏所有 "`id="test` 的元素


## 文档就绪函数
把 jQuery 函数位于一个 `document ready` 函数中，为了防止文档在完全加载（就绪）之前运行 jQuery 代码。
```
$(document).ready(function(){

--- jQuery functions go here ----

});
```


## 选择器
### jQuery 元素选择器
- jQuery 使用 CSS 选择器来选取 HTML 元素。
- `$("p")` 选取 `<p>` 元素。
- `$("p.intro")` 选取所有 `class="intro"` 的 `<p>` 元素。
- `$("p#demo")` 选取所有 `id="demo"` 的 `<p>` 元素。

### jQuery 属性选择器
- jQuery 使用 XPath 表达式来选择带有给定属性的元素。
- `$("[href]")` 选取所有带有 href 属性的元素。
- `$("[href='#']")` 选取所有带有 `href` 值等于 `#` 的元素。
- `$("[href!='#']")` 选取所有带有 `href` 值不等于 `#` 的元素。
- `$("[href$='.jpg']")` 选取所有 `href` 值以 `.jpg` 结尾的元素。

### jQuery CSS 选择器
jQuery CSS 选择器可用于改变 HTML 元素的 CSS 属性。


## jQuery 名称冲突
- jQuery 使用 $ 符号作为 jQuery 的简介方式。
- 某些其他 JavaScript 库中的函数（比如 Prototype）同样使用 $ 符号。
- jQuery 使用名为 `noConflict()` 的方法来解决该问题。
- `var jq=jQuery.noConflict()`，帮助您使用自己的名称（比如 jq）来代替 $ 符号。


## jQuery代码维护
由于 jQuery 是为处理 HTML 事件而特别设计的，那么当您遵循以下原则时，您的代码会更恰当且更易维护：
- 把所有 jQuery 代码置于事件处理函数中
- 把所有事件处理函数置于文档就绪事件处理器中
- 把 jQuery 代码置于单独的 .js 文件中
- 如果存在名称冲突，则重命名 jQuery 库



## jQuery事件
Event 函数绑定函数至
- `$(document).ready(function)`将函数绑定到文档的就绪事件（当文档完成加载时）
- `$(selector).click(function)`触发或将函数绑定到被选元素的点击事件
- `$(selector).dblclick(function)`触发或将函数绑定到被选元素的双击事件
- `$(selector).focus(function)`触发或将函数绑定到被选元素的获得焦点事件
- `$(selector).mouseover(function)`触发或将函数绑定到被选元素的鼠标悬停事件


