# NGRX - ngrx/store

Key concepts
- **Actions** describe unique events that are dispatched from components and services.
- **State** changes are handled by pure functions called **reducers** that take the current state and the latest action to compute a new state.
- **Selectors** are pure functions used to select, derive and compose pieces of state.
- State accessed with the Store, an observable of state and an observer of actions.


## Action
Action Interface
```typescript
interface Action {
  type: string;
}
```
- type property is for describing the action that will be dispatched in your application
- The value of the type comes in the form of `[Source] Event`

Example:
```typescript
{
  type: '[Auth API] Login Success'
}
```


Rules:
- Upfront - write actions before developing features to understand and gain a shared knowledge of the feature being implemented.
- Divide - categorize actions based on the event source.
- Many - actions are inexpensive to write, so the more actions you write, the better you express flows in your application.
- Event-Driven - capture events not commands as you are separating the description of an event and the handling of that event.
- Descriptive - provide context that are targeted to a unique event with more detailed information you can use to aid in debugging with the developer tools.

Example:
```
// login-page.actions.ts
import { createAction, props } from '@ngrx/store';

export const login = createAction(
  '[Login Page] Login',
  props<{ username: string; password: string }>()
);

// login-page.component.ts
onSubmit(username: string, password: string) {
  store.dispatch(login({ username: username, password: password }));
}
```
The login action creator receives an object of username and password and returns a plain JavaScript object with a type property of `[Login Page]` Login, with username and password as additional properties.



  

## Reducer
- handling transitions from one state to the next state
- Reducer functions handle these transitions by determining which actions to handle based on the action's type.
- Reducers are pure functions in that they produce the same output for a given input. They are without side effects and handle each state transition synchronously.
- Each reducer function takes the latest Action dispatched, the current state, and determines whether to return a newly modified state or the original state.

### Reducer Function
Example:
```
// scoreboard-page.actions.ts
import { createAction } from '@ngrx/store';

export const homeScore = createAction('[Scoreboard Page] Home Score');
export const awayScore createAction('[Scoreboard Page] Away Score');
export const reset = createAction('[Scoreboard Page] Score Reset');


// scoreboard.reducer.ts
import { Action, createReducer, on } from '@ngrx/store';
import * as ScoreboardPageActions from '../actions/scoreboard-page.actions';

export interface State {
  home: number;
  away: number;
}

```

Setting the initial state
```
// scoreboard.reducer.ts
export const initialState: State = {
  home: 0,
  away: 0,
};

```

Creating the reducer function
```
const scoreboardReducer = createReducer(
  initialState,
  on(ScoreboardPageActions.homeScore, state => ({ ...state, home: state.home + 1 })),
  on(ScoreboardPageActions.awayScore, state => ({ ...state, away: state.away + 1 })),
  on(ScoreboardPageActions.resetScore, state => ({ home: 0, away: 0 }))
);

export function reducer(state: State | undefined, action: Action) {
  return scoreboardReducer(state, action);
}
```


### Registering root state
- The state of your application is defined as one large object. 
- Registering reducer functions to manage parts of your state only defines keys with associated values in the object.
- use the StoreModule.forRoot() method with a map of key/value pairs that define your state.
- Registering states with StoreModule.forRoot() ensures that the states are defined upon application startup.

```
// app.module.ts

import { NgModule } from '@angular/core';
import { StoreModule } from '@ngrx/store';
import * as fromScoreboard from './reducers/scoreboard.reducer';

@NgModule({
  imports: [
    StoreModule.forRoot({ game: fromScoreboard.reducer })
  ],
})
export class AppModule {}
```


### Register feature state
Feature states behave in the same way root states do, but allow you to define them with specific feature areas in your application. Your state is one large object, and feature states register additional keys and values in that object.




## Selectors
* Selectors are pure functions used for obtaining slices of store state
* Selectors provide many features when selecting slices of state.
    * Portable
    * Memoization
    * Composition
    * Testable
    * Type-safe

Using a selector for one piece of state
```
import { createSelector } from '@ngrx/store';

export interface FeatureState {
  counter: number;
}

export interface AppState {
  feature: FeatureState;
}

export const selectFeature = (state: AppState) => state.feature;

export const selectFeatureCount = createSelector(
  selectFeature,
  (state: FeatureState) => state.counter
);
```

Using selectors for multiple pieces of state
```
import { createSelector } from '@ngrx/store';

export interface User {
  id: number;
  name: string;
}

export interface Book {
  id: number;
  userId: number;
  name: string;
}

export interface AppState {
  selectedUser: User;
  allBooks: Book[];
}

export const selectUser = (state: AppState) => state.selectedUser;
export const selectAllBooks = (state: AppState) => state.allBooks;

export const selectVisibleBooks = createSelector(
  selectUser,
  selectAllBooks,
  (selectedUser: User, allBooks: Book[]) => {
    if (selectedUser && allBooks) {
      return allBooks.filter((book: Book) => book.userId === selectedUser.id);
    } else {
      return allBooks;
    }
  }
);
```


Using selectors with props
- To select a piece of state based on data that isn't available in the store you can pass props to the selector function. 
- For example if we have a counter and we want to multiply its value, we can add the multiply factor as a prop:
- a selector only keeps the previous input arguments in its cache.


```
// index.ts
export const getCount = createSelector(
  getCounterValue,
  (counter, props) => counter * props.multiply
);

// app.component.ts
ngOnInit() {
  this.counter = this.store.pipe(select(fromRoot.getCount, { multiply: 2 }))
}
```

using multiple counters differentiated by id
```
// index.ts
export const getCount = () =>
  createSelector(
    (state, props) => state.counter[props.id],
    (counter, props) => counter * props.multiply
  );

// app.component.ts
ngOnInit() {
  this.counter2 = this.store.pipe(select(fromRoot.getCount(), { id: 'counter2', multiply: 2 }));
  this.counter4 = this.store.pipe(select(fromRoot.getCount(), { id: 'counter4', multiply: 4 }));
  this.counter6 = this.store.pipe(select(fromRoot.getCount(), { id: 'counter6', multiply: 6 }));
}

```


### Selecting Feature States
The createFeatureSelector is a convenience method for returning a top level feature state. It returns a typed selector function for a feature slice of state.
```
import { createSelector, createFeatureSelector } from '@ngrx/store';

export interface FeatureState {
  counter: number;
}

export interface AppState {
  feature: FeatureState;
}

export const selectFeature = createFeatureSelector<AppState, FeatureState>('feature');

export const selectFeatureCount = createSelector(
  selectFeature,
  (state: FeatureState) => state.counter
);
```
The following selector below would not compile because foo is not a feature slice of AppState.
```
export const selectFeature = createFeatureSelector<AppState, FeatureState>('foo');
```


### Resetting Memoized Selectors

```
import { createSelector } from '@ngrx/store';

export interface State {
  counter1: number;
  counter2: number;
}

export const selectCounter1 = (state: State) => state.counter1;
export const selectCounter2 = (state: State) => state.counter2;
export const selectTotal = createSelector(
  selectCounter1,
  selectCounter2,
  (counter1, counter2) => counter1 + counter2
); // selectTotal has a memoized value of null, because it has not yet been invoked.

let state = { counter1: 3, counter2: 4 };

selectTotal(state); // computes the sum of 3 & 4, returning 7. selectTotal now has a memoized value of 7
selectTotal(state); // does not compute the sum of 3 & 4. selectTotal instead returns the memoized value of 7

state = { ...state, counter2: 5 };

selectTotal(state); // computes the sum of 3 & 5, returning 8. selectTotal now has a memoized value of 8
```

A selector's memoized value stays in memory indefinitely. If the memoized value is, for example, a large dataset that is no longer needed it's possible to reset the memoized value to null so that the large dataset can be removed from memory. This can be accomplished by invoking the release method on the selector.

```
selectTotal(state); // returns the memoized value of 8
selectTotal.release(); // memoized value of selectTotal is now null
```


Releasing a selector also recursively releases any ancestor selectors. Consider the following:
```
export interface State {
  evenNums: number[];
  oddNums: number[];
}

export const selectSumEvenNums = createSelector(
  (state: State) => state.evenNums,
  evenNums => evenNums.reduce((prev, curr) => prev + curr)
);
export const selectSumOddNums = createSelector(
  (state: State) => state.oddNums,
  oddNums => oddNums.reduce((prev, curr) => prev + curr)
);
export const selectTotal = createSelector(
  selectSumEvenNums,
  selectSumOddNums,
  (evenSum, oddSum) => evenSum + oddSum
);

selectTotal({
  evenNums: [2, 4],
  oddNums: [1, 3],
});

/**
 * Memoized Values before calling selectTotal.release()
 *   selectSumEvenNums  6
 *   selectSumOddNums   4
 *   selectTotal        10
 */

selectTotal.release();

/**
 * Memoized Values after calling selectTotal.release()
 *   selectSumEvenNums  null
 *   selectSumOddNums   null
 *   selectTotal        null
 */
```


### Advanced Usage
#### Breaking Down the Basics
Select a non-empty state using pipeable operators

Let's pretend we have a selector called selectValues and the component for displaying the data is only interested in defined values, i.e., it should not display empty states.

```
import { map, filter } from 'rxjs/operators';

store
  .pipe(
    map(state => selectValues(state)),
    filter(val => val !== undefined)
  )
  .subscribe(/* .. */);
```
further re-written to use the select() utility function from NgRx:
```
import { select } from '@ngrx/store';
import { map, filter } from 'rxjs/operators';

store
  .pipe(
    select(selectValues),
    filter(val => val !== undefined)
  )
  .subscribe(/* .. */);
```

Solution: Extracting a pipeable operator

To make the select() and filter() behaviour a re-usable piece of code, we extract a pipeable operator using the RxJS pipe() utility function:

```
import { select } from '@ngrx/store';
import { pipe } from 'rxjs';
import { filter } from 'rxjs/operators';

export const selectFilteredValues = pipe(
  select(selectValues),
  filter(val => val !== undefined)
);

store.pipe(selectFilteredValues).subscribe(/* .. */);
```

Advanced Example: Select the last {n} state transitions

```
// index.ts
export const selectProjectedValues = createSelector(
  selectFoo,
  selectBar,
  (foo, bar) => {
    if (foo && bar) {
      return { foo, bar };
    }

    return undefined;
  }
);

// select-last-state-transition.ts
// The number of state transitions is given by the user (subscriber)
export const selectLastStateTransitions = (count: number) => {

  return pipe(
    // Thanks to `createSelector` the operator will have memoization "for free"
    select(selectProjectedValues),
    // Combines the last `count` state values in array
    scan((acc, curr) => {
      return [ curr, acc[0], acc[1] ].filter(val => val !== undefined);
    }, [] as {foo: number; bar: string}[]) // XX: Explicit type hint for the array.
                                          // Equivalent to what is emitted by the selector
  );
}

// app.component.ts
// Subscribe to the store using the custom pipeable operator
store.pipe(selectLastStateTransitions(3)).subscribe(/* .. */);


```


## Meta-reducers
@ngrx/store composes your map of reducers into a single reducer.


Using a meta-reducer to log all actions
```
// app.module.ts
import { StoreModule, ActionReducer, MetaReducer } from '@ngrx/store';
import { reducers } from './reducers';

// console.log all actions
export function debug(reducer: ActionReducer<any>): ActionReducer<any> {
  return function(state, action) {
    console.log('state', state);
    console.log('action', action);

    return reducer(state, action);
  };
}

export const metaReducers: MetaReducer<any>[] = [debug];

@NgModule({
  imports: [
    StoreModule.forRoot(reducers, { metaReducers })
  ],
})
export class AppModule {}
```


## Using Dependency Injection
### Injecting Reducers
```
import { NgModule, InjectionToken } from '@angular/core';
import { StoreModule, ActionReducerMap } from '@ngrx/store';

import { SomeService } from './some.service';
import * as fromRoot from './reducers';

export const REDUCER_TOKEN = new InjectionToken<ActionReducerMap<fromRoot.State>>('Registered Reducers', {
  factory: () => {
    const serv = inject(SomeService);
    // return reducers synchronously
    return serv.getReducers();
  }
});

@NgModule({
  imports: [StoreModule.forRoot(REDUCER_TOKEN)]
})
export class AppModule {}
```

Reducers are also injected when composing state through feature modules.
```
import { NgModule, InjectionToken } from '@angular/core';
import { StoreModule, ActionReducerMap } from '@ngrx/store';

import * as fromFeature from './reducers';

export const FEATURE_REDUCER_TOKEN = new InjectionToken<
  ActionReducerMap<fromFeature.State>
>('Feature Reducers');

export function getReducers(): ActionReducerMap<fromFeature.State> {
  // map of reducers
  return {};
}

@NgModule({
  imports: [StoreModule.forFeature('feature', FEATURE_REDUCER_TOKEN)],
  providers: [
    {
      provide: FEATURE_REDUCER_TOKEN,
      useFactory: getReducers,
    },
  ],
})
export class FeatureModule {}
```


### Injecting Meta-Reducers
To inject 'middleware' meta reducers, use the META_REDUCERS injection token exported in the Store API and a Provider to register the meta reducers through dependency injection.

```
import { MetaReducer, META_REDUCERS } from '@ngrx/store';
import { SomeService } from './some.service';
import * as fromRoot from './reducers';

export function getMetaReducers(
  some: SomeService
): MetaReducer<fromRoot.State>[] {
  // return array of meta reducers;
}

@NgModule({
  providers: [
    {
      provide: META_REDUCERS,
      deps: [SomeService],
      useFactory: getMetaReducers,
    },
  ],
})
export class AppModule {}
```


### Injecting Feature Config
```
import { NgModule, InjectionToken } from '@angular/core';
import { StoreModule, StoreConfig } from '@ngrx/store';
import { SomeService } from './some.service';

import * as fromFeature from './reducers';

export const FEATURE_CONFIG_TOKEN = new InjectionToken<StoreConfig<fromFeature.State>>('Feature Config');

export function getConfig(someService: SomeService): StoreConfig<fromFeature.State> {
  // return the config synchronously.
  return {initialState: someService.getInitialState()};
}

@NgModule({
  imports: [StoreModule.forFeature('feature', fromFeature.reducers, FEATURE_CONFIG_TOKEN)],
  providers: [
    {
      provide: FEATURE_CONFIG_TOKEN,
      deps: [SomeService],
      useFactory: getConfig,
    },
  ],
})
export class FeatureModule {}
```



# @ngrx/store-devtools

# @ngrx/router-store

# @ngrx/entity

# @ngrx/data

# @ngrx/schematics


