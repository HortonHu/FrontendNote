# Start
## Step 1: Install
```
npm install --save @angular/material @angular/cdk @angular/animations
```

## Step 2: Configure
```
import {BrowserAnimationsModule} from '@angular/platform-browser/animations';

@NgModule({
  ...
  imports: [BrowserAnimationsModule],
  ...
})
export class PizzaPartyAppModule { }

```

## Step 3: Import
```
import {MatButtonModule} from '@angular/material/button';
import {MatCheckboxModule} from '@angular/material/checkbox';

@NgModule({
  ...
  imports: [MatButtonModule, MatCheckboxModule],
  ...
})
export class PizzaPartyAppModule { }
```

## Step 4: Include a theme
add this to your `styles.css`
```
@import "~@angular/material/prebuilt-themes/indigo-pink.css";

```

## Step 5: Gesture Support
```
npm install --save hammerjs
```

After installing, import it on your app's entry point (e.g. src/main.ts).

```
import 'hammerjs';
```


## Step 6 (Optional): Add Material Icons
If you want to use the mat-icon component with the official Material Design Icons, load the icon font in your index.html.

```
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```




