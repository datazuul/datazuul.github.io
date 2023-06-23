---
layout: post
title: Angular Tutorial - Tour of Heroes - Part 7 (Get data from a server)
author: Ralf Eichinger
toc: true
---

# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/tour-of-heroes/toh-pt6>.

> This tutorial adds the following data persistence features with help from Angular's [HttpClient](https://angular.io/api/common/http/HttpClient).
>
> * The `HeroService` gets hero data with HTTP requests
> * Users can add, edit, and delete heroes and save these changes over HTTP
> * Users can search for heroes by name

# Enable HTTP services

`HttpClient` is Angular's mechanism for communicating with a remote server over HTTP.

Make `HttpClient` available everywhere in the application in two steps:

* add it to the root `AppModule` by importing `HttpClientModule`
* add `HttpClientModule` to the imports array

File `src/app/app.module.ts`:

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';
import { HeroesComponent } from './heroes/heroes.component';
import { HeroDetailComponent } from './hero-detail/hero-detail.component';
import { MessagesComponent } from './messages/messages.component';
import { AppRoutingModule } from './app-routing.module';
import { DashboardComponent } from './dashboard/dashboard.component';

@NgModule({
  declarations: [
    AppComponent,
    HeroesComponent,
    HeroDetailComponent,
    MessagesComponent,
    DashboardComponent
  ],
  imports: [
    AppRoutingModule,
    BrowserModule,
    FormsModule,
    HttpClientModule,
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

# Simulate a data server

This tutorial sample mimics communication with a remote data server by using the *In-memory Web API* module (see <https://github.com/angular/angular/tree/main/packages/misc/angular-in-memory-web-api>).

After installing the module, the application makes requests to and receive responses from the `HttpClient`. The application doesn't know that the *In-memory Web API* is intercepting those requests, applying them to an in-memory data store, and returning simulated responses.

By using the In-memory Web API, *you won't have to set up a server* to learn about HttpClient.

IMPORTANT: The In-memory Web API module has nothing to do with HTTP in Angular. If you're reading this tutorial to learn about HttpClient, you can skip over this step. If you're coding along with this tutorial, stay here and add the In-memory Web API now.

Install the In-memory Web API package from npm with the following command:

```sh
$ cd tutorial-angular-tour-of-heroes
$ npm install angular-in-memory-web-api --save
```

You now can find the package under:

```
$ ls -al node_modules/angular-in-memory-web-api/
insgesamt 68
drwxrwxr-x   4 ralf ralf  4096 Jun 22 13:37 .
drwxrwxr-x 561 ralf ralf 20480 Jun 22 13:37 ..
drwxrwxr-x   3 ralf ralf  4096 Jun 22 13:37 esm2022
drwxrwxr-x   2 ralf ralf  4096 Jun 22 13:37 fesm2022
-rwxrwxr-x   1 ralf ralf 25269 Jun 22 13:37 index.d.ts
-rwxrwxr-x   1 ralf ralf  1026 Jun 22 13:37 package.json
-rwxrwxr-x   1 ralf ralf   291 Jun 22 13:37 README.md
```
    
Generate the service class `src/app/in-memory-data.service.ts` with the following command:

```
$ ng generate service InMemoryData
CREATE src/app/in-memory-data.service.spec.ts (389 bytes)
CREATE src/app/in-memory-data.service.ts (141 bytes)
```

Replace the default contents of `src/app/in-memory-data.service.ts` with the following:

```
import { Injectable } from '@angular/core';
import { InMemoryDbService } from 'angular-in-memory-web-api';

import { Hero } from './hero';

@Injectable({
  providedIn: 'root',
})
export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    const heroes = [
      { id: 12, name: 'Dr. Nice' },
      { id: 13, name: 'Bombasto' },
      { id: 14, name: 'Celeritas' },
      { id: 15, name: 'Magneta' },
      { id: 16, name: 'RubberMan' },
      { id: 17, name: 'Dynama' },
      { id: 18, name: 'Dr. IQ' },
      { id: 19, name: 'Magma' },
      { id: 20, name: 'Tornado' }
    ];
    return {heroes};
  }

  // Overrides the genId method to ensure that a hero always has an id.
  // If the heroes array is empty,
  // the method below returns the initial number (11).
  // if the heroes array is not empty, the method below returns the highest
  // hero id + 1.
  genId(heroes: Hero[]): number {
    return heroes.length > 0 ? Math.max(...heroes.map(hero => hero.id)) + 1 : 11;
  }
}
```

In the `AppModule`

* import the `HttpClientInMemoryWebApiModule` and the `InMemoryDataService` class, which you create next
* after the `HttpClientModule`, add the `HttpClientInMemoryWebApiModule` to the `AppModule` imports array and configure it with the `InMemoryDataService`.

File `src/app/app.module.ts`:

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here
import { HttpClientModule } from '@angular/common/http';
import { HttpClientInMemoryWebApiModule } from 'angular-in-memory-web-api';

import { AppComponent } from './app.component';
import { HeroesComponent } from './heroes/heroes.component';
import { HeroDetailComponent } from './hero-detail/hero-detail.component';
import { MessagesComponent } from './messages/messages.component';
import { AppRoutingModule } from './app-routing.module';
import { DashboardComponent } from './dashboard/dashboard.component';
import { InMemoryDataService } from './in-memory-data.service';

@NgModule({
  declarations: [
    AppComponent,
    HeroesComponent,
    HeroDetailComponent,
    MessagesComponent,
    DashboardComponent
  ],
  imports: [
    AppRoutingModule,
    BrowserModule,
    FormsModule,
    HttpClientModule,
    // The HttpClientInMemoryWebApiModule module intercepts HTTP requests
    // and returns simulated server responses.
    // Remove it when a real server is ready to receive requests.
    HttpClientInMemoryWebApiModule.forRoot(
      InMemoryDataService, { dataEncapsulation: false }
    )
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

The `forRoot()` configuration method takes an `InMemoryDataService` class that primes the in-memory database.

The `in-memory-data.service.ts` file takes over the function of `mock-heroes.ts`. Don't delete `mock-heroes.ts` yet. You still need it for a few more steps of this tutorial.

After the server is ready, detach the In-memory Web API in next step so the application's requests can go through to the server.

# Heroes and HTTP

In the `HeroService`

* import `HttpClient` and `HttpHeaders`
* inject `HttpClient` into the constructor in a private property called `http`
* keep injecting the `MessageService` but since your application calls it so frequently, wrap it in a private `log()` method
* define the `heroesUrl` of the form `:base/:collectionName` with the address of the heroes resource on the server. Here `base` is the resource to which requests are made, and `collectionName` is the heroes data object in the `in-memory-data-service.ts`

File `src/app/hero.service.ts`:

```
import { Injectable } from '@angular/core';
import { Observable, of } from 'rxjs';
import { HttpClient, HttpHeaders } from '@angular/common/http';

import { Hero } from './hero';
import { HEROES } from './mock-heroes';
import { MessageService } from './message.service';

@Injectable({
  providedIn: 'root'
})
export class HeroService {
  private heroesUrl = 'api/heroes';  // URL to web api

  constructor(
    private http: HttpClient,
    private messageService: MessageService) { }

  getHero(id: number): Observable<Hero> {
    // For now, assume that a hero with the specified `id` always exists.
    // Error handling will be added in the next step of the tutorial.
    const hero = HEROES.find(h => h.id === id)!;
    this.log(`fetched hero id=${id}`);
    return of(hero);
  }

  getHeroes(): Observable<Hero[]> {
    const heroes = of(HEROES);
    this.log('fetched heroes');
    return heroes;
  }

  /** Log a HeroService message with the MessageService */
  private log(message: string) {
    this.messageService.add(`HeroService: ${message}`);
  }
}
```

## Get heroes with HttpClient

The current `HeroService.getHeroes()` uses the RxJS `of()` function to return an array of mock heroes as an `Observable<Hero[]>`.

Convert that method to use `HttpClient` as follows:

File `src/app/hero.service.ts`:

```
/** GET heroes from the server */
getHeroes(): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
}
```

Refresh the browser. The hero data should successfully load from the mock server.

You've swapped `of()` for `http.get()` and the application keeps working without any other changes because both functions return an `Observable<Hero[]>`.

## HttpClient methods return one value

All `HttpClient` methods return an RxJS `Observable` of something.

HTTP is a request/response protocol. You make a request, it returns a single response.

In general, an observable *can* return more than one value over time. An observable from `HttpClient` always emits a single value and then completes, never to emit again.

This particular call to `HttpClient.get()` returns an `Observable<Hero[]>`, which is an *observable of hero arrays*. In practice, it only returns a single hero array.

## HttpClient.get() returns response data

`HttpClient.get()` returns the body of the response as an untyped JSON object by default. Applying the optional type specifier, `<Hero[]>`, adds TypeScript capabilities, which reduce errors during compile time.

The server's data API determines the shape of the JSON data. The *Tour of Heroes* data API returns the hero data as an array.

Note: Other APIs may bury the data that you want within an object. You might have to dig that data out by processing the `Observable` result with the RxJS `map()` operator.

Although not discussed here, there's an example of `map()` in the `getHeroNo404()` method included in the sample source code.

## Error handling

Things go wrong, especially when you're getting data from a remote server. The `HeroService.getHeroes()` method should catch errors and do something appropriate.

To catch errors, you **"pipe" the observable** result from `http.get()` through an RxJS `catchError()` operator.

* Import the `catchError` symbol from `rxjs/operators`, along with some other operators (`map`, `tap`) to use later.
* Extend the observable result with the `pipe()` method and give it a `catchError()` operator.

File `src/app/hero.service.ts`:

```
import { Injectable } from '@angular/core';
import { Observable, of } from 'rxjs';
import { catchError, map, tap } from 'rxjs/operators';
import { HttpClient, HttpHeaders } from '@angular/common/http';

import { Hero } from './hero';
import { HEROES } from './mock-heroes';
import { MessageService } from './message.service';

@Injectable({
  providedIn: 'root'
})
export class HeroService {
  private heroesUrl = 'api/heroes';  // URL to web api

  constructor(
    private http: HttpClient,
    private messageService: MessageService) { }

  getHero(id: number): Observable<Hero> {
    // For now, assume that a hero with the specified `id` always exists.
    // Error handling will be added in the next step of the tutorial.
    const hero = HEROES.find(h => h.id === id)!;
    this.log(`fetched hero id=${id}`);
    return of(hero);
  }

  /** GET heroes from the server */
  getHeroes(): Observable<Hero[]> {
    return this.http.get<Hero[]>(this.heroesUrl)
      .pipe(
        catchError(this.handleError<Hero[]>('getHeroes', []))
      );
  }

  /** Log a HeroService message with the MessageService */
  private log(message: string) {
    this.messageService.add(`HeroService: ${message}`);
  }
}
```

The `catchError()` operator intercepts an **`Observable` that failed**. The operator then passes the error to the error handling function.

The following `handleError()` method reports the error and then returns an innocuous result so that the application keeps working.

### handleError

The following `handleError()` can be shared by many `HeroService` methods so it's generalized to meet their different needs.

Instead of handling the error directly, it returns an error handler function to `catchError`. This function is configured with both the name of the operation that failed and a safe return value.

File `src/app/hero.service.ts`:

```
/**
 * Handle Http operation that failed.
 * Let the app continue.
 *
 * @param operation - name of the operation that failed
 * @param result - optional value to return as the observable result
 */
private handleError<T>(operation = 'operation', result?: T) {
  return (error: any): Observable<T> => {

    // TODO: send the error to remote logging infrastructure
    console.error(error); // log to console instead

    // TODO: better job of transforming error for user consumption
    this.log(`${operation} failed: ${error.message}`);

    // Let the app keep running by returning an empty result.
    return of(result as T);
  };
}
```

After reporting the error to the console, the handler constructs a friendly message and returns a safe value so the application can keep working.

Because each service method returns a different kind of `Observable` result, `handleError()` takes a type parameter `T` to return the safe value as the type that the application expects.

## Tap into the Observable

The `getHeros()` method taps into the flow of observable values and sends a message, using the `log()` method, to the message area at the bottom of the page.

The RxJS `tap()` operator enables this ability by looking at the observable values, doing something with those values, and passing them along. The `tap()` callback doesn't access the values themselves.

Here is the final version of `getHeroes()` with the `tap()` that logs the operation.

File `src/app/hero.service.ts`:

```
/** GET heroes from the server */
getHeroes(): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
    .pipe(
      tap(_ => this.log('fetched heroes')),
      catchError(this.handleError<Hero[]>('getHeroes', []))
    );
}
```

## Get hero by id

Most web APIs support a *get by id* request in the form `:baseURL/:id`.

Here, the *base URL* is the `heroesURL` defined in the Heroes and HTTP section as `api/heroes` and `id` is the number of the hero that you want to retrieve. For example, `api/heroes/11`.

Update the `HeroService.getHero()` method with the following to make that request:

File `src/app/hero.service.ts`

```
/** GET hero by id. Will 404 if id not found */
getHero(id: number): Observable<Hero> {
  const url = `${this.heroesUrl}/${id}`;
  return this.http.get<Hero>(url).pipe(
    tap(_ => this.log(`fetched hero id=${id}`)),
    catchError(this.handleError<Hero>(`getHero id=${id}`))
  );
}
```

`getHero()` has three significant differences from `getHeroes()`:

* `getHero()` constructs a request URL with the desired hero's id
*  The server should respond with a single hero rather than an array of heroes
*  `getHero()` returns an `Observable<Hero>`, which is an observable of `Hero` *objects* rather than an observable of `Hero` *arrays*.

# Update heroes

Edit a hero's name in the hero detail view. As you type, the hero name updates the heading at the top of the page, yet when you click **Go back**, your changes are lost (not reflected in the list).

If you want changes to persist, you must write them back to the server.

At the end of the hero detail template, add a save button with a `click` event binding that invokes a new component method named `save()`.

File `src/app/hero-detail/hero-detail.component.html`:

```
<div *ngIf="hero">

  <h2>{{hero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{hero.id}}</div>
  <div>
    <label for="hero-name">Hero name: </label>
    <input id="hero-name" [(ngModel)]="hero.name" placeholder="name">
  </div>

  <button type="button" (click)="goBack()">go back</button>
  <button type="button" (click)="save()">save</button>
</div>
```

In the `HeroDetail` component class, add the following `save()` method, which persists hero name changes using the hero service `updateHero()` method and then navigates back to the previous view.

File `src/app/hero-detail/hero-detail.component.ts`:

```
save(): void {
  if (this.hero) {
    this.heroService.updateHero(this.hero).subscribe(() => this.goBack());
  }
}
```

## Add HeroService.updateHero()

The structure of the `updateHero()` method is like that of `getHeroes()`, but it uses `http.put()` to persist the changed hero on the server. Add the following to the `HeroService`.

File `src/app/hero.service.ts`:

```
/** PUT: update the hero on the server */
updateHero(hero: Hero): Observable<any> {
  return this.http.put(this.heroesUrl, hero, this.httpOptions).pipe(
    tap(_ => this.log(`updated hero id=${hero.id}`)),
    catchError(this.handleError<any>('updateHero'))
  );
}
```

The `HttpClient.put()` method takes three parameters:

* The URL
* The data to update, which is the modified `hero` in this case
* Options

The URL is unchanged. The heroes web API knows which hero to update by looking at the hero's `id`.

The heroes web API expects a special header in HTTP save requests. That header is in the `httpOptions` constant defined in the `HeroService`. Add the following to the `HeroService` class.

File `src/app/hero.service.ts`:

```
httpOptions = {
  headers: new HttpHeaders({ 'Content-Type': 'application/json' })
};
```

Refresh the browser, change a hero name and save your change. The `save()` method in `HeroDetailComponent` navigates to the previous view. The hero now appears in the list with the changed name.

# Add a new hero

To add a hero, this application only needs the hero's name. You can use an `<input>` element paired with an add button.

Insert the following into the `HeroesComponent` template, after the heading:

File `src/app/heroes/heroes.component.html`:

```
<h2>My Heroes</h2>

<div>
  <label for="new-hero">Hero name: </label>
  <input id="new-hero" #heroName />

  <!-- (click) passes input value to add() and then clears the input -->
  <button type="button" class="add-button" (click)="add(heroName.value); heroName.value=''">
    Add hero
  </button>
</div>

<ul class="heroes">
  ...
</ul>
```
    
In response to a `click` event, call the component's `click` handler, `add()`, and then clear the input field so that it's ready for another name. Add the following to the `HeroesComponent` class:

File `src/app/heroes/heroes.component.ts`

```
add(name: string): void {
  name = name.trim();
  if (!name) { return; }
  this.heroService.addHero({ name } as Hero)
    .subscribe(hero => {
      this.heroes.push(hero);
    });
}
```
    
When the given name isn't blank, the handler creates an object based on the hero's name. The handler passes the object name to the service's `addHero()` method.

When `addHero()` creates a new object, the `subscribe()` callback receives the new hero and pushes it into to the `heroes` list for display.

Add the following `addHero()` method to the `HeroService` class.

File `src/app/hero.service.ts`:

```
/** POST: add a new hero to the server */
addHero(hero: Hero): Observable<Hero> {
  return this.http.post<Hero>(this.heroesUrl, hero, this.httpOptions).pipe(
    tap((newHero: Hero) => this.log(`added hero w/ id=${newHero.id}`)),
    catchError(this.handleError<Hero>('addHero'))
  );
}
```

`addHero()` differs from `updateHero()` in two ways:

* It calls `HttpClient.post()` instead of `put()`
* It expects the server to create an `id` for the new hero, which it returns in the `Observable<Hero>` to the caller

Add some CSS to the `heroes.component.css`.

File `src/app/heroes/heroes.component.css`:

```
.add-button {
 padding: .5rem 1.5rem;
 font-size: 1rem;
 margin-bottom: 2rem;
}

.add-button:hover {
  color: white;
  background-color: #42545C;
}
```

Refresh the browser and add some heroes.


![Add hero](/assets/topics/development/javascript/angular-heroes-21.jpg)

# Delete a hero

Each hero in the heroes list should have a delete button.

Add the following button element to the `HeroesComponent` template, after the hero name in the repeated `<li>` element.

File `src/app/heroes/heroes.component.html`:

```
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{% raw %}{{hero.id}}{% endraw %}">
      <span class="badge">{% raw %}{{hero.id}}{% endraw %}</span> {% raw %}{{hero.name}}{% endraw %}
    </a>
    <button type="button" class="delete" title="delete hero"
      (click)="delete(hero)">x</button>
  </li>
</ul>
```

To position the delete button at the far right of the hero entry, add some CSS to the `heroes.component.css`.

File `src/app/heroes/heroes.component.css`:

```
button.delete {
  position: absolute;
  left: 210px;
  top: 5px;
  background-color: white;
  color:  #525252;
  font-size: 1.1rem;
  margin: 0;
  padding: 1px 10px 3px 10px;
}

button.delete:hover {
  background-color: #525252;
  color: white;
}
```

Add the `delete()` handler to the component class.

File `src/app/heroes/heroes.component.ts`:

```
delete(hero: Hero): void {
  this.heroes = this.heroes.filter(h => h !== hero);
  this.heroService.deleteHero(hero.id).subscribe();
}
```

Although the component delegates hero deletion to the `HeroService`, it remains responsible for updating its own list of heroes. The component's `delete()` method immediately removes the *hero-to-delete* from that list, anticipating that the `HeroService` succeeds on the server.

There's really nothing for the component to do with the `Observable` returned by `heroService.deleteHero()` **but it must subscribe anyway**.

Next, add a `deleteHero()` method to `HeroService` like this.

File `src/app/hero.service.ts`:

```
/** DELETE: delete the hero from the server */
deleteHero(id: number): Observable<Hero> {
  const url = `${this.heroesUrl}/${id}`;

  return this.http.delete<Hero>(url, this.httpOptions).pipe(
    tap(_ => this.log(`deleted hero id=${id}`)),
    catchError(this.handleError<Hero>('deleteHero'))
  );
}
```

Notice the following key points:

* `deleteHero()` calls `HttpClient.delete()`
* The URL is the heroes resource URL plus the `id` of the hero to delete
* You don't send data as you did with `put()` and `post()`
* You still send the `httpOptions`

Refresh the browser and try the new delete capability.

Important: If you neglect to `subscribe()`, the service can't send the delete request to the server. As a rule, an `Observable` *does nothing* until something subscribes. Confirm this for yourself by temporarily removing the `subscribe()`, clicking **Dashboard**, then clicking **Heroes**. This shows the full list of heroes again.

# Search by name

In this last exercise, you learn to chain `Observable` operators together so you can reduce the number of similar HTTP requests to consume network bandwidth economically.

## Add a heroes search feature to the Dashboard

As the user types a name into a search box, your application makes repeated HTTP requests for heroes filtered by that name. Your goal is to issue only as many requests as necessary.

### HeroService.searchHeroes()

Start by adding a `searchHeroes()` method to the `HeroService`.

File `src/app/hero.service.ts`:

```
/* GET heroes whose name contains search term */
searchHeroes(term: string): Observable<Hero[]> {
  if (!term.trim()) {
    // if not search term, return empty hero array.
    return of([]);
  }
  return this.http.get<Hero[]>(`${this.heroesUrl}/?name=${term}`).pipe(
    tap(x => x.length ?
       this.log(`found heroes matching "${term}"`) :
       this.log(`no heroes matching "${term}"`)),
    catchError(this.handleError<Hero[]>('searchHeroes', []))
  );
}
```

The method returns immediately with an empty array if there is no search term. The rest of it closely resembles `getHeroes()`, the only significant difference being the URL, which includes a query string with the search term.

## Add search to the dashboard

Open the `DashboardComponent` template and add the hero search element, `<app-hero-search>`, to the bottom of the markup.

File `src/app/dashboard/dashboard.component.html`:

```
<h2>Top Heroes</h2>
<div class="heroes-menu">
  <a *ngFor="let hero of heroes" routerLink="/detail/{{hero.id}}">
    {% raw %}{{hero.name}}{% endraw %}
  </a>
</div>

<app-hero-search></app-hero-search>
```

This template looks a lot like the `*ngFor` repeater in the `HeroesComponent` template.

For this to work, the next step is to add a component with a selector that matches `<app-hero-search>`.

## Create HeroSearchComponent

Run `ng generate` to create a `HeroSearchComponent`.

```
$ ng generate component hero-search
CREATE src/app/hero-search/hero-search.component.css (0 bytes)
CREATE src/app/hero-search/hero-search.component.html (26 bytes)
CREATE src/app/hero-search/hero-search.component.spec.ts (588 bytes)
CREATE src/app/hero-search/hero-search.component.ts (221 bytes)
UPDATE src/app/app.module.ts (1461 bytes)
```

`ng generate` creates the three `HeroSearchComponent` files and adds the component to the `AppModule` declarations.

Replace the `HeroSearchComponent` template with an `<input>` and a list of matching search results, as follows.

File `src/app/hero-search/hero-search.component.html`:

```
<div id="search-component">
  <label for="search-box">Hero Search</label>
  <input #searchBox id="search-box" (input)="search(searchBox.value)" />

  <ul class="search-result">
    <li *ngFor="let hero of heroes$ | async">
      <a routerLink="/detail/{{hero.id}}">
        {% raw %}{{hero.name}}{% endraw %}
      </a>
    </li>
  </ul>
</div>
```

Add private CSS styles to file `src/app/hero-search/hero-search.component.css`:

```
/* HeroSearch private styles */

label {
  display: block;
  font-weight: bold;
  font-size: 1.2rem;
  margin-top: 1rem;
  margin-bottom: .5rem;

}
input {
  padding: .5rem;
  width: 100%;
  max-width: 600px;
  box-sizing: border-box;
  display: block;
}

input:focus {
  outline: #336699 auto 1px;
}

li {
  list-style-type: none;
}
.search-result li a {
  border-bottom: 1px solid gray;
  border-left: 1px solid gray;
  border-right: 1px solid gray;
  display: inline-block;
  width: 100%;
  max-width: 600px;
  padding: .5rem;
  box-sizing: border-box;
  text-decoration: none;
  color: black;
}

.search-result li a:hover {
  background-color: #435A60;
  color: white;
}

ul.search-result {
  margin-top: 0;
  padding-left: 0;
}
```

As the user types in the search box, an input event binding calls the component's `search()` method with the new search box value.

### AsyncPipe

The `*ngFor` repeats hero objects. Notice that the `*ngFor` iterates over a list called `heroes$`, not `heroes`. The `$` is a convention that indicates `heroes$` is an `Observable`, not an array.

File `src/app/hero-search/hero-search.component.html`:

```
<li *ngFor="let hero of heroes$ | async">
```
    
Since `*ngFor` can't do anything with an `Observable`, use the pipe `|` character followed by `async`. This identifies Angular's `AsyncPipe` (see <https://angular.io/api/common/AsyncPipe>) and subscribes to an `Observable` automatically so you won't have to do so in the component class.

## Edit the HeroSearchComponent class

Replace the `HeroSearchComponent` class and metadata as follows.

File `src/app/hero-search/hero-search.component.ts`:

```
import { Component, OnInit } from '@angular/core';

import { Observable, Subject } from 'rxjs';

import {
   debounceTime, distinctUntilChanged, switchMap
 } from 'rxjs/operators';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-hero-search',
  templateUrl: './hero-search.component.html',
  styleUrls: [ './hero-search.component.css' ]
})
export class HeroSearchComponent implements OnInit {
  heroes$!: Observable<Hero[]>;
  private searchTerms = new Subject<string>();

  constructor(private heroService: HeroService) {}

  // Push a search term into the observable stream.
  search(term: string): void {
    this.searchTerms.next(term);
  }

  ngOnInit(): void {
    this.heroes$ = this.searchTerms.pipe(
      // wait 300ms after each keystroke before considering the term
      debounceTime(300),

      // ignore new term if same as previous term
      distinctUntilChanged(),

      // switch to new search observable each time the term changes
      switchMap((term: string) => this.heroService.searchHeroes(term)),
    );
  }
}
```
    
Notice the declaration of `heroes$` as an `Observable`:

```
heroes$!: Observable<Hero[]>;
```

Set this in `ngOnInit()`. Before you do, focus on the definition of `searchTerms`.

## The searchTerms RxJS subject

The `searchTerms` property is an RxJS `Subject`.

File `src/app/hero-search/hero-search.component.ts`:

```
private searchTerms = new Subject<string>();

// Push a search term into the observable stream.
search(term: string): void {
  this.searchTerms.next(term);
}
```

A `Subject` is both a source of observable values and an `Observable` itself. You can subscribe to a `Subject` as you would to any `Observable`.

You can also push values into that `Observable` by calling its `next(value)` method as the `search()` method does.

The event binding to the text box's `input` event calls the `search()` method.

File `src/app/hero-search/hero-search.component.html`:

```
<input #searchBox id="search-box" (input)="search(searchBox.value)" />
```

Every time the user types in the text box, the binding calls `search()` with the text box value as a *search term*. The `searchTerms` becomes an `Observable` emitting a steady stream of search terms.

## Chaining RxJS operators

Passing a new search term directly to the `searchHeroes()` after every user keystroke creates excessive HTTP requests, which taxes server resources and burns through data plans.

Instead, the `ngOnInit()` method pipes the `searchTerms` observable through a sequence of RxJS operators that reduce the number of calls to the `searchHeroes()`. Ultimately, this returns an observable of timely hero search results where each one is a `Hero[]`.

Here's a closer look at the code.

File `src/app/hero-search/hero-search.component.ts`:

```
this.heroes$ = this.searchTerms.pipe(
  // wait 300ms after each keystroke before considering the term
  debounceTime(300),

  // ignore new term if same as previous term
  distinctUntilChanged(),

  // switch to new search observable each time the term changes
  switchMap((term: string) => this.heroService.searchHeroes(term)),
);
```

Each operator works as follows:

* `debounceTime(300)`: waits until the flow of new string events pauses for 300 milliseconds before passing along the latest string. Requests aren't likely to happen more frequently than 300 ms.
* `distinctUntilChanged()`: ensures that a request is sent only if the filter text changed.
* `switchMap()` calls the search service for each search term that makes it through `debounce()` and `distinctUntilChanged()`. It cancels and discards previous search observables, returning only the latest search service observable.

Note: With the `switchMap` (see <https://www.learnrxjs.io/learn-rxjs/operators/transformation/switchmap>) operator, every qualifying key event can trigger an `HttpClient.get()` method call. Even with a 300 ms pause between requests, you could have many HTTP requests in flight and they may not return in the order sent.

`switchMap()` preserves the original request order while returning only the observable from the most recent HTTP method call. Results from prior calls are canceled and discarded.

Canceling a previous `searchHeroes()` `Observable` doesn't actually cancel a pending HTTP request. Unwanted results are discarded before they reach your application code.

Remember that the component `class` doesn't subscribe to the `heroes$` observable. That's the job of the `AsyncPipe` in the template.

# Try it

Run the application again. In the `Dashboard`, enter some text in the search box. Enter characters that match any existing hero names, and look for something like this.

![Search autocompletion](/assets/topics/development/javascript/angular-heroes-22.jpg)

Congratulation! This ends our tutorial.