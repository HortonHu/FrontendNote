# 定义和调用
格式
```
function functionname()
{
    这里是要执行的代码
}
```

Example:
```
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}

var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
```
- 参数检查
```
if (typeof x !== 'number') {
    throw 'Not a number';
}
```
- JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数
- 传入的参数比定义的少也没有问题
- `arguments`它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。`arguments`类似`Array`但它不是一个`Array` 。`arguments`最常用于判断传入参数的个数
- ES6标准引入了`rest参数`为了获得额外的rest参数 `function foo(a, b, ...rest)` `rest`参数只能写在最后，前面用`...`标识


# 变量作用域与解构赋值
- 一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量
- JavaScript的函数可以嵌套，此时，内部函数可以访问外部函数定义的变量，反过来则不行
- 如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。
- JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部
- JavaScript引擎自动提升了变量的声明，但不会提升变量的赋值，最好在函数内部首先申明所有变量，最常见的做法是用一个var申明函数内部用到的所有变量
- JavaScript实际上只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报ReferenceError错误。
- 全局变量会绑定到window上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。
- 减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。
- 为了解决块级作用域，ES6引入了新的关键字let，用let替代var可以申明一个块级作用域的变量：
- 通常用全部大写的变量来表示“这是一个常量，不要修改它的值” 
- ES6标准引入了新的关键字`const`来定义常量，`const`与`let`都具有块级作用域：
- 在ES6中，可以使用解构赋值，直接对多个变量同时赋值：
```
var [x, y, z] = ['hello', 'JavaScript', 'ES6'];
```
- 解构赋值还可以忽略某些元素
```
let [, , z] = ['hello', 'JavaScript', 'ES6'];
```
- 如果需要从一个对象中取出若干属性，也可以使用解构赋值，便于快速获取对象的指定属性。
```
var {name, age, passport} = person;
```
- 对一个对象进行解构赋值时，同样可以直接对嵌套的对象属性进行赋值，只要保证对应的层次是一致的
```
var {name, address: {city, zip}} = person;
```
- 使用解构赋值对对象属性进行赋值时，如果对应的属性不存在，变量将被赋值为`undefined`，这和引用一个不存在的属性获得`undefined`是一致的。
- 如果要使用的变量名和属性名不一致，可以用
```
let {name, passport:id} = person;
```


# 方法
- 在一个对象中绑定函数，称为这个对象的方法。
- 如果以对象的方法形式调用，该函数的this指向被调用的对象。要保证this指向正确，必须用obj.xxx()的形式调用！
- 如果单独调用函数，此时，该函数的this指向全局对象，也就是window


# 表单验证
## 检查必填字段是否为空
```
function validate_required(field,alerttxt) {
    with (field) {
        if (value==null||value=="") {
            alert(alerttxt);return false
        } else {
            return true
        }
    }
}
```

## 检查email合法性
输入的数据必须包含 @ 符号和点号(.)。同时，@ 不可以是邮件地址的首字符，并且 @ 之后需有至少一个点号
```
function validate_email(field,alerttxt) {
    with (field) {
        apos=value.indexOf("@")
        dotpos=value.lastIndexOf(".")
        if (apos<1||dotpos-apos<2) {
            alert(alerttxt);return false
        } else {
            return true
        }
    }
}
```

# 高阶函数
一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。
- map/reduce
- filter
- sort


# map


# reduce



# filter



# sort


# 闭包


# 箭头函数


# generator
