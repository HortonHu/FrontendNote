# 指令 Directives


# 结构型指令
结构型指令通过添加、移除或替换 DOM 元素来修改布局。 

- 每个宿主元素上只能有一个结构型指令

### 星号(*)
星号(*)是简写方法，而这个字符串是一个微语法，而不是通常的模板表达式。 Angular 会解开这个语法糖，变成一个 `<ng-template>` 标记，包裹着宿主元素及其子元素。
```
<div *ngIf="hero" class="name">{{hero.name}}</div>

// 等价于

<ng-template [ngIf]="hero">
  <div class="name">{{hero.name}}</div>
</ng-template>
```
- 星号（*）语法比不带语法糖的形式更加清晰。 如果找不到单一的元素来应用该指令，可以使用<ng-container>作为该指令的容器。
- <ng-template>是一个 Angular 元素，用来渲染 HTML。 它永远不会直接显示出来。 事实上，在渲染视图之前，Angular 会把 <ng-template> 及其内容替换为一个注释。
- 如果没有使用结构型指令，而仅仅把一些别的元素包装进 <ng-template> 中，那些元素就是不可见的。


## *ngFor 
一个迭代器，它要求 Angular 为 heroes 列表中的每个英雄渲染出一个 `<li>`。

### 带 trackBy 的 *ngFor
- ngFor 指令有时候会性能较差，特别是在大型列表中。 对一个条目的一丁点改动、移除或添加，都会导致级联的 DOM 操作。
- 指定一个 trackBy，Angular 就可以避免这种折腾。 往组件中添加一个方法，它会返回 NgFor应该追踪的值。 


## *ngIf 
接受一个布尔值，并据此让一整块 DOM 树出现或消失。

- 隐藏子树和用 NgIf 排除子树是截然不同的。当隐藏子树时，它仍然留在 DOM 中。 子树中的组件及其状态仍然保留着。 即使对于不可见属性，Angular 也会继续检查变更。 子树可能占用相当可观的内存和运算资源。
- 当 NgIf 为 false 时，Angular 从 DOM 中物理地移除了这个元素子树。 它销毁了子树中的组件及其状态，也潜在释放了可观的资源，最终让用户体验到更好的性能。
- 显示/隐藏的技术对于只有少量子元素的元素是很好用的，但要当心别试图隐藏大型组件树。相比之下，NgIf 则是个更安全的选择。
- ngIf 指令通常会用来防范空指针错误。 
- NgIf 引用的是指令的类名，而 ngIf 引用的是指令的属性名*。


## *ngSwitch / *ngSwitchCase / *ngSwitchDefault
一组指令，用来在多个可选视图之间切换。

```
<div [ngSwitch]="hero?.emotion">
  <app-happy-hero    *ngSwitchCase="'happy'"    [hero]="hero"></app-happy-hero>
  <app-sad-hero      *ngSwitchCase="'sad'"      [hero]="hero"></app-sad-hero>
  <app-confused-hero *ngSwitchCase="'confused'" [hero]="hero"></app-confused-hero>
  <app-unknown-hero  *ngSwitchDefault           [hero]="hero"></app-unknown-hero>
</div>
```

# 属性型指令
属性型指令会修改现有元素的外观或行为。 在模板中，它们看起来就像普通的 HTML 属性一样，因此得名“属性型指令”。

## NgClass
通过绑定到 NgClass，可以同时添加或移除多个类。

## NgStyle
NgStyle 绑定可以同时设置多个内联样式。

## NgModel 
ngModel 指令就是属性型指令的一个例子，它实现了双向数据绑定。 ngModel 修改现有元素（一般是 <input>）的行为：设置其显示属性值，并响应 change 事件。

- 使用 ngModel 时需要 FormsModule
- 在使用 ngModel 指令进行双向数据绑定之前，你必须导入 FormsModule 并把它添加到 NgModule 的 imports 列表中。 
``` 
<input [(ngModel)]="hero.name">
```


## 创建一个简单的属性型指令
属性型指令至少需要一个带有 @Directive 装饰器的控制器类。该装饰器指定了一个用于标识属性的选择器。 控制器类实现了指令需要的指令行为。

如何创建一个简单的属性型指令 appHighlight ，当用户把鼠标悬停在一个元素上时，改变它的背景色。你可以这样用它：
```
<p appHighlight>Highlight me!</p>
```

- 在命令行窗口下用 CLI 命令 ng generate directive 创建指令类文件。
```
// src/app/highlight.directive.ts
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
    constructor(el: ElementRef) {
       el.nativeElement.style.backgroundColor = 'yellow';
    }
}
```


## 响应用户引发的事件
```
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) { }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

## 使用 @Input 数据绑定向指令传递值
高亮的颜色目前是硬编码在指令中的，这不够灵活。 在这一节中，你应该让指令的使用者可以指定要用哪种颜色进行高亮。

```
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {

  constructor(private el: ElementRef) { }

  @Input('appHighlight') highlightColor: string;

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor || 'red');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```





