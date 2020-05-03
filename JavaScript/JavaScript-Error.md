# Error
JavaScript 原生提供Error构造函数，所有抛出的错误都是这个构造函数的实例。
```
var err = new Error('出错了');
err.message // "出错了"
```
- message：错误提示信息
- name：错误名称（非标准属性）
- stack：错误的堆栈（非标准属性）

## 原生错误类型
6种派生错误，连同原始的Error对象，都是构造函数。开发者可以使用它们，手动生成错误对象的实例。这些构造函数都接受一个参数，代表错误提示信息（message）。

### SyntaxError
SyntaxError对象是解析代码时发生的语法错误。
```
// 变量名错误
var 1a;
// Uncaught SyntaxError: Invalid or unexpected token

// 缺少括号
console.log 'hello');
// Uncaught SyntaxError: Unexpected string
```

### ReferenceError
ReferenceError对象是引用一个不存在的变量时发生的错误。
```
// 使用一个不存在的变量
unknownVariable
// Uncaught ReferenceError: unknownVariable is not defined

// 另一种触发场景是，将一个值分配给无法分配的对象，比如对函数的运行结果或者this赋值。
// 等号左侧不是变量
console.log() = 1
// Uncaught ReferenceError: Invalid left-hand side in assignment

// this 对象不能手动赋值
this = 1
// ReferenceError: Invalid left-hand side in assignment
```

### RangeError
RangeError对象是一个值超出有效范围时发生的错误。主要有几种情况，一是数组长度为负数，二是Number对象的方法参数超出范围，以及函数堆栈超过最大值。
```
// 数组长度不得为负数
new Array(-1)
// Uncaught RangeError: Invalid array length
```

### TypeError
TypeError对象是变量或参数不是预期类型时发生的错误。比如，对字符串、布尔值、数值等原始类型的值使用new命令，就会抛出这种错误，因为new命令的参数应该是一个构造函数。
```
new 123
// Uncaught TypeError: number is not a func

var obj = {};
obj.unknownMethod()
// Uncaught TypeError: obj.unknownMethod is not a function
```

### URIError
URIError对象是 URI 相关函数的参数不正确时抛出的错误，主要涉及encodeURI()、decodeURI()、encodeURIComponent()、decodeURIComponent()、escape()和unescape()这六个函数。
```
decodeURI('%2')
// URIError: URI malformed
```

### EvalError
eval函数没有被正确执行时，会抛出EvalError错误。该错误类型已经不再使用了，只是为了保证与以前代码兼容，才继续保留。


## 自定义错误
自定义一个错误对象UserError，让它继承Error对象。然后，就可以生成这种自定义类型的错误了。
```
function UserError(message) {
  this.message = message || '默认信息';
  this.name = 'UserError';
}

UserError.prototype = new Error();
UserError.prototype.constructor = UserError;
```


## throw
throw语句允许我们创建自定义错误。正确的技术术语是：创建或抛出异常（exception）。
```
throw new xxxError
```


## try catch finally
```
try {
  //在这里运行代码
  throw new Error('出错了!');
} catch(err) {
  //在这里处理错误
} finally {
    ...
}

```

