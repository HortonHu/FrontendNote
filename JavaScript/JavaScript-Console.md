# console
console对象是 JavaScript 的原生对象，它有点像 Unix 系统的标准输出stdout和标准错误stderr，可以输出各种信息到控制台，并且还提供了很多有用的辅助方法。

## console 对象的静态方法
### console.log
- console.log方法用于在控制台输出信息。它可以接受一个或多个参数，将它们连接起来输出。
- console.log方法支持以下占位符，不同类型的数据必须使用对应的占位符。
```
%s 字符串
%d 整数
%i 整数
%f 浮点数
%o 对象的链接
%c CSS 格式字符串

var number = 11 * 9;
var color = 'red';
console.log('%d %s balloons', number, color);
// 99 red balloons

console.log(
  '%cThis text is styled!',
  'color: red; background: yellow; font-size: 24px;'
)
```
- console.info是console.log方法的别名，用法完全一样。只不过console.info方法会在输出信息的前面，加上一个蓝色图标。
- console.debug方法与console.log方法类似，会在控制台输出调试信息。但是，默认情况下，console.debug输出的信息不会显示，只有在打开显示级别在verbose的情况下，才会显示。
```
['log', 'info', 'warn', 'error'].forEach(function(method) {
  console[method] = console[method].bind(
    console,
    new Date().toISOString()
  );
});

console.log("出错了！");
// 2014-05-18T09:00.000Z 出错了！
```

### console.warn() & console.error()
warn方法和error方法也是在控制台输出信息，它们与log方法的不同之处在于，warn方法输出信息时，在最前面加一个黄色三角，表示警告；error方法输出信息时，在最前面加一个红色的叉，表示出错。同时，还会高亮显示输出文字和错误发生的堆栈。


### console.table()
对于某些复合类型的数据，console.table方法可以将其转为表格显示
```
var languages = [
  { name: "JavaScript", fileExtension: ".js" },
  { name: "TypeScript", fileExtension: ".ts" },
  { name: "CoffeeScript", fileExtension: ".coffee" }
];

console.table(languages);
```

### console.count()
count方法用于计数，输出它被调用了多少次。

### console.dir()，console.dirxml()
- dir方法用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示。
- dirxml方法主要用于以目录树的形式，显示 DOM 节点。


### console.assert()
console.assert方法主要用于程序运行过程中，进行条件判断，如果不满足条件，就显示一个错误，但不会中断程序执行。这样就相当于提示用户，内部状态不正确。它接受两个参数，第一个参数是表达式，第二个参数是字符串。只有当第一个参数为false，才会提示有错误，在控制台输出第二个参数，否则不会有任何结果
```
console.assert(list.childNodes.length < 500, '节点个数大于等于500')
```


### console.time()，console.timeEnd()
这两个方法用于计时，可以算出一个操作所花费的准确时间。参数是计时器的名称。调用timeEnd方法之后，控制台会显示“计时器名称: 所耗费的时间”。
```
console.time('Array initialize');

var array= new Array(1000000);
for (var i = array.length - 1; i >= 0; i--) {
  array[i] = new Object();
};

console.timeEnd('Array initialize');
// Array initialize: 1914.481ms
```


### console.group()，console.groupEnd()，console.groupCollapsed()
console.group和console.groupEnd这两个方法用于将显示的信息分组。它只在输出大量信息时有用，分在一组的信息，可以用鼠标折叠/展开。


### console.trace()，console.clear()
- console.trace方法显示当前执行的代码在堆栈中的调用路径。
- console.clear方法用于清除当前控制台的所有输出，将光标回置到第一行。如果用户选中了控制台的“Preserve log”选项，console.clear方法将不起作用。


## 命令行 API
- `$_` 属性返回上一个表达式的值。
- 控制台保存了最近5个在 Elements 面板选中的 DOM 元素，`$0`代表倒数第一个（最近一个），`$1`代表倒数第二个，以此类推直到`$4`
- `$(selector)`返回第一个匹配的元素，等同于`document.querySelector()`。注意，如果页面脚本对$有定义，则会覆盖原始的定义。比如，页面里面有 jQuery，控制台执行`$(selector)`就会采用 jQuery 的实现，返回一个数组。
- `$$(selector)`返回选中的 DOM 对象，等同于`document.querySelectorAll`。
- `$x(path)`方法返回一个数组，包含匹配特定 XPath 表达式的所有 DOM 元素。
- `inspect(object)`方法打开相关面板，并选中相应的元素，显示它的细节。DOM 元素在Elements面板中显示，比如`inspect(document)`会在 `Elements` 面板显示document元素。JavaScript 对象在控制台面板Profiles面板中显示，比如`inspect(window)`。
- `getEventListeners(object)`方法返回一个对象，该对象的成员为object登记了回调函数的各种事件（比如click或keydown），每个事件对应一个数组，数组的成员为该事件的回调函数。
- `keys(object)`方法返回一个数组，包含object的所有键名。`values(object)`方法返回一个数组，包含object的所有键值。
- `monitorEvents(object[, events])`方法监听特定对象上发生的特定事件。事件发生时，会返回一个Event对象，包含该事件的相关信息。`unmonitorEvents`方法用于停止监听。



## debugger
- debugger语句主要用于除错，作用是设置断点。如果有正在运行的除错工具，程序运行到debugger语句时会自动停下。如果没有除错工具，debugger语句不会产生任何结果，JavaScript 引擎自动跳过这一句。
- Chrome 浏览器中，当代码运行到debugger语句时，就会暂停运行，自动打开脚本源码界面。
```
for(var i = 0; i < 5; i++){
  console.log(i);
  if (i === 2) debugger;
}
```












