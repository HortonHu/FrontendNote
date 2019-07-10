# 布尔值
```
let isDone: boolean = false;
```

# 数字
TypeScript里的所有数字都是浮点数。 这些浮点数的类型是 number。 除了支持十进制和十六进制字面量，TypeScript还支持ECMAScript 2015中引入的二进制和八进制字面量。
```
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

# 字符串
```
let name: string = "bob";
name = "smith";
```

模版字符串，它可以定义多行文本和内嵌表达式。 这种字符串是被反引号包围（ `），并且以${ expr }这种形式嵌入表达式
```
let name: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ name }.

I'll be ${ age + 1 } years old next month.`;
```

# 数组
有两种方式可以定义数组。 
- 第一种，可以在元素类型后面接上 []，表示由此类型元素组成的一个数组
- 第二种方式是使用数组泛型，Array<元素类型>
```
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

# 元组 Tuple
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 
```
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

# 枚举
使用枚举类型可以为一组数值赋予友好的名字。
```
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

# Any
还不清楚类型的变量指定一个类型。

any类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。 你可能认为 Object有相似的作用，就像它在其它语言中那样。 但是 Object类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法：
```
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```
当你只知道一部分数据的类型时，any类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：

```
let list: any[] = [1, true, "free"];

list[1] = 100;
```

# Void
某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 void：
```
function warnUser(): void {
    console.log("This is my warning message");
}
```
声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null：


# Null 和 Undefined
undefined和null两者各自有自己的类型分别叫做undefined和null。默认情况下null和undefined是所有类型的子类型。 就是说你可以把 null和undefined赋值给number类型的变量。
                                           
# Never
- never类型表示的是那些永不存在的值的类型。 例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。
- never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never。
```
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

# Object
object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。
```
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

# 类型断言
通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。
```
//  其一是“尖括号”语法：
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

// 另一个为as语法：
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```
