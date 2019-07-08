支持ES6的浏览器版本号

| Chrome 58   | Edge 14     | Firefox 54 | Safari 10 | Opera 55 |
| ----------- | ----------- | ---------- | --------- | ---------|
| Jan 2017    | Aug 2016    | Mar 2017   | Jul 2016  | Aug 2018 |


# ES6 to ES5
```
npm install --save-dev @babel/cli
npm install --save-dev @babel/core
npm install --save-dev @babel/preset-env

// edit ~/.babelrc
  {
    "presets": [
      "@babel/env",
    ],
    "plugins": []
  }
```
Babel 提供命令行工具@babel/cli，用于命令行转码。
```
# 转码结果输出到标准输出
$ npx babel example.js

# 转码结果写入一个文件
# --out-file 或 -o 参数指定输出文件
$ npx babel example.js --out-file compiled.js
# 或者
$ npx babel example.js -o compiled.js

# 整个目录转码
# --out-dir 或 -d 参数指定输出目录
$ npx babel src --out-dir lib
# 或者
$ npx babel src -d lib

# -s 参数生成source map文件
$ npx babel src -d lib -s
```

// TODO: more about babel
https://es6.ruanyifeng.com/#docs/intro


# let
ES6 新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。
```
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```
- for循环的计数器，就很合适使用let命令。
- 不存在变量提升.let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。
- 只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。称为“暂时性死区”（temporal dead zone，简称 TDZ）
 ```
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```
- let不允许在相同作用域内，重复声明同一个变量。

## 块级作用域
- let实际上为 JavaScript 新增了块级作用域。块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。
- ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。

                                                                        

# const
const声明一个只读的常量。一旦声明，常量的值就不能改变。
```
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```
- const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。
- 对于const来说，只声明不赋值，就会报错。
- const的作用域与let命令相同：只在声明所在的块级作用域内有效。
- const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。
- const声明的常量，也与let一样不可重复声明。
- const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。



# 变量的解构赋值
```
// 数组的解构赋值 
let [a, b, c] = [1, 2, 3];
// 默认值
let [foo = true] = [];
foo // true
```
- ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员严格等于undefined，默认值才会生效。如果一个数组成员是null，默认值就不会生效，因为null不严格等于undefined。


```
// 对象的解构赋值
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
foo // "aaa"
bar // "bbb
```
- 对象的属性没有次序，变量必须与属性同名，才能取到正确的值。


```
// 字符串的解构赋
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

// 数值和布尔值的解构赋值
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true


// 函数参数的解构赋
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```


# Exponentiation Operator(**)
The exponentiation operator (**) raises the first operand to the power of the second operand.
```
var x = 5;
var z = x ** 2;          // result is 25
```


# Default Parameter Values
```
function myFunction(x, y = 10) {
  // y is 10 if not passed or undefined
  return x + y;
}
myFunction(5); // will return 15
```


# Array.find()
The find() method returns the value of the first array element that passes a test function.
```
var numbers = [4, 9, 16, 25, 29];
var first = numbers.find(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```


# Array.findIndex()
The findIndex() method returns the index of the first array element that passes a test function.
```
var numbers = [4, 9, 16, 25, 29];
var first = numbers.findIndex(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```


# Number Properties
ES6 added the following properties to the Number object:
- EPSILON
- MIN_SAFE_INTEGER
- MAX_SAFE_INTEGER


# Number Methods
- Number.isInteger()
- Number.isSafeInteger()
```
Number.isInteger(10);        // returns true
Number.isInteger(10.5);      // returns false
```

A safe integer is an integer that can be exactly represented as a double precision number.
The Number.isSafeInteger() method returns true if the argument is a safe integer.
Safe integers are all integers from -(253 - 1) to +(253 - 1).
This is safe: 9007199254740991. This is not safe: 9007199254740992.
```
Number.isSafeInteger(10);    // returns true
Number.isSafeInteger(12345678901234567890);  // returns false
```


# Global Methods
- isFinite()
- isNaN()
The global isFinite() method returns false if the argument is Infinity or NaN.
```
isFinite(10/0);       // returns false
isFinite(10/1);       // returns true
```
The global isNaN() method returns true if the argument is NaN. Otherwise it returns false:
```
isNaN("Hello");       // returns true
```



# 函数的扩展
# Arrow Functions
Arrow functions allows a short syntax for writing function expressions.
You don't need the function keyword, the return keyword, and the curly brackets.
```
// ES5
var x = function(x, y) {
   return x * y;
}

// ES6
const x = (x, y) => x * y;
```
Arrow functions do not have their own this. They are not well suited for defining object methods.
Arrow functions are not hoisted. They must be defined before they are used.
Using const is safer than using var, because a function expression is always constant value.
You can only omit the return keyword and the curly brackets if the function is a single statement. Because of this, it might be a good habit to always keep them:
```
const x = (x, y) => { return x * y };
```






