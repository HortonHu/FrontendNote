# 指令 Directives


# 结构型指令
结构型指令通过添加、移除或替换 DOM 元素来修改布局。 

## *ngFor 
一个迭代器，它要求 Angular 为 heroes 列表中的每个英雄渲染出一个 `<li>`。

### 带 trackBy 的 *ngFor
- ngFor 指令有时候会性能较差，特别是在大型列表中。 对一个条目的一丁点改动、移除或添加，都会导致级联的 DOM 操作。
- 指定一个 trackBy，Angular 就可以避免这种折腾。 往组件中添加一个方法，它会返回 NgFor应该追踪的值。 


## *ngIf 
条件语句，只有当选中的英雄存在时，它才会包含 HeroDetail 组件。

- 隐藏子树和用 NgIf 排除子树是截然不同的。当隐藏子树时，它仍然留在 DOM 中。 子树中的组件及其状态仍然保留着。 即使对于不可见属性，Angular 也会继续检查变更。 子树可能占用相当可观的内存和运算资源。
- 当 NgIf 为 false 时，Angular 从 DOM 中物理地移除了这个元素子树。 它销毁了子树中的组件及其状态，也潜在释放了可观的资源，最终让用户体验到更好的性能。
- 显示/隐藏的技术对于只有少量子元素的元素是很好用的，但要当心别试图隐藏大型组件树。相比之下，NgIf 则是个更安全的选择。
- ngIf 指令通常会用来防范空指针错误。 


## *ngSwitch 
一组指令，用来在多个可选视图之间切换。


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





