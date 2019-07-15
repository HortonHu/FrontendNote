# Form
Angular 提供了两种不同的方法来通过表单处理用户输入：
- 响应式表单。响应式表单更健壮：它们的可扩展性、可复用性和可测试性更强。 如果表单是应用中的关键部分，或者你已经准备使用响应式编程模式来构建应用，请使用响应式表单。
- 模板驱动表单。模板驱动表单在往应用中添加简单的表单时非常有用，比如邮件列表的登记表单。它们很容易添加到应用中，但是不像响应式表单那么容易扩展。如果你有非常基本的表单需求和简单到能用模板管理的逻辑，请使用模板驱动表单。

|                    | REACTIVE                                  | TEMPLATE-DRIVEN                      |
|--------------------|-------------------------------------------|--------------------------------------|
| Setup (form model) | More explicit, created in component class | Less explicit, created by directives |
| Data model         | Structured                                | Unstructured                         |
| Predictability     | Synchronous                               | Asynchronous                         |
| Form validation    | Functions                                 | Directives                           |
| Mutability         | Immutable                                 | Mutable                              |
| Scalability        | Low-level API access                      | Abstraction on top of APIs           |


# Set up
## Setup in reactive forms
```typescript
import { Component } from '@angular/core';
import { FormControl } from '@angular/forms';
 
@Component({
  selector: 'app-reactive-favorite-color',
  template: `
    Favorite Color: <input type="text" [formControl]="favoriteColorControl">
  `
})
export class FavoriteColorComponent {
  favoriteColorControl = new FormControl('');
}
```


## Setup in template-driven forms
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-template-favorite-color',
  template: `
    Favorite Color: <input type="text" [(ngModel)]="favoriteColor">
  `
})
export class FavoriteColorComponent {
  favoriteColor = '';
}
```


# Data flow in forms
## Data flow in reactive forms

## Data flow in template-driven forms
- each form element is linked to a directive that manages the form model internally


# Form validation
- 响应式表单把自定义验证器定义成函数，它以要验证的控件作为参数。
- 模板驱动表单和模板指令紧密相关，并且必须提供包装了验证函数的自定义验证器指令。


# Testing
## Testing reactive forms


## Testing template-driven forms



# Mutability


# Scalability



