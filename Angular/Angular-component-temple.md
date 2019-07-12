# 组件 component & 模板 template



# Component的组成
- `selector` 一旦在模板 HTML 中找到了这个选择器对应的标签，就创建并插入该组件的一个实例。
- `templateUrl` 该组件的 HTML 模板文件相对于这个组件文件的地址。 另外，你还可以用 `template` 属性的值来提供内联的 HTML 模板
- `styleUrls` 该组件的 `Style` 模板文件相对于这个组件文件的地址。 另外，你还可以内联的 `Style` 模板 或者直接为空
- `Providers`: 当前组件所需的服务的一个数组



# 模板语法
-  `<script>` 元素，它被禁用了
- 有些合法的 `HTML` 被用在模板中是没有意义的。`<html>、<body> 和 <base>` 元素这个舞台上中并没有扮演有用的角色。剩下的所有元素基本上就都一样用了。



# 组件之间的交互
## 通过输入型绑定把数据从父组件传到子组件
HeroChildComponent:
```
import { Component, Input } from '@angular/core';
 
import { Hero } from './hero';
 
@Component({
  selector: 'app-hero-child',
  template: `
    <h3>{{hero.name}} says:</h3>
    <p>I, {{hero.name}}, am at your service, {{masterName}}.</p>
  `
})
export class HeroChildComponent {
  @Input() hero: Hero;
  @Input('master') masterName: string;
}
```
HeroParentComponent:
```
import { Component } from '@angular/core';

import { HEROES } from './hero';

@Component({
  selector: 'app-hero-parent',
  template: `
    <h2>{{master}} controls {{heroes.length}} heroes</h2>
    <app-hero-child *ngFor="let hero of heroes"
      [hero]="hero"
      [master]="master">
    </app-hero-child>
  `
})
export class HeroParentComponent {
  heroes = HEROES;
  master = 'Master';
}
```
父组件 HeroParentComponent 把子组件的 HeroChildComponent 放到 *ngFor 循环器中，把自己的 master 字符串属性绑定到子组件的 master 别名上，并把每个循环的 hero 实例绑定到子组件的 hero 属性。


## 通过 setter 截听(Intercept)输入属性值的变化 
子组件 NameChildComponent 的输入属性 name 上的这个 setter，会 trim 掉名字里的空格，并把空值替换成默认字符串。
```
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-name-child',
  template: '<h3>"{{name}}"</h3>'
})
export class NameChildComponent {
  private _name = '';

  @Input()
  set name(name: string) {
    this._name = (name && name.trim()) || '<no name set>';
  }

  get name(): string { return this._name; }
}
```


## 通过ngOnChanges()来截听输入属性值的变化
- 使用 OnChanges 生命周期钩子接口的 ngOnChanges() 方法来监测输入属性值的变化并做出回应。
- 当需要监视多个、交互式输入属性的时候，本方法比用属性的 setter 更合适。
  
`VersionChildComponent`监测输入属性 major 和 minor 的变化，并把这些变化编写成日志以报告这些变化。
```
import { Component, Input, OnChanges, SimpleChange } from '@angular/core';

@Component({
  selector: 'app-version-child',
  template: `
    <h3>Version {{major}}.{{minor}}</h3>
    <h4>Change log:</h4>
    <ul>
      <li *ngFor="let change of changeLog">{{change}}</li>
    </ul>
  `
})
export class VersionChildComponent implements OnChanges {
  @Input() major: number;
  @Input() minor: number;
  changeLog: string[] = [];

  ngOnChanges(changes: {[propKey: string]: SimpleChange}) {
    let log: string[] = [];
    for (let propName in changes) {
      let changedProp = changes[propName];
      let to = JSON.stringify(changedProp.currentValue);
      if (changedProp.isFirstChange()) {
        log.push(`Initial value of ${propName} set to ${to}`);
      } else {
        let from = JSON.stringify(changedProp.previousValue);
        log.push(`${propName} changed from ${from} to ${to}`);
      }
    }
    this.changeLog.push(log.join(', '));
  }
}
```
`VersionParentComponent` 提供 minor 和 major 值，把修改它们值的方法绑定到按钮上。
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-version-parent',
  template: `
    <h2>Source code version</h2>
    <button (click)="newMinor()">New minor version</button>
    <button (click)="newMajor()">New major version</button>
    <app-version-child [major]="major" [minor]="minor"></app-version-child>
  `
})
export class VersionParentComponent {
  major = 1;
  minor = 23;

  newMinor() {
    this.minor++;
  }

  newMajor() {
    this.major++;
    this.minor = 0;
  }
}
```


## 父组件监听子组件的事件
- 子组件暴露一个 `EventEmitter` 属性，当事件发生时，子组件利用该属性 `emits`(向上弹射)事件。父组件绑定到这个事件属性，并在事件发生时作出回应。
- 子组件的 EventEmitter 属性是一个输出属性，通常带有@Output 装饰器，就像在 VoterComponent 中看到的。

```
import { Component, EventEmitter, Input, Output } from '@angular/core';

@Component({
  selector: 'app-voter',
  template: `
    <h4>{{name}}</h4>
    <button (click)="vote(true)"  [disabled]="didVote">Agree</button>
    <button (click)="vote(false)" [disabled]="didVote">Disagree</button>
  `
})
export class VoterComponent {
  @Input()  name: string;
  @Output() voted = new EventEmitter<boolean>();
  didVote = false;

  vote(agreed: boolean) {
    this.voted.emit(agreed);
    this.didVote = true;
  }
}
```
VoteTakerComponent 绑定了一个事件处理器(onVoted())，用来响应子组件的事件($event)并更新一个计数器。

```
import { Component }      from '@angular/core';

@Component({
  selector: 'app-vote-taker',
  template: `
    <h2>Should mankind colonize the Universe?</h2>
    <h3>Agree: {{agreed}}, Disagree: {{disagreed}}</h3>
    <app-voter *ngFor="let voter of voters"
      [name]="voter"
      (voted)="onVoted($event)">
    </app-voter>
  `
})
export class VoteTakerComponent {
  agreed = 0;
  disagreed = 0;
  voters = ['Narco', 'Celeritas', 'Bombasto'];

  onVoted(agreed: boolean) {
    agreed ? this.agreed++ : this.disagreed++;
  }
}
```


## 父组件与子组件通过本地变量互动
父组件不能使用数据绑定来读取子组件的属性或调用子组件的方法。但可以在父组件模板里，新建一个本地变量来代表子组件，然后利用这个变量来读取子组件的属性和调用子组件的方法

子组件 CountdownTimerComponent 进行倒计时，归零时发射一个导弹。start 和 stop 方法负责控制时钟并在模板里显示倒计时的状态信息。
```
import { Component, OnDestroy, OnInit } from '@angular/core';

@Component({
  selector: 'app-countdown-timer',
  template: '<p>{{message}}</p>'
})
export class CountdownTimerComponent implements OnInit, OnDestroy {

  intervalId = 0;
  message = '';
  seconds = 11;

  clearTimer() { clearInterval(this.intervalId); }

  ngOnInit()    { this.start(); }
  ngOnDestroy() { this.clearTimer(); }

  start() { this.countDown(); }
  stop()  {
    this.clearTimer();
    this.message = `Holding at T-${this.seconds} seconds`;
  }

  private countDown() {
    this.clearTimer();
    this.intervalId = window.setInterval(() => {
      this.seconds -= 1;
      if (this.seconds === 0) {
        this.message = 'Blast off!';
      } else {
        if (this.seconds < 0) { this.seconds = 10; } // reset
        this.message = `T-${this.seconds} seconds and counting`;
      }
    }, 1000);
  }
}
```
宿主组件 CountdownLocalVarParentComponent
```
import { Component }                from '@angular/core';
import { CountdownTimerComponent }  from './countdown-timer.component';

@Component({
  selector: 'app-countdown-parent-lv',
  template: `
  <h3>Countdown to Liftoff (via local variable)</h3>
  <button (click)="timer.start()">Start</button>
  <button (click)="timer.stop()">Stop</button>
  <div class="seconds">{{timer.seconds}}</div>
  <app-countdown-timer #timer></app-countdown-timer>
  `,
  styleUrls: ['../assets/demo.css']
})
export class CountdownLocalVarParentComponent { }
```
把本地变量(#timer)放到(<countdown-timer>)标签中，用来代表子组件。这样父组件的模板就得到了子组件的引用，于是可以在父组件的模板中访问子组件的所有属性和方法。


## 父组件调用@ViewChild()
- 本地变量方法是个简单便利的方法。但是它也有局限性，因为父组件-子组件的连接必须全部在父组件的模板中进行。父组件本身的代码对子组件没有访问权。
- 如果父组件的类需要读取子组件的属性值或调用子组件的方法.当父组件类需要这种访问时，可以把子组件作为 ViewChild，注入到父组件里面。

父组件 CountdownViewChildParentComponent
```
import { AfterViewInit, ViewChild } from '@angular/core';
import { Component }                from '@angular/core';
import { CountdownTimerComponent }  from './countdown-timer.component';

@Component({
  selector: 'app-countdown-parent-vc',
  template: `
  <h3>Countdown to Liftoff (via ViewChild)</h3>
  <button (click)="start()">Start</button>
  <button (click)="stop()">Stop</button>
  <div class="seconds">{{ seconds() }}</div>
  <app-countdown-timer></app-countdown-timer>
  `,
  styleUrls: ['../assets/demo.css']
})
export class CountdownViewChildParentComponent implements AfterViewInit {

  @ViewChild(CountdownTimerComponent, {static: false})
  private timerComponent: CountdownTimerComponent;

  seconds() { return 0; }

  ngAfterViewInit() {
    // Redefine `seconds()` to get from the `CountdownTimerComponent.seconds` ...
    // but wait a tick first to avoid one-time devMode
    // unidirectional-data-flow-violation error
    setTimeout(() => this.seconds = () => this.timerComponent.seconds, 0);
  }

  start() { this.timerComponent.start(); }
  stop() { this.timerComponent.stop(); }
}
```
额外的准备工作
- 必须导入对装饰器 ViewChild 以及生命周期钩子 AfterViewInit 的引用。
- 通过 @ViewChild 属性装饰器，将子组件 CountdownTimerComponent 注入到私有属性 timerComponent 里面。
- 组件元数据里就不再需要 #timer 本地变量了。而是把按钮绑定到父组件自己的 start 和 stop 方法，使用父组件的 seconds 方法的插值表达式来展示秒数变化。
- ngAfterViewInit() 生命周期钩子是非常重要的一步。被注入的计时器组件只有在 Angular 显示了父组件视图之后才能访问，所以它先把秒数显示为 0.
- 然后 Angular 会调用 ngAfterViewInit 生命周期钩子，但这时候再更新父组件视图的倒计时就已经太晚了。Angular 的单向数据流规则会阻止在同一个周期内更新父组件视图。应用在显示秒数之前会被迫再等一轮。
- 使用 setTimeout() 来等下一轮，然后改写 seconds() 方法，这样它接下来就会从注入的这个计时器组件里获取秒数的值。


## 父组件和子组件通过服务来通讯
父组件和它的子组件共享同一个服务，利用该服务在组件家族内部实现双向通讯。该服务实例的作用域被限制在父组件和其子组件内。这个组件子树之外的组件将无法访问该服务或者与它们通讯。

MissionService 把 MissionControlComponent 和多个 AstronautComponent 子组件连接起来。
```
import { Injectable } from '@angular/core';
import { Subject }    from 'rxjs';

@Injectable()
export class MissionService {

  // Observable string sources
  private missionAnnouncedSource = new Subject<string>();
  private missionConfirmedSource = new Subject<string>();

  // Observable string streams
  missionAnnounced$ = this.missionAnnouncedSource.asObservable();
  missionConfirmed$ = this.missionConfirmedSource.asObservable();

  // Service message commands
  announceMission(mission: string) {
    this.missionAnnouncedSource.next(mission);
  }

  confirmMission(astronaut: string) {
    this.missionConfirmedSource.next(astronaut);
  }
}
``` 
AstronautComponent 也通过自己的构造函数注入该服务。由于每个 AstronautComponent 都是 MissionControlComponent 的子组件，所以它们获取到的也是父组件的这个服务实例。
```
import { Component, Input, OnDestroy } from '@angular/core';

import { MissionService } from './mission.service';
import { Subscription }   from 'rxjs';

@Component({
  selector: 'app-astronaut',
  template: `
    <p>
      {{astronaut}}: <strong>{{mission}}</strong>
      <button
        (click)="confirm()"
        [disabled]="!announced || confirmed">
        Confirm
      </button>
    </p>
  `
})
export class AstronautComponent implements OnDestroy {
  @Input() astronaut: string;
  mission = '<no mission announced>';
  confirmed = false;
  announced = false;
  subscription: Subscription;

  constructor(private missionService: MissionService) {
    this.subscription = missionService.missionAnnounced$.subscribe(
      mission => {
        this.mission = mission;
        this.announced = true;
        this.confirmed = false;
    });
  }

  confirm() {
    this.confirmed = true;
    this.missionService.confirmMission(this.astronaut);
  }

  ngOnDestroy() {
    // prevent memory leak when component destroyed
    this.subscription.unsubscribe();
  }
}
```



# 组件样式
在组件的元数据中设置 styles 属性。 styles 属性可以接受一个包含 CSS 代码的字符串数组。 通常你只给它一个字符串就行了，如同下例：
```
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styles: ['h1 { font-weight: normal; }']
})
export class HeroAppComponent {
/* . . . */
}
```

- 既不会被模板中嵌入的组件继承，也不会被通过内容投影（如 ng-content）嵌进来的组件继承。


## 特殊的选择器
组件样式中有一些从影子(Shadow) DOM 样式范围领域（记录在W3C的CSS Scoping Module Level 1中） 引入的特殊选择器

### :host
使用 :host 伪类选择器，用来选择组件宿主元素中的元素（相对于组件模板内部的元素）。
```
:host {
  display: block;
  border: 1px solid black;
}
```
- :host 选择是是把宿主元素作为目标的唯一方式。除此之外，你将没办法指定它， 因为宿主不是组件自身模板的一部分，而是父组件模板的一部分。
- 要把宿主样式作为条件，就要像函数一样把其它选择器放在 :host 后面的括号中。下一个例子再次把宿主元素作为目标，但是只有当它同时带有 active CSS 类的时候才会生效。

```
:host(.active) {
  border-width: 3px;
}
```

### :host-context
- 有时候，基于某些来自组件视图外部的条件应用样式是很有用的。 例如，在文档的 <body> 元素上可能有一个用于表示样式主题 (theme) 的 CSS 类，你应当基于它来决定组件的样式。
- 这时可以使用 :host-context() 伪类选择器。它也以类似 :host() 形式使用。它在当前组件宿主元素的祖先节点中查找 CSS 类， 直到文档的根节点为止。在与其它选择器组合使用时，它非常有用。

只有当某个祖先元素有 CSS 类 theme-light 时，才会把 background-color 样式应用到组件内部的所有 <h2> 元素中。
```
:host-context(.theme-light) h2 {
  background-color: #eef;
}
```



# 自定义元素（Elements）
## 工作原理
使用 createCustomElement() 函数来把组件转换成一个可注册成浏览器中自定义元素的类。 注册完这个配置好的类之后，你就可以在内容中像内置 HTML 元素一样使用这个新元素了，比如直接把它加到 DOM 中：
```
<my-popup message="Use Angular!"></my-popup>
```
当你的自定义元素放进页面中时，浏览器会创建一个已注册类的实例。其内容是由组件模板提供的，它使用 Angular 模板语法，并且使用组件和 DOM 数据进行渲染。组件的输入属性（Property）对应于该元素的输入属性（Attribute）


## 把组件转换成自定义元素
- Angular 提供了 createCustomElement() 函数，以支持把 Angular 组件及其依赖转换成自定义元素。该函数会收集该组件的 Observable 型属性，提供浏览器创建和销毁实例时所需的 Angular 功能，还会对变更进行检测并做出响应。

### 映射
寄宿着 Angular 组件的自定义元素在组件中定义的"数据及逻辑"和标准的 DOM API 之间建立了一座桥梁。组件的属性和逻辑会直接映射到 HTML 属性和浏览器的事件系统中。

- 用于创建的 API 会解析该组件，以查找输入属性（Property），并在这个自定义元素上定义相应的属性（Attribute）。 它把属性名转换成与自定义元素兼容的形式（自定义元素不区分大小写），生成的属性名会使用中线分隔的小写形式。 比如，对于带有 @Input('myInputProp') inputProp 的组件，其对应的自定义元素会带有一个 my-input-prop 属性。
- 组件的输出属性会用 HTML 自定义事件的形式进行分发，自定义事件的名字就是这个输出属性的名字。 比如，对于带有 @Output() valueChanged = new EventEmitter() 属性的组件，其相应的自定义元素将会分发名叫 "valueChanged" 的事件，事件中所携带的数据存储在该事件对象的 detail 属性中。 如果你提供了别名，就改用这个别名。比如，@Output('myClick') clicks = new EventEmitter<string>(); 会导致分发名为 "myClick" 事件。



# 动态组件加载器
下面的例子展示了如何构建动态广告条。

## 辅助指令
AdDirective 的辅助指令来在模板中标记出有效的插入点。
```
import { Directive, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[ad-host]',
})
export class AdDirective {
  constructor(public viewContainerRef: ViewContainerRef) { }
}
```
- `AdDirective` 注入了 `ViewContainerRef` 来获取对容器视图的访问权，这个容器就是那些动态加入的组件的宿主。
- 在 @Directive 装饰器中，要注意选择器的名称：ad-host，它就是你将应用到元素上的指令。下一节会展示该如何做。


## 加载组件
`<ng-template>` 元素就是刚才制作的指令将应用到的地方。 要应用 AdDirective，回忆一下来自 ad.directive.ts 的选择器 `ad-host`。把它应用到 `<ng-template>`（不用带方括号）。 这下，Angular 就知道该把组件动态加载到哪里了。
```
template: `
            <div class="ad-banner-example">
              <h3>Advertisements</h3>
              <ng-template ad-host></ng-template>
            </div>
          `
```


## 解析组件
- AdBannerComponent 接收一个 AdItem 对象的数组作为输入，它最终来自 AdService。 AdItem 对象指定要加载的组件类，以及绑定到该组件上的任意数据。 AdService 可以返回广告活动中的那些广告。
- 给 AdBannerComponent 传入一个组件数组可以在模板中放入一个广告的动态列表，而不用写死在模板中。
- 通过 getAds() 方法，AdBannerComponent 可以循环遍历 AdItems 的数组，并且每三秒调用一次 loadComponent() 来加载新组件。

```
export class AdBannerComponent implements OnInit, OnDestroy {
  @Input() ads: AdItem[];
  currentAdIndex = -1;
  @ViewChild(AdDirective, {static: true}) adHost: AdDirective;
  interval: any;

  constructor(private componentFactoryResolver: ComponentFactoryResolver) { }

  ngOnInit() {
    this.loadComponent();
    this.getAds();
  }

  ngOnDestroy() {
    clearInterval(this.interval);
  }

  loadComponent() {
    this.currentAdIndex = (this.currentAdIndex + 1) % this.ads.length;
    let adItem = this.ads[this.currentAdIndex];

    let componentFactory = this.componentFactoryResolver.resolveComponentFactory(adItem.component);

    let viewContainerRef = this.adHost.viewContainerRef;
    viewContainerRef.clear();

    let componentRef = viewContainerRef.createComponent(componentFactory);
    (<AdComponent>componentRef.instance).data = adItem.data;
  }

  getAds() {
    this.interval = setInterval(() => {
      this.loadComponent();
    }, 3000);
  }
}
```

要想确保编译器照常生成工厂类，就要把这些动态加载的组件添加到 NgModule 的 entryComponents 数组中：
```
// src/app/app.module.ts (entry components)
entryComponents: [ HeroJobAdComponent, HeroProfileComponent ],
```


## 公共的 AdComponent 接口
```
// hero-job-ad.component.ts
import { Component, Input } from '@angular/core';

import { AdComponent }      from './ad.component';

@Component({
  template: `
    <div class="job-ad">
      <h4>{{data.headline}}</h4>

      {{data.body}}
    </div>
  `
})
export class HeroJobAdComponent implements AdComponent {
  @Input() data: any;

}

// hero-profile.component.ts
import { Component, Input }  from '@angular/core';

import { AdComponent }       from './ad.component';

@Component({
  template: `
    <div class="hero-profile">
      <h3>Featured Hero Profile</h3>
      <h4>{{data.name}}</h4>

      <p>{{data.bio}}</p>

      <strong>Hire this hero today!</strong>
    </div>
  `
})
export class HeroProfileComponent implements AdComponent {
  @Input() data: any;
}

// ad.component.ts
export interface AdComponent {
  data: any;
}
```
