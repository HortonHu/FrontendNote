# 数据类型
## 特殊
- `==`    自动转换数据类型再比较，很多时候，会得到非常诡异的结果
- `===`  不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较。
- NaN这个特殊的Number与所有其他值都不相等，包括它自己。唯一能判断NaN的方法是通过isNaN()函数
- 比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值
- null表示一个“空”的值，它和0以及空字符串''不同，0是一个数值，''表示长度为0的字符串，而null表示“空”。undefined，它表示“未定义”。
- 大多数情况下，我们都应该用null。undefined仅仅在判断函数参数是否传递的情况下有用。
- JavaScript的数组可以包括任意数据类型。{1, 2,  3, true, 1.1, null, 'test'}
- 另一种创建数组的方法是通过Array()函数实现：new Array(1, 2, 3); 
- 申明一个变量用var语句, var x = 1; 查看变量的内容，可以用console.log(x)
- 布尔（逻辑）只能有两个值：true 或 false


## 数字
- JavaScript 只有一种数字类型。所有 JavaScript 数字均为 64 位。
- 整数（不使用小数点或指数计数法）最多为 15 位。小数的最大位数是 17，但是浮点运算并不总是 100% 准确
- 如果前缀为 0，则 JavaScript 会把数值常量解释为八进制数，如果前缀为 0 和 "x"，则解释为十六进制数。

### 数字属性和方法
#### 属性
- MAX VALUE
- MIN VALUE
- NEGATIVE INFINITIVE
- POSITIVE INFINITIVE
- NaN
- prototype
- constructor
#### 方法
- toExponential()
- toFixed()
- toPrecision()
- toString()
- valueOf()

Math的作用是：执行常见的算数任务。JavaScript 提供 8 种可被 Math 对象访问的算数值：
- 常数
- 圆周率
- 2 的平方根
- 1/2 的平方根
- 2 的自然对数
- 10 的自然对数
- 以 2 为底的 e 的对数
- 以 10 为底的 e 的对数

这是在 Javascript 中使用这些值的方法：（与上面的算数值一一对应）
- Math.E
- Math.PI
- Math.SQRT2
- Math.SQRT1_2
- Math.LN2
- Math.LN10
- Math.LOG2E
- Math.LOG10E

- round() 四舍五入
- random() 返回一个介于 0 和 1 之间的随机数
- max() 返回两个给定的数中的较大的数
- min() 返回两个给定的数中的较小的数


## 字符串
- 对于' " \需要用\来转义
- \x**表示十六进制
- \u####表示Unicode
- 用`...`可以表示多行字符串
- 字符串可以用+拼接
- 模板字符串
```
var name = "alice" 
var message = `hello ${name}`
```
字符串变量可以 length、下标取char、 toUpperCase、toLowerCase、indexOf、substring


## 数组
- 申明变量var arr = [1, 2, 3];
- arr.length如果赋值会发生变化， arr.length = 5 arr变成 {1, 2, 3, undefined, undefined}
- indexOf
- slice(start, end) 如果不传递参数，就拷贝了整个arr
- push
- pop 空arr不会报错，会返回undefined
- unshift 在arr头部添加元素
- shift删除头部元素
- sort
- reverse
- splice(index, count, ...)从指定的index开始删除若干个元素，可以再添加若干个元素
- concat拼接arr，返回一个新的arr，不限制arr的个数，并且会自动拆开arr
```
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```
- join(string)用string把arr中的每个元素都连起来
- 合并两个数组 - concat()


#  对象
- JavaScript 中的所有事物都是对象：字符串、数字、数组、日期，等等。在 JavaScript 中，对象是拥有属性和方法的数据。
- JavaScript的对象是一组由键-值组成的无序集合，例如：
```
var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
};
```
- 一个`{...}`表示一个对象
- 访问属性是通过.操作符完成的，但这要求属性名必须是一个有效的变量名。如果属性名包含特殊字符，就必须用``括起来，访问这个属性也无法使用.操作符，必须用`['xxx']`来访问
- JavaScript对象的所有属性都是字符串，不过属性对应的值可以是任意数据类型。
- 访问不存在的属性不报错，而是返回`undefined`
- 可以自由地给一个对象添加或删除属性 `x.xxx = xxxx  delete x.xxx`
- 检测是否拥有某一属性，可以用`in`操作符. 如果`in`判断一个属性存在，这个属性不一定是该对象的，它可能是该对象继承得到的
- 判断一个属性是否是对象自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法

## Map
API: get,set,delete
```
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get("Bob");
m.set("A", 1);
```
## Set
API: add, delete,
```
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
```


## iterable
ES6标准引入了新的iterable类型，Array、Map和Set都属于iterable类型。
- 用for ... of循环遍历集合 
```
var a = [1, 2, 3];
for (var x of a) {
}
```
- for ... in循环由于历史遗留问题，它遍历的实际上是对象的属性名称。一个Array数组实际上也是一个对象，它的每个元素的索引被视为一个属性。
```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    console.log(x); // '0', '1', '2', 'name'
}
```
- for ... of循环则完全修复了这些问题，它只循环集合本身的元素
```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
    console.log(x); // 'A', 'B', 'C'
}
```
- 更好的方式是直接使用iterable内置的forEach方法，它接收一个函数，每次迭代就自动回调该函数。



# 控制语句
## 判断
- JavaScript把null、undefined、0、NaN和空字符串''视为false，其他值一概视为true
```
if (...) {
    ...
else if (...) {
    ...
} else {
    ...
}
```

## 循环
```
for(...; ...; ...)
for (x in xx)
while
do ... while
```

# 输出
如果在文档已完成加载后执行 document.write，整个 HTML 页面将被覆盖
```
<script>
document.write("<p>我的第一段 JavaScript</p>");
</script>
```

# 注释
- 单行注释以 // 开头
- 多行注释以 /* 开始，以 */ 结尾


# 日期
Date 对象用于处理日期和时间。可以通过 new 关键词来定义 Date 对象。以下代码定义了名为 myDate 的 Date 对象：
```
var myDate=new Date() 
```
- `myDate.setFullYear(2008,7,9) `表示月份的参数介于 0 到 11 之间。也就是说，如果希望把月设置为 8 月，则参数应该是 7。
- 日期对象也可用于比较两个日期。


# 正则表达式RegExp
- RegExp 对象用于存储检索模式。
- 通过 new 关键词来定义 RegExp 对象。以下代码定义了名为 patt1 的 RegExp 对象，其模式是 "e".当您使用该 RegExp 对象在一个字符串中检索时，将寻找的是字符 "e"。
```
var patt1=new RegExp("e");
```

RegExp 对象有 3 个方法：test()、exec() 以及 compile()。
- test() 方法检索字符串中的指定值。返回值是 true 或 false。
- exec() 方法检索字符串中的指定值。返回值是被找到的值。如果没有发现匹配，则返回 null。
- compile() 方法用于改变 RegExp。 compile() 既可以改变检索模式，也可以添加或删除第二个参数。


# 计时
- setTimeout() 未来的某时执行代码
```
var t=setTimeout("javascript语句",xxx ms)
```
setTimeout() 方法会返回某个值。在上面的语句中，值被储存在名为 t 的变量中。假如你希望取消这个 setTimeout()，你可以使用这个变量名来指定它。
- clearTimeout() 取消setTimeout()
- clearTimeout(setTimeout_variable)


# Cookies


# Error
throw 语句允许我们创建自定义错误。正确的技术术语是：创建或抛出异常（exception）。
throw exception（自定义错误）
```
try
  {
  //在这里运行代码
  }
catch(err)
  {
  //在这里处理错误
  }
```


# OOP
- 创建对象
- 原型继承
- class继承



# 标准对象
- Data
- RegExp
- JSON



# 浏览器对象模型 (BOM)
## Window 对象
- 所有浏览器都支持 window 对象。它表示浏览器窗口。
- 所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。
- 全局变量是 window 对象的属性。
- 全局函数是 window 对象的方法。
- 甚至 HTML DOM 的 document 也是 window 对象的属性之一：`window.document.getElementById("header");`


## Window 尺寸
有三种方法能够确定浏览器窗口的尺寸（浏览器的视口，不包括工具栏和滚动条）。
- 对于Internet Explorer、Chrome、Firefox、Opera 以及 Safari：
```
window.innerHeight - 浏览器窗口的内部高度
window.innerWidth - 浏览器窗口的内部宽度
```
- 对于 Internet Explorer 8、7、6、5：
```
document.documentElement.clientHeight
document.documentElement.clientWidth
```
- 或者
```
document.body.clientHeight
document.body.clientWidth
```
- 实用的 JavaScript 方案（涵盖所有浏览器）：
```
var w=window.innerWidth
|| document.documentElement.clientWidth
|| document.body.clientWidth;

var h=window.innerHeight
|| document.documentElement.clientHeight
|| document.body.clientHeight;
```

### 其他 Window 方法
- `window.open()` - 打开新窗口
- `window.close()` - 关闭当前窗口
- `window.moveTo()` - 移动当前窗口
- `window.resizeTo()` - 调整当前窗口的尺寸



### window.screen对象
- `window.screen` 对象包含有关用户屏幕的信息。
- `window.screen` 对象在编写时可以不使用 window 这个前缀。
一些属性：
- `screen.availWidth` - 可用的屏幕宽度 属性返回访问者屏幕的宽度，以像素计，减去界面特性，比如窗口任务栏。
- `screen.availHeight` - 属性返回访问者屏幕的高度，以像素计，减去界面特性，比如窗口任务栏。



### Window Location
window.location 对象在编写时可不使用 window 这个前缀。一些例子：
- `location.hostname` 返回 web 主机的域名
- `location.pathname` 返回当前页面的路径和文件名
- `location.port` 返回 web 主机的端口 （80 或 443）
- `location.protocol` 返回所使用的 web 协议（http:// 或 https://）
- `location.href` 属性返回当前页面的 URL。
- `location.pathname` 属性返回 URL 的路径名。
- `location.assign()` 方法加载新的文档。



### Window History
window.history 对象在编写时可不使用 window 这个前缀。为了保护用户隐私，对 JavaScript 访问该对象的方法做出了限制。一些方法：
- `history.back()` - 与在浏览器点击后退按钮相同
- `history.forward()` - 与在浏览器中点击按钮向前相同



### Window Navigator
- `window.navigator` 对象包含有关访问者浏览器的信息。
- `window.navigator` 对象在编写时可不使用 window 这个前缀。来自 `navigator` 对象的信息具有误导性，不应该被用于检测浏览器版本，这是因为：`navigator` 数据可被浏览器使用者更改。浏览器无法报告晚于浏览器发布的新操作系统



### JavaScript 消息框
- 警告框 `alert`("文本")
- 确认框 `confirm`("文本") 如果用户点击确认，那么返回值为 true。如果用户点击取消，那么返回值为 false。
- 提示框 当提示框出现后，用户需要输入某个值，然后点击确认或取消按钮才能继续操纵。
























