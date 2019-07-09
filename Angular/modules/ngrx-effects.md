# @ngrx/effects
effects provide a way to interact with those services and isolate them from the components.
- Effects are an RxJS powered side effect model for Store. Effects use streams to provide new sources of actions to reduce state based on external interactions such as network requests, web socket messages and time-based events.
- Effects are where you handle tasks such as fetching data, long-running tasks that produce multiple events, and other external interactions where your components don't need explicit knowledge of these interactions.

# Key Concepts:
- Effects isolate side effects from components, allowing for more pure components that select state and dispatch actions.
- Effects are long-running services that listen to an observable of every action dispatched from the Store.
- Effects filter those actions based on the type of action they are interested in. This is done by using an operator.
- Effects perform tasks, which are synchronous or asynchronous and return a new action.


# Comparison with component-based side effects
Here is a component that fetches and displays a list of movies.
```
// movies-page.component.ts
@Component({
  template: `
    <li *ngFor="let movie of movies">
      {{ movie.name }}
    </li>
  `
})
export class MoviesPageComponent {
  movies: Movie[];

  constructor(private movieService: MoviesService) {}

  ngOnInit() {
    this.movieService.getAll().subscribe(movies => this.movies = movies);
  }
}

// corresponding service that handles the fetching of movies.
// movies.service.ts
@Injectable({
  providedIn: 'root'
})
export class MoviesService {
  constructor (private http: HttpClient) {}

  getAll() {
    return this.http.get('/movies');
  }
}
```
Effects when used along with Store, decrease the responsibility of the component.

Effects handle external data and interactions, allowing your services to be less stateful and only perform tasks related to external interactions. Next, refactor the component to put the shared movie data in the `Store`. Effects handle the fetching of movie data.


```
// movies-page.component.ts
@Component({
  template: `
    <div *ngFor="let movie of movies$ | async">
      {{ movie.name }}
    </div>
  `
})
export class MoviesPageComponent {
  movies$: Observable = this.store.select(state => state.movies);

  constructor(private store: Store<{ movies: Movie[] }>) {}

  ngOnInit() {
    this.store.dispatch({ type: '[Movies Page] Load Movies' });
  }
}
```
The movies are still fetched through the MoviesService, but the component is no longer concerned with how the movies are fetched and loaded. It's only responsible for declaring its intent to load movies and using selectors to access movie list data. Effects are where the asynchronous activity of fetching movies happens. Your component becomes easier to test and less responsible for the data it needs.


## Writing Effects

```
// movie.effects.ts
import { Injectable } from '@angular/core';
import { Actions, Effect, ofType } from '@ngrx/effects';
import { EMPTY } from 'rxjs';
import { map, mergeMap, catchError } from 'rxjs/operators';
import { MoviesService } from './movies.service';

@Injectable()
export class MovieEffects {

  loadMovies$ = createEffect(() => this.actions$.pipe(
    ofType('[Movies Page] Load Movies'),
    mergeMap(() => this.moviesService.getAll()
      .pipe(
        map(movies => ({ type: '[Movies API] Movies Loaded Success', payload: movies })),
        catchError(() => EMPTY)
      ))
    )
  );

  constructor(
    private actions$: Actions,
    private moviesService: MoviesService
  ) {}
}
```
The loadMovies$ effect is listening for all dispatched actions through the Actions stream, but is only interested in the `[Movies Page]` Load Movies event using the ofType operator. The stream of actions is then flattened and mapped into a new observable using the mergeMap operator. The MoviesService#getAll() method returns an observable that maps the movies to a new action on success, and currently returns an empty observable if an error occurs. The action is dispatched to the Store where it can be handled by reducers when a state change is needed. Its also important to handle errors when dealing with observable streams so that the effects continue running.



## Handling Errors
```
// movie.effects.ts
import { Injectable } from '@angular/core';
import { Actions, Effect, ofType } from '@ngrx/effects';
import { of } from 'rxjs';
import { map, mergeMap, catchError } from 'rxjs/operators';
import { MoviesService } from './movies.service';

@Injectable()
export class MovieEffects {

  loadMovies$ = createEffect(() =>
    this.actions$.pipe(
      ofType('[Movies Page] Load Movies'),
      mergeMap(() => this.moviesService.getAll()
        .pipe(
          map(movies => ({ type: '[Movies API] Movies Loaded Success', payload: movies })),
          catchError(() => of({ type: '[Movies API] Movies Loaded Error' }))
        )
      )
    )
  );

  constructor(
    private actions$: Actions,
    private moviesService: MoviesService
  ) {}
}
```


## Registering root effects
After you've written your Effects class, you must register it so the effects start running. To register root-level effects, add the `EffectsModule.forRoot()` method with an array of your effects to your AppModule.
```
// app.module.ts
import { EffectsModule } from '@ngrx/effects';
import { MovieEffects } from './effects/movie.effects';

@NgModule({
  imports: [
    EffectsModule.forRoot([MovieEffects])
  ],
})
export class AppModule {}
```
- The EffectsModule.forRoot() method must be added to your AppModule imports even if you don't register any root-level effects.
- Effects start running immediately after the AppModule is loaded to ensure they are listening for all relevant actions as soon as possible.



## Registering feature effects
For feature modules, register your effects by adding the `EffectsModule.forFeature()` method in the imports array of your `NgModule`.
```
// admin.module.ts
import { EffectsModule } from '@ngrx/effects';
import { MovieEffects } from './effects/movie.effects';

@NgModule({
  imports: [
    EffectsModule.forFeature([MovieEffects])
  ],
})
export class MovieModule {}
```


## Incorporating State
If additional metadata is needed to perform an effect besides the initiating action's type, we should rely on passed metadata from an action creator's props method.

```
// login-page.actions.ts
import { createAction, props } from '@ngrx/store';
import { Credentials } from '../models/user';

export const login = createAction(
  '[Login Page] Login',
  props<{ credentials: Credentials }>()
);
```

```
// auth.effects.ts
import { Injectable } from '@angular/core';
import { Actions, ofType, createEffect } from '@ngrx/effects';
import { of } from 'rxjs';
import { catchError, exhaustMap, map } from 'rxjs/operators';
import {
  LoginPageActions,
  AuthApiActions,
} from '../actions';
import { Credentials } from '../models/user';
import { AuthService } from '../services/auth.service';

@Injectable()
export class AuthEffects {
  login$ = createEffect(() =>
    this.actions$.pipe(
      ofType(LoginPageActions.login),
      exhaustMap(action =>
        this.authService.login(action.credentials).pipe(
          map(user => AuthApiActions.loginSuccess({ user })),
          catchError(error => of(AuthApiActions.loginFailure({ error })))
        )
      )
    )
  );

  constructor(
    private actions$: Actions,
    private authService: AuthService
  ) {}
}
```
The login action has additional credentials metadata which is passed to a service to log the specific user into the application.


The example below shows the addBookToCollectionSuccess$ effect displaying a different alert depending on the number of books in the collection state.

```
// collection.effects.ts
import { Injectable } from '@angular/core';
import { Store, select } from '@ngrx/store';
import { Actions, ofType, createEffect } from '@ngrx/effects';
import { of } from 'rxjs';
import { tap, concatMap, withLatestFrom } from 'rxjs/operators';
import { CollectionApiActions } from '../actions';
import * as fromBooks from '../reducers';

@Injectable()
export class CollectionEffects {
  addBookToCollectionSuccess$ = createEffect(
    () =>
      this.actions$.pipe(
        ofType(CollectionApiActions.addBookSuccess),
        concatMap(action => of(action).pipe(
          withLatestFrom(this.store.pipe(select(fromBooks.getCollectionBookIds)))
        )),
        tap(([action, bookCollection]) => {
          if (bookCollection.length === 1) {
            window.alert('Congrats on adding your first book!');
          } else {
            window.alert('You have added book number ' + bookCollection.length);
          }
        })
      ),
    { dispatch: false }
  );

  constructor(
    private actions$: Actions,
    private store: Store<fromBooks.State>
  ) {}
}
```
