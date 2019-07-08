# 异步操作的模式
## 回调函数模式
如果f1是异步的，想要f2等到f1执行完成才能执行。把f2写成f1的回调函数
```
function f1(callback) {
  // ...
  callback();
}

function f2() {
  // ...
}

f1(f2);
```
- 优点是简单、容易理解和实现
- 缺点是不利于代码的阅读和维护，各个部分之间高度耦合（coupling），使得程序结构混乱、流程难以追踪（尤其是多个回调函数嵌套的情况），而且每个任务只能指定一个回调函数。



## 事件监听模式
异步任务的执行不取决于代码的顺序，而取决于某个事件是否发生。
```
f1.on('done', f2);

// 当f1发生done事件，就执行f2。然后，对f1进行改写：
function f1() {
  setTimeout(function () {
    // ...
    f1.trigger('done');
  }, 1000);
}
```
- 优点是比较容易理解，可以绑定多个事件，每个事件可以指定多个回调函数，而且可以“去耦合”（decoupling），有利于实现模块化。
- 缺点是整个程序都要变成事件驱动型，运行流程会变得很不清晰。阅读代码的时候，很难看出主流程。


## 发布/订阅模式
事件完全可以理解成“信号”，如果存在一个“信号中心”，某个任务执行完成，就向信号中心“发布”（publish）一个信号，其他任务可以向信号中心“订阅”（subscribe）这个信号，从而知道什么时候自己可以开始执行。这就叫做”发布/订阅模式”（publish-subscribe pattern），又称“观察者模式”（observer pattern）。


# 异步操作的流程控制
如果有多个异步操作，就存在一个流程控制的问题：如何确定异步操作执行的顺序，以及如何保证遵守这种顺序。

## 串行执行
我们可以编写一个流程控制函数，让它来控制异步任务，一个任务完成以后，再执行另一个。这就叫串行执行。函数series就是串行函数，它会依次执行异步任务，所有任务都完成后，才会执行final函数。items数组保存每一个异步任务的参数，results数组保存每一个异步任务的运行结果。
```
var items = [ 1, 2, 3, 4, 5, 6 ];
var results = [];

function async(arg, callback) {
  console.log('参数为 ' + arg +' , 1秒后返回结果');
  setTimeout(function () { callback(arg * 2); }, 1000);
}

function final(value) {
  console.log('完成: ', value);
}

function series(item) {
  if(item) {
    async( item, function(result) {
      results.push(result);
      return series(items.shift());
    });
  } else {
    return final(results[results.length - 1]);
  }
}

series(items.shift());
```


## 并行执行
流程控制函数也可以是并行执行，即所有异步任务同时执行，等到全部完成以后，才执行final函数。
```
var items = [ 1, 2, 3, 4, 5, 6 ];
var results = [];

function async(arg, callback) {
  console.log('参数为 ' + arg +' , 1秒后返回结果');
  setTimeout(function () { callback(arg * 2); }, 1000);
}

function final(value) {
  console.log('完成: ', value);
}

items.forEach(function(item) {
  async(item, function(result){
    results.push(result);
    if(results.length === items.length) {
      final(results[results.length - 1]);
    }
  })
});
```
forEach方法会同时发起六个异步任务，等到它们全部完成以后，才会执行final函数。


## 并行与串行的结合
所谓并行与串行的结合，就是设置一个门槛，每次最多只能并行执行n个异步任务，这样就避免了过分占用系统资源。
```
var items = [ 1, 2, 3, 4, 5, 6 ];
var results = [];
var running = 0;
var limit = 2;

function async(arg, callback) {
  console.log('参数为 ' + arg +' , 1秒后返回结果');
  setTimeout(function () { callback(arg * 2); }, 1000);
}

function final(value) {
  console.log('完成: ', value);
}

function launcher() {
  while(running < limit && items.length > 0) {
    var item = items.shift();
    async(item, function(result) {
      results.push(result);
      running--;
      if(items.length > 0) {
        launcher();
      } else if(running == 0) {
        final(results);
      }
    });
    running++;
  }
}

launcher();
```


# Promise
Promise 是一个对象，也是一个构造函数。
```
function f1(resolve, reject) {
  // 异步代码...
}

var p1 = new Promise(f1);
```
上面代码中，Promise构造函数接受一个回调函数f1作为参数，f1里面是异步操作的代码。然后，返回的p1就是一个 Promise 实例。

Promise 的设计思想是，所有异步任务都返回一个 Promise 实例。Promise 实例有一个then方法，用来指定下一步的回调函数。
```
var p1 = new Promise(f1);
p1.then(f2);
// f1的异步操作执行完成，就会执行f2
```


## Promise 对象的状态
Promise 实例具有三种状态。三种状态里面，fulfilled和rejected合在一起称为resolved（已定型）。
- 异步操作未完成（pending）
- 异步操作成功（fulfilled）
- 异步操作失败（rejected）

这三种的状态的变化途径只有两种。
- 从“未完成”到“成功”。异步操作成功，Promise 实例传回一个值（value），状态变为fulfilled。
- 从“未完成”到“失败”。异步操作失败，Promise 实例抛出一个错误（error），状态变为rejected。

JavaScript 提供原生的Promise构造函数，用来生成 Promise 实例。
```
var promise = new Promise(function (resolve, reject) {
  // ...

  if (/* 异步操作成功 */){
    resolve(value);
  } else { /* 异步操作失败 */
    reject(new Error());
  }
});
```
- Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己实现。
- `resolve`函数的作用是，将Promise实例的状态从“未完成”变为“成功”（即从pending变为fulfilled），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去。
- `reject`函数的作用是，将Promise实例的状态从“未完成”变为“失败”（即从pending变为rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
```
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100)
```


## Promise.prototype.then()
Promise 实例的then方法，用来添加回调函数。

then方法可以接受两个回调函数.一旦状态改变，就调用相应的回调函数。
- 第一个是异步操作成功时（变为fulfilled状态）的回调函数
- 第二个是异步操作失败（变为rejected）时的回调函数（该参数可以省略）。
```
var p1 = new Promise(function (resolve, reject) {
  resolve('成功');
});
p1.then(console.log, console.error);
// "成功"

var p2 = new Promise(function (resolve, reject) {
  reject(new Error('失败'));
});
p2.then(console.log, console.error);
// Error: 失败
```
上面代码中，p1和p2都是Promise 实例，它们的then方法绑定两个回调函数：成功时的回调函数console.log，失败时的回调函数console.error（可以省略）。p1的状态变为成功，p2的状态变为失败，对应的回调函数会收到异步操作传回的值，然后在控制台输出。

then方法可以链式使用。
```
p1
  .then(step1)
  .then(step2)
  .then(step3)
  .then(
    console.log,
    console.error
  );
```
- p1后面有四个then，意味依次有四个回调函数。只要前一步的状态变为fulfilled，就会依次执行紧跟在后面的回调函数。
- console.log只显示step3的返回值，而console.error可以显示p1、step1、step2、step3之中任意一个发生的错误。如果step1的状态变为rejected，那么step2和step3都不会执行了（因为它们是resolved的回调函数）。Promise 开始寻找，接下来第一个为rejected的回调函数，在上面代码中是console.error。这就是说，Promise 对象的报错具有传递性。


## then() 用法辨析
```
// 写法一
f1().then(function () {
  return f2();
});

// 写法二
f1().then(function () {
  f2();
});

// 写法三
f1().then(f2());

// 写法四
f1().then(f2);
```
为了便于讲解，下面这四种写法都再用then方法接一个回调函数f3。

写法一的f3回调函数的参数，是f2函数的运行结果。
```
f1().then(function () {
  return f2();
}).then(f3);
```

写法二的f3回调函数的参数是undefined。
```
f1().then(function () {
  f2();
  return;
}).then(f3);
```

写法三的f3回调函数的参数，是f2函数返回的函数的运行结果。
```
f1().then(f2())
  .then(f3);
```

写法四与写法一只有一个差别，那就是f2会接收到f1()返回的结果。
```
f1().then(f2)
  .then(f3);
```

## Promise优点缺点
- Promise 的优点在于，让回调函数变成了规范的链式写法，程序流程可以看得很清楚。它有一整套接口，可以实现许多强大的功能，比如同时执行多个异步操作，等到它们的状态都改变以后，再执行一个回调函数；再比如，为多个回调函数中抛出的错误，统一指定处理方法等等。
- Promise 还有一个传统写法没有的好处：它的状态一旦改变，无论何时查询，都能得到这个状态。这意味着，无论何时为 Promise 实例添加回调函数，该函数都能正确执行。所以，你不用担心是否错过了某个事件或信号。如果是传统写法，通过监听事件来执行回调函数，一旦错过了事件，再添加回调函数是不会执行的。
- Promise 的缺点是，编写的难度比传统写法高，而且阅读代码也不是一眼可以看懂。你只会看到一堆then，必须自己在then的回调函数里面理清逻辑。
