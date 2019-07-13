# 管道
- 在很多不同的应用中，都在重复做出某些相同的变换。 
- 管道把数据作为输入，然后转换它，给出期望的输出。


```
import { Component } from '@angular/core';

@Component({
  selector: 'app-hero-birthday',
  template: `<p>The hero's birthday is {{ birthday | date }}</p>`
})
export class HeroBirthdayComponent {
  birthday = new Date(1988, 3, 15); // April 15, 1988
}
```


## 对管道进行参数化 Parameterizing a pipe
可以在管道名后面添加一个冒号( : )再跟一个参数值，来为管道添加参数(比如 `currency:'EUR'`)。 如果这个管道可以接受多个参数，那么就用冒号来分隔这些参数值(比如 `slice:1:5`)

```
<p>The hero's birthday is {{ birthday | date:"MM/dd/yy" }} </p>
```
参数值可以是任何有效的模板表达式（参见模板语法中的模板表达式部分），比如字符串字面量或组件的属性。 换句话说，借助属性绑定，你也可以像用绑定来控制生日的值一样，控制生日的显示格式。

来写第二个组件，它把管道的格式参数绑定到该组件的 format 属性。这里是新组件的模板
```
template: `
  <p>The hero's birthday is {{ birthday | date:format }}</p>
  <button (click)="toggleFormat()">Toggle Format</button>
`

export class HeroBirthday2Component {
  birthday = new Date(1988, 3, 15); // April 15, 1988
  toggle = true; // start with true == shortDate

  get format()   { return this.toggle ? 'shortDate' : 'fullDate'; }
  toggleFormat() { this.toggle = !this.toggle; }
}
```


## 链式管道 Chaining pipes
```
The chained hero's birthday is
{{ birthday | date | uppercase}}
```


## 自定义管道 Custom pipes
```
import { Pipe, PipeTransform } from '@angular/core';
/*
 * Raise the value exponentially
 * Takes an exponent argument that defaults to 1.
 * Usage:
 *   value | exponentialStrength:exponent
 * Example:
 *   {{ 2 | exponentialStrength:10 }}
 *   formats to: 1024
*/
@Pipe({name: 'exponentialStrength'})
export class ExponentialStrengthPipe implements PipeTransform {
  transform(value: number, exponent?: number): number {
    return Math.pow(value, isNaN(exponent) ? 1 : exponent);
  }
}
```
这个管道的定义中体现了几个关键点：
- 管道是一个带有“管道元数据(`pipe metadata`)”装饰器的类。
- 这个管道类实现了 `PipeTransform` 接口的 `transform` 方法，该方法接受一个输入值和一些可选参数，并返回转换后的值。`transform` 方法是管道的基本要素。 `PipeTransform`接口中定义了它，并用它指导各种工具和编译器。 理论上说，它是可选的。Angular 不会管它，而是直接查找并执行 `transform` 方法。
- 当每个输入值被传给 transform 方法时，还会带上另一个参数，比如你这个管道就有一个 `exponent`(放大指数) 参数。
- 可以通过 `@Pipe` 装饰器来告诉 `Angular`：这是一个管道。该装饰器是从 `Angular` 的 `core` 库中引入的。
- 这个 `@Pipe` 装饰器允许你定义管道的名字，这个名字会被用在模板表达式中。它必须是一个有效的 JavaScript 标识符。 比如，你这个管道的名字是 `exponentialStrength`。


## 管道与变更检测



## 纯(pure)管道与非纯(impure)管道
默认情况下，管道都是纯的。以前见到的每个管道都是纯的。 通过把它的 pure 标志设置为 false，你可以制作一个非纯管道。你可以像这样让 FlyingHeroesPipe 变成非纯的：

### 纯管道
- Angular 只有在它检测到输入值发生了纯变更时才会执行纯管道。 纯变更是指对原始类型值(String、Number、Boolean、Symbol)的更改， 或者对对象引用(Date、Array、Function、Object)的更改。
- Angular 会忽略(复合)对象内部的更改。 如果你更改了输入日期(Date)中的月份、往一个输入数组(Array)中添加新值或者更新了一个输入对象(Object)的属性，它都不会调用纯管道。
- 这可能看起来是一种限制，但它保证了速度。 对象引用的检查是非常快的(比递归的深检查要快得多)，所以 Angular 可以快速的决定是否应该跳过管道执行和视图更新。
- 因此，如果要和变更检测策略打交道，就会更喜欢用纯管道。 如果不能，你就可以转回到非纯管道。


### 非纯管道
- Angular 会在每个组件的变更检测周期中执行非纯管道。 非纯管道可能会被调用很多次，和每个按键或每次鼠标移动一样频繁。
- 要在脑子里绷着这根弦，必须小心翼翼的实现非纯管道。 一个昂贵、迟钝的管道将摧毁用户体验。


## 非纯 AsyncPipe
Angular 的 AsyncPipe 是一个有趣的非纯管道的例子。 AsyncPipe 接受一个 Promise 或 Observable 作为输入，并且自动订阅这个输入，最终返回它们给出的值。

AsyncPipe 管道是有状态的。 该管道维护着一个所输入的 Observable 的订阅，并且持续从那个 Observable 中发出新到的值。

```
import { Component } from '@angular/core';

import { Observable, interval } from 'rxjs';
import { map, take } from 'rxjs/operators';

@Component({
  selector: 'app-hero-message',
  template: `
    <h2>Async Hero Message and AsyncPipe</h2>
    <p>Message: {{ message$ | async }}</p>
    <button (click)="resend()">Resend</button>`,
})
export class HeroAsyncMessageComponent {
  message$: Observable<string>;

  private messages = [
    'You are my hero!',
    'You are the best hero!',
    'Will you be my hero?'
  ];

  constructor() { this.resend(); }

  resend() {
    this.message$ = interval(500).pipe(
      map(i => this.messages[i]),
      take(this.messages.length)
    );
  }
}
```

### 更多的非纯管道：一个向服务器发起 HTTP 请求的管道。
```
// src/app/fetch-json.pipe.ts
import { HttpClient }          from '@angular/common/http';
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'fetch',
  pure: false
})
export class FetchJsonPipe implements PipeTransform {
  private cachedData: any = null;
  private cachedUrl = '';

  constructor(private http: HttpClient) { }

  transform(url: string): any {
    if (url !== this.cachedUrl) {
      this.cachedData = null;
      this.cachedUrl = url;
      this.http.get(url).subscribe(result => this.cachedData = result);
    }

    return this.cachedData;
  }
}


// src/app/hero-list.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-hero-list',
  template: `
    <h2>Heroes from JSON File</h2>

    <div *ngFor="let hero of ('assets/heroes.json' | fetch) ">
      {{hero.name}}
    </div>

    <p>Heroes as JSON:
      {{'assets/heroes.json' | fetch | json}}
    </p>`
})
export class HeroListComponent { }
```

### 纯管道与纯函数
- 纯管道使用纯函数。 纯函数是指在处理输入并返回结果时，不会产生任何副作用的函数。 给定相同的输入，它们总是返回相同的输出。
- 在本章前面讨论的管道都是用纯函数实现的。 内置的 DatePipe 就是一个用纯函数实现的纯管道。 ExponentialStrengthPipe 是如此， FlyingHeroesComponent 也是如此。 不久前你刚看过的 FlyingHeroesImpurePipe 就是一个用纯函数实现的非纯管道。
- 但是一个纯管道必须总是用纯函数实现。忽略这个警告将导致失败并带来一大堆这样的控制台错误：表达式在被检查后被变更。


### 没有 FilterPipe 或者 OrderByPipe
Angular 不想提供这些管道，因为 
- 它们性能堪忧
- 它们会阻止比较激进的代码最小化(minification)。 无论是 filter 还是 orderBy 都需要它的参数引用对象型属性。 你在前面学过，这样的管道必然是非纯管道，并且 Angular 会在几乎每一次变更检测周期中调用非纯管道。

