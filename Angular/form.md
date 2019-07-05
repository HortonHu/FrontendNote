# Form
- Form: reactive and template-driven.
- Reactive forms are more robust: they're more scalable, reusable, and testable. If forms are a key part of your application, or you're already using reactive patterns for building your application, use reactive forms.
- Template-driven forms are useful for adding a simple form to an app, such as an email list signup form. They're easy to add to an app, but they don't scale as well as reactive forms. If you have very basic form requirements and logic that can be managed solely in the template, use template-driven forms.

|                    | REACTIVE                                  | TEMPLATE-DRIVEN                      |
|--------------------|-------------------------------------------|--------------------------------------|
| Setup (form model) | More explicit, created in component class | Less explicit, created by directives |
| Data model         | Structured                                | Unstructured                         |
| Predictability     | Synchronous                               | Asynchronous                         |
| Form validation    | Functions                                 | Directives                           |
| Mutability         | Immutable                                 | Mutable                              |
| Scalability        | Low-level API access                      | Abstraction on top of APIs           |


## Set up
### Setup in reactive forms
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


### Setup in template-driven forms
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


## Data flow in forms
### Data flow in reactive forms

### Data flow in template-driven forms
- each form element is linked to a directive that manages the form model internally


## Form validation
- Reactive forms define custom validators as functions that receive a control to validate.
- Template-driven forms are tied to template directives, and must provide custom validator directives that wrap validation functions.


## Testing
### Testing reactive forms


### Testing template-driven forms



## Mutability


## Scalability



