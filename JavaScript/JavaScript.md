# 变量
`var a = 1;`的形式来赋值变量
- 如果只是声明变量而没有赋值，则该变量的值是undefined。undefined是一个特殊的值，表示“无定义”。

## 变量scope
对于var命令来说，JavaScript 的区块不构成单独的作用域（scope）。
```
{
  var a = 1;
}

a // 1
```


## 变量提升
JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。
```
console.log(a);
var a = 1;
```
实际运行的是
```
var a;
console.log(a);
a = 1;
```
最后的结果是显示undefined，表示变量a已声明，但还未赋值。



# 数据类型
### JavaScript 的数据类型，共有六种。（ES6 又新增了第七种 Symbol 类型的值)
- 数值（number）：整数和小数（比如1和3.14）
- 字符串（string）：文本（比如Hello World）。
- 布尔值（boolean）：表示真伪的两个特殊值，即true（真）和false（假）
- undefined：表示“未定义”或不存在，即由于目前没有定义，所以此处暂时没有任何值
- null：表示空值，即此处的值为空。
- 对象（object）：各种值组成的集合。


### JavaScript 有三种方法，可以确定一个值到底是什么类型。
- typeof运算符
- instanceof运算符
- Object.prototype.toString方法

### typeof和instanceof的区别
- typeof运算符可以返回一个值的数据类型。
- instanceof运算符可以区分数组和对象。

```
// 数值、字符串、布尔值分别返回number、string、boolean。
typeof 123 // "number"
typeof '123' // "string"
typeof false // "boolean"

// 函数返回function。
function f() {}
typeof f
// "function"

// undefined返回undefined。
typeof undefined
// "undefined"

// 利用这一点，typeof可以用来检查一个没有声明的变量，而不报错。
v
// ReferenceError: v is not defined
typeof v
// "undefined"

// 对象返回object。
typeof window // "object"
typeof {} // "object"
typeof [] // "object" 空数组（[]）的类型也是object

// null返回object。
typeof null // "object"

```

### null 和 undefined
null是一个表示“空”的对象，转为数值时为0；undefined是一个表示"此处无定义"的原始值，转为数值时为NaN
- null表示空值，即该处的值现在为空。调用函数时，某个参数未设置任何值，这时就可以传入null，表示该参数为空。比如，某个函数接受引擎抛出的错误作为参数，如果运行过程中未出错，那么这个参数就会传入null，表示未发生错误。
- undefined表示“未定义”，下面是返回undefined的典型场景。
```
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的参数没有提供，该参数等于 undefined
function f(x) {
  return x;
}
f() // undefined

// 对象没有赋值的属性
var  o = new Object();
o.p // undefined

// 函数没有返回值时，默认返回 undefined
function f() {}
f() // undefined
```



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
- 布尔（逻辑）只能有两个值：true 或 false 只有以下六个是false 其他都是true。undefined/null/false/0/NaN/""或''（空字符串）


## Number
- JavaScript 只有一种数字类型。所有 JavaScript 数字均为 64 位。1与1.0是相同的，是同一个数。
```
1 === 1.0 // true
```
- 整数（不使用小数点或指数计数法）最多为 15 位。小数的最大位数是 17，但是浮点运算并不总是 100% 准确
- 如果前缀为 0，则 JavaScript 会把数值常量解释为八进制数，如果前缀为 0 和 "x"，则解释为十六进制数。
- JavaScript 内部实际上存在2个0：一个是+0，一个是-0，区别就是64位浮点数表示法的符号位不同。它们是等价的。唯一有区别的场合是，+0或-0当作分母，返回的值是不相等的。
- NaN是 JavaScript 的特殊值，表示“非数字”（Not a Number），主要出现在将字符串解析成数字出错的场合。 NaN不等于任何值，包括它本身。NaN与任何数（包括它自己）的运算，得到的都是NaN。
```
5 - 'x' // NaN
```                                            

### 方法
#### parseInt()
parseInt方法用于将字符串转为整数。
- 如果遇到不能转为数字的字符，就不再进行下去，返回已经转好的部分。
- 如果字符串的第一个字符不能转化为数字（后面跟着数字的正负号除外），返回NaN。parseInt的返回值只有两种可能，要么是一个十进制整数，要么是NaN。

#### parseFloat()
parseFloat方法用于将一个字符串转为浮点数。

#### isNaN()
isNaN方法可以用来判断一个值是否为NaN。

#### isFinite()
isFinite方法返回一个布尔值，表示某个值是否为正常的数值。



## 字符串
- 单引号字符串的内部，可以使用双引号。双引号字符串的内部，可以使用单引号。
- length属性返回字符串的长度，该属性也是无法改变的。
- 对于' " \需要用\来转义
- \x**表示十六进制
- \u####表示Unicode
- 用`...`可以表示多行字符串
- 字符串可以用+拼接
- 字符串可以被视为字符数组，因此可以使用数组的方括号运算符，用来返回某个位置的字符（位置编号从0开始）。
- 模板字符串
  
```
var name = "alice" 
var message = `hello ${name}`
```
字符串变量可以 length、下标取char、 toUpperCase、toLowerCase、indexOf、substring


### Base64 转码
文本里面包含一些不可打印的符号，比如 ASCII 码0到31的符号都无法打印出来，这时可以使用 Base64 编码，将它们转成可以打印的字符。另一个场景是，有时需要以文本格式传递二进制数据，那么也可以使用 Base64 编码。
- btoa()：任意值转为 Base64 编码
- atob()：Base64 编码转为原来的值
```
var string = 'Hello World!';
btoa(string) // "SGVsbG8gV29ybGQh"
atob('SGVsbG8gV29ybGQh') // "Hello World!"
```
这两个方法不适合非 ASCII 码的字符，会报错。要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用这两个方法。
```
function b64Encode(str) {
  return btoa(encodeURIComponent(str));
}

function b64Decode(str) {
  return decodeURIComponent(atob(str));
}

b64Encode('你好') // "JUU0JUJEJUEwJUU1JUE1JUJE"
b64Decode('JUU0JUJEJUEwJUU1JUE1JUJE') // "你好"
```    



## 数组
- 申明变量`var arr = [1, 2, 3]`;
- arr.length如果赋值会发生变化， `arr.length = 5` arr变成 `{1, 2, 3, undefined, undefined}` 
- 数组的空位计入length属性。length属性不过滤空位。数组最后一个成员后面有一个逗号，这不影响length属性的值。
```
var a = [1, , 1];
a.length // 3
```
- 使用数组的forEach方法、for...in结构、以及Object.keys方法进行遍历，空位都会被跳过。如果某个位置是undefined，遍历的时候就不会被跳过。
- `indexOf`
- `slice(start, end)` 如果不传递参数，就拷贝了整个arr
- `push`
- `pop` 空arr不会报错，会返回`undefined`
- `unshift` 在arr头部添加元素
- `shift`删除头部元素
- `sort`
- `reverse`
- `splice(index, count, ...)`从指定的index开始删除若干个元素，可以再添加若干个元素
- `concat`拼接arr，返回一个新的arr，不限制arr的个数，并且会自动拆开arr
```
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```
- `join(string)`用string把arr中的每个元素都连起来
- `Object.keys`方法返回数组的所有键名。可以看到数组的键名就是整数0、1、2。
- 清空数组的一个有效方法，就是将length属性设为0。
- 如果数组的键名是添加超出范围的数值，该键名会自动转为字符串。
- 数组的slice方法可以将“类似数组的对象”变成真正的数组。`var arr = Array.prototype.slice.call(arrayLike);`
```
// forEach 方法
function logArgs() {
  Array.prototype.forEach.call(arguments, function (elem, i) {
    console.log(i + '. ' + elem);
  });
}

// 等同于 for 循环
function logArgs() {
  for (var i = 0; i < arguments.length; i++) {
    console.log(i + '. ' + arguments[i]);
  }
}
```
- 字符串也是类似数组的对象，所以也可以用Array.prototype.forEach.call遍历。注意，这种方法比直接使用数组原生的forEach要慢，所以最好还是先将“类似数组的对象”转为真正的数组，然后再直接调用数组的forEach方法。
```
Array.prototype.forEach.call('abc', function (chr) {
  console.log(chr);
});
// a
// b
// c
```



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
- 对象的所有键名都是字符串（ES6 又引入了 Symbol 值也可以作为键名），所以加不加引号都可以。
- 一个`{...}`表示一个对象
- 如果不同的变量名指向同一个对象，那么它们都是这个对象的引用，也就是说指向同一个内存地址。修改其中一个变量，会影响到其他所有变量。
- 访问属性是通过.操作符完成的，但这要求属性名必须是一个有效的变量名。如果属性名包含特殊字符，就必须用``括起来，访问这个属性也无法使用.操作符，必须用`['xxx']`来访问
- JavaScript对象的所有属性都是字符串，不过属性对应的值可以是任意数据类型。
- 访问不存在的属性不报错，而是返回`undefined`
- 可以自由地给一个对象添加或删除属性 `x.xxx = xxxx  delete x.xxx`
- 检测是否拥有某一属性，可以用`in`操作符. 如果`in`判断一个属性存在，这个属性不一定是该对象的，它可能是该对象继承得到的
- 判断一个属性是否是对象自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法
- 查看一个对象本身的所有属性，可以使用Object.keys方法。
  
## with 语句
操作同一个对象的多个属性时，提供一些书写的方便。
```
with (对象) {
  语句;
}

with (document.links[0]){
  console.log(href);
  console.log(title);
  console.log(style);
}
// 等同于
console.log(document.links[0].href);
console.log(document.links[0].title);
console.log(document.links[0].style);
```


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


# 运算符
## 算术运算符
JS支持的算术运算符： + - * / ** % ++ -- 


## 比较运算符
- `>` 大于运算符
- `<` 小于运算符
- `<=` 小于或等于运算符
- `>=` 大于或等于运算符
- `==` 相等运算符
- `===` 严格相等运算符
- `!=` 不相等运算符
- `!==` 严格不相等运算符

### 非字符串的比较
如果两个运算子都是原始类型的值，则是先转成数值再比较。
```
5 > '4' // true
// 等同于 5 > Number('4')
// 即 5 > 4

true > false // true
// 等同于 Number(true) > Number(false)
// 即 1 > 0

2 > true // true
// 等同于 2 > Number(true)
// 即 2 > 1
```
- 这里需要注意与NaN的比较。任何值（包括NaN本身）与NaN比较，返回的都是false。
- 运算子是对象，会转为原始类型的值，再进行比较。对象转换成原始类型的值，算法是先调用valueOf方法；如果返回的还是对象，再接着调用toString方法
```
var x = [2];
x > '11' // true
// 等同于 [2].valueOf().toString() > '11'
// 即 '2' > '11'

x.valueOf = function () { return '1' };
x > '11' // false
// 等同于 [2].valueOf() > '11'
// 即 '1' > '11'
```

## 严格相等运算符
 相等运算符（==）比较两个值是否相等，严格相等运算符（===）比较它们是否为“同一个值”。如果两个值不是同一类型，严格相等运算符（===）直接返回false，而相等运算符（==）会将它们转换成同一个类型，再用严格相等运算符进行比较。对于严格相等运算符（===）：
- 如果两个值的类型不同，直接返回false。
- 同一类型的原始类型的值（数值、字符串、布尔值）比较时，值相同就返回true，值不同就返回false。
- 两个复合类型（对象、数组、函数）的数据比较时，不是比较它们的值是否相等，而是比较它们是否指向同一个地址。
- undefined和null与自身严格相等。

## 相等运算符
相等运算符用来比较相同类型的数据时，与严格相等运算符完全一样。
- 原始类型的值会转换成数值再进行比较。
- 对象（这里指广义的对象，包括数组和函数）与原始类型的值比较时，对象转换成原始类型的值，再进行比较。
- undefined和null与其他类型的值比较时，结果都为false，它们互相比较时结果为true。
- 相等运算符隐藏的类型转换，会带来一些违反直觉的结果。
```
0 == ''             // true
0 == '0'            // true

2 == true           // false
2 == false          // false

false == 'false'    // false
false == '0'        // true

false == undefined  // false
false == null       // false
null == undefined   // true

' \t\r\n ' == 0     // true
```  


## void 运算符
- void运算符的作用是执行一个表达式，然后不返回任何值，或者说返回undefined。
- 这个运算符的主要用途是浏览器的书签工具（Bookmarket），以及在超级链接中插入代码防止网页跳转。
```
<a href="javascript: void(f())">文字</a>

// 下面是一个更实际的例子，用户点击链接提交表单，但是不产生页面跳转。 
<a href="javascript: void(document.form.submit())">
 提交
</a>

```



# 控制语句
## 判断
### if 
JavaScript把null、undefined、0、NaN和空字符串''视为false，其他值一概视为true
```
if (...) {
    ...
else if (...) {
    ...
} else {
    ...
}
```
### switch 
- 每个case代码块内部的break语句不能少，否则会接下去执行下一个case代码块，而不是跳出switch结构。
```
switch (x) {
  case 1:
    console.log('x 等于1');
    break;
  case 2:
    console.log('x 等于2');
    break;
  default:
    console.log('x 等于其他值');
}
```
- switch语句后面的表达式，与case语句后面的表示式比较运行结果时，采用的是严格相等运算符（===），而不是相等运算符（==），这意味着比较时不会发生类型转换。
```
var x = 1;

switch (x) {
  case true:
    console.log('x 发生类型转换');
    break;
  default:
    console.log('x 没有发生类型转换');
}
// x 没有发生类型转换
```

### 三元运算符 ?:
(条件) ? 表达式1 : 表达式2


## 循环
- for(...; ...; ...)
- for (x in xx)
- while(...) {...}
- do ... while

## label
JavaScript 语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置，标签的格式如下。
```
label:
  语句
```
标签通常与break语句和continue语句配合使用，跳出特定的循环。
```
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
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
























