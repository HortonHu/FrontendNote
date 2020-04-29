# 变量声明
- let在很多方面与var是相似的，但是可以帮助大家避免在JavaScript里常见一些问题。
- const是对let的一个增强，它能阻止对一个变量再次赋值。


# let 声明
```
let hello = "Hello!";
```
## 块作用域
- 当用let声明一个变量，它使用的是词法作用域或块作用域。
- 不同于使用 var声明的变量那样可以在包含它们的函数外访问，块作用域变量在包含它们的块或for循环之外是不能访问的。

```
function f(input: boolean) {
    let a = 100;

    if (input) {
        // Still okay to reference 'a'
        let b = a + 1;
        return b;
    }

    // Error: 'b' doesn't exist here
    return b;
}
```

- 拥有块级作用域的变量的另一个特点是，它们不能在被声明之前读或写。 虽然这些变量始终“存在”于它们的作用域里，但在直到声明它的代码之前的区域都属于 暂时性死区。 它只是用来说明我们不能在let语句之前访问它们，幸运的是TypeScript可以告诉我们这些信息。


## 重定义及屏蔽
var声明时，它不在乎你声明多少次；你只会得到1个。let不能在1个作用域里多次声明
```
function f(x) {
    var x;
    var x;

    if (true) {
        var x;
    }
}

let x = 10;
let x = 20; // 错误，不能在1个作用域里多次声明`x`
```

在一个嵌套作用域里引入一个新名字的行为称做屏蔽。 它是一把双刃剑，它可能会不小心地引入新问题，同时也可能会解决一些错误。 通常来讲应该避免使用屏蔽，因为我们需要写出清晰的代码。 同时也有些场景适合利用它，你需要好好打算一下。
                                                             
## 块级作用域变量的获取


# const 声明
被赋值后不能再改变。 换句话说，它们拥有与 let相同的作用域规则，但是不能对它们重新赋值。

实际上const变量的内部状态是可修改的。
```
const numLivesForCat = 9;
const kitty = {
    name: "Aurora",
    numLives: numLivesForCat,
}

// Error
kitty = {
    name: "Danielle",
    numLives: numLivesForCat
};

// all "okay"
kitty.name = "Rory";
kitty.name = "Kitty";
kitty.name = "Cat";
kitty.numLives--;
```


# 解构
## 解构数组
```
let input = [1, 2];
let [first, second] = input;
console.log(first); // outputs 1
console.log(second); // outputs 2
```

解构作用于已声明的变量会更好：

```
// swap variables
[first, second] = [second, first];
```

作用于函数参数
```
function f([first, second]: [number, number]) {
    console.log(first);
    console.log(second);
}
f(input);
```

在数组里使用`...`语法创建剩余变量
```
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); // outputs 1
console.log(rest); // outputs [ 2, 3, 4 ]
```

尾随元素、或其它元素
```
let [first] = [1, 2, 3, 4];
console.log(first); // outputs 1

let [, second, , fourth] = [1, 2, 3, 4];
```


# 对象解构
```
let o = {
    a: "foo",
    b: 12,
    c: "bar"
};
let { a, b } = o;
```

用没有声明的赋值
```
({ a, b } = { a: "baz", b: 101 });
```
需要用括号将它括起来，因为Javascript通常会将以 { 起始的语句解析为一个块。

在对象里使用`...`语法创建剩余变量
```
let { a, ...passthrough } = o;
let total = passthrough.b + passthrough.c.length;
```

## 属性重命名
给属性以不同的名字
```
let { a: newName1, b: newName2 } = o;
```













