# Autocomplete
```
<form class="example-form">
  <mat-form-field class="example-full-width">
    <input type="text" placeholder="Pick one" aria-label="Number" matInput [formControl]="myControl" [matAutocomplete]="auto">
    <mat-autocomplete #auto="matAutocomplete">
      <mat-option *ngFor="let option of options" [value]="option">
        {{option}}
      </mat-option>
    </mat-autocomplete>
  </mat-form-field>
</form>
```

## Adding a custom filter
## Setting separate control and display values
## Automatically highlighting the first option
## Autocomplete on a custom input element
## Attaching the autocomplete panel to a different element
## Option groups



# Checkbox
```
<mat-checkbox>Check me!</mat-checkbox>
```

## Checkbox label
## Click action config
This behavior can be customized by providing a new value of MAT_CHECKBOX_CLICK_ACTION to the checkbox.
```
providers: [
  {provide: MAT_CHECKBOX_CLICK_ACTION, useValue: 'check'}
]
```

## Theming
The color of a <mat-checkbox> can be changed by using the color property. By default, checkboxes use the theme's accent color. This can be changed to 'primary' or 'warn'.



# Datepicker
```
<mat-form-field>
  <input matInput [matDatepicker]="picker" placeholder="Choose a date">
  <mat-datepicker-toggle matSuffix [for]="picker"></mat-datepicker-toggle>
  <mat-datepicker #picker></mat-datepicker>
</mat-form-field>
```

## Connecting a datepicker to an input
## Setting the calendar starting view
## Watching the views for changes on selected years and months
## Setting the selected date
## Changing the datepicker colors
## Date validation
## Input and change events
## Disabling parts of the datepicker
## Touch UI mode
## Customizing the calendar header
## Localizing labels and messages
## Highlighting specific dates



# Form field
```
<div class="example-container">
  <mat-form-field>
    <input matInput placeholder="Input">
  </mat-form-field>

  <mat-form-field>
    <textarea matInput placeholder="Textarea"></textarea>
  </mat-form-field>

  <mat-form-field>
    <mat-select placeholder="Select">
      <mat-option value="option">Option</mat-option>
    </mat-select>
  </mat-form-field>
</div>
```
## Form field appearance variants
## Floating label
## Hint labels
## Error messages
## Prefix & suffix
## Custom form field controls
## Theming



# Input
```
<form class="example-form">
  <mat-form-field class="example-full-width">
    <input matInput placeholder="Favorite food" value="Sushi">
  </mat-form-field>

  <mat-form-field class="example-full-width">
    <textarea matInput placeholder="Leave a comment"></textarea>
  </mat-form-field>
</form>
```

## `<input>` and `<textarea>` attributes
All of the attributes that can be used with normal `<input>` and `<textarea>` elements can be used on elements inside `<mat-form-field>` as well. This includes Angular directives such as ngModel and formControl.

The only limitation is that the type attribute can only be one of the values supported by matInput.

The following input types can be used with `matInput`:
- color
- date
- datetime-local
- email
- month
- number
- password
- search
- tel
- text
- time
- url
- week


## Form field features
## Placeholder
## Changing when error messages are shown
## Auto-resizing <textarea> elements



# Radio button
```
// html
<label id="example-radio-group-label">Pick your favorite season</label>
<mat-radio-group
  aria-labelledby="example-radio-group-label"
  class="example-radio-group"
  [(ngModel)]="favoriteSeason">
  <mat-radio-button class="example-radio-button" *ngFor="let season of seasons" [value]="season">
    {{season}}
  </mat-radio-button>
</mat-radio-group>
<div>Your favorite season is: {{favoriteSeason}}</div>


// ts
import {Component} from '@angular/core';

/**
 * @title Radios with ngModel
 */
@Component({
  selector: 'radio-ng-model-example',
  templateUrl: 'radio-ng-model-example.html',
  styleUrls: ['radio-ng-model-example.css'],
})
export class RadioNgModelExample {
  favoriteSeason: string;
  seasons: string[] = ['Winter', 'Spring', 'Summer', 'Autumn'];
}
```
## Radio-button label
## Radio groups
## Default Color Configuration



# Select
```
<h4>Basic mat-select</h4>
<mat-form-field>
  <mat-label>Favorite food</mat-label>
  <mat-select>
    <mat-option *ngFor="let food of foods" [value]="food.value">
      {{food.viewValue}}
    </mat-option>
  </mat-select>
</mat-form-field>

<h4>Basic native select</h4>
<mat-form-field>
  <mat-label>Cars</mat-label>
  <select matNativeControl required>
    <option value="volvo">Volvo</option>
    <option value="saab">Saab</option>
    <option value="mercedes">Mercedes</option>
    <option value="audi">Audi</option>
  </select>
</mat-form-field>
```
## Getting and setting the select value
## Form field features
## Setting a static placeholder
## Disabling the select or individual options
## Resetting the select value
## Creating groups of options
## Multiple selection
## Customizing the trigger label
## Disabling the ripple effect
## Adding custom styles to the dropdown panel
## Changing when error messages are shown



# Slider
```
<mat-card>
  <mat-card-content>
    <h2 class="example-h2">Slider configuration</h2>

    <section class="example-section">
      <mat-form-field class="example-margin">
        <input matInput type="number" placeholder="Value" [(ngModel)]="value">
      </mat-form-field>
      <mat-form-field class="example-margin">
        <input matInput type="number" placeholder="Min value" [(ngModel)]="min">
      </mat-form-field>
      <mat-form-field class="example-margin">
        <input matInput type="number" placeholder="Max value" [(ngModel)]="max">
      </mat-form-field>
      <mat-form-field class="example-margin">
        <input matInput type="number" placeholder="Step size" [(ngModel)]="step">
      </mat-form-field>
    </section>

    <section class="example-section">
      <mat-checkbox class="example-margin" [(ngModel)]="showTicks">Show ticks</mat-checkbox>
      <mat-checkbox class="example-margin" [(ngModel)]="autoTicks" *ngIf="showTicks">
        Auto ticks
      </mat-checkbox>
      <mat-form-field class="example-margin" *ngIf="showTicks && !autoTicks">
        <input matInput type="number" placeholder="Tick interval" [(ngModel)]="tickInterval">
      </mat-form-field>
    </section>

    <section class="example-section">
      <mat-checkbox class="example-margin" [(ngModel)]="thumbLabel">Show thumb label</mat-checkbox>
    </section>

    <section class="example-section">
      <mat-checkbox class="example-margin" [(ngModel)]="vertical">Vertical</mat-checkbox>
      <mat-checkbox class="example-margin" [(ngModel)]="invert">Inverted</mat-checkbox>
    </section>

    <section class="example-section">
      <mat-checkbox class="example-margin" [(ngModel)]="disabled">Disabled</mat-checkbox>
    </section>

  </mat-card-content>
</mat-card>

<mat-card class="result">
  <mat-card-content>
    <h2 class="example-h2">Result</h2>

    <mat-slider
        class="example-margin"
        [disabled]="disabled"
        [invert]="invert"
        [max]="max"
        [min]="min"
        [step]="step"
        [thumbLabel]="thumbLabel"
        [tickInterval]="tickInterval"
        [(ngModel)]="value"
        [vertical]="vertical">
    </mat-slider>
  </mat-card-content>
</mat-card>


// ts
import {coerceNumberProperty} from '@angular/cdk/coercion';
import {Component} from '@angular/core';

/**
 * @title Configurable slider
 */
@Component({
  selector: 'slider-configurable-example',
  templateUrl: 'slider-configurable-example.html',
  styleUrls: ['slider-configurable-example.css'],
})
export class SliderConfigurableExample {
  autoTicks = false;
  disabled = false;
  invert = false;
  max = 100;
  min = 0;
  showTicks = false;
  step = 1;
  thumbLabel = false;
  value = 0;
  vertical = false;

  get tickInterval(): number | 'auto' {
    return this.showTicks ? (this.autoTicks ? 'auto' : this._tickInterval) : 0;
  }
  set tickInterval(value) {
    this._tickInterval = coerceNumberProperty(value);
  }
  private _tickInterval = 1;
}
```



# Slide toggle
```
<mat-card>
  <mat-card-content>
    <h2 class="example-h2">Slider configuration</h2>

    <section class="example-section">
      <label class="example-margin">Color:</label>
      <mat-radio-group [(ngModel)]="color">
        <mat-radio-button class="example-margin" value="primary">
          Primary
        </mat-radio-button>
        <mat-radio-button class="example-margin" value="accent">
          Accent
        </mat-radio-button>
        <mat-radio-button class="example-margin" value="warn">
          Warn
        </mat-radio-button>
      </mat-radio-group>
    </section>

    <section class="example-section">
      <mat-checkbox class="example-margin" [(ngModel)]="checked">Checked</mat-checkbox>
    </section>

    <section class="example-section">
      <mat-checkbox class="example-margin" [(ngModel)]="disabled">Disabled</mat-checkbox>
    </section>
  </mat-card-content>
</mat-card>

<mat-card class="result">
  <mat-card-content>
    <h2 class="example-h2">Result</h2>

    <section class="example-section">
      <mat-slide-toggle
          class="example-margin"
          [color]="color"
          [checked]="checked"
          [disabled]="disabled">
        Slide me!
      </mat-slide-toggle>
    </section>
  </mat-card-content>
</mat-card>

// ts
import {Component} from '@angular/core';

/**
 * @title Configurable slide-toggle
 */
@Component({
  selector: 'slide-toggle-configurable-example',
  templateUrl: 'slide-toggle-configurable-example.html',
  styleUrls: ['slide-toggle-configurable-example.css'],
})
export class SlideToggleConfigurableExample {
  color = 'accent';
  checked = false;
  disabled = false;
}

// css
.example-h2 {
  margin: 10px;
}

.example-section {
  display: flex;
  align-content: center;
  align-items: center;
  height: 60px;
}

.example-margin {
  margin: 10px;
}

```
