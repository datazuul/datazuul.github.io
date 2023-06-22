---
layout: post
title: Angular Tutorial - Tour of Heroes - Part 6 (Add navigation with routing)
author: Ralf Eichinger
toc: true
---

# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/tour-of-heroes/toh-pt5>.

> The Tour of Heroes application has new requirements:
>
> * Add a Dashboard view
> * Add the ability to navigate between the Heroes and Dashboard views
> * When users click a hero name in either view, navigate to a detail view of the selected hero
> * When users click a deep link in an email, open the detail view for a particular hero

> When you're done, users can navigate the application like this:

![Heroes navigation](/assets/topics/development/javascript/angular-heroes-16.png)

(original image from <https://angular.io/generated/images/guide/toh/nav-diagram.png>)

# Add the AppRoutingModule

In Angular, the best practice is to load and configure the router in a separate, top-level module. The router is dedicated to routing (URLs to target views) and imported by the root `AppModule`.

By convention, the module class name is `AppRoutingModule` and it belongs in the `app-routing.module.ts` in the `src/app` directory.

Run `ng generate` to create the application routing module.

```
$ ng generate module app-routing --flat --module=app
CREATE src/app/app-routing.module.ts (196 bytes)
UPDATE src/app/app.module.ts (754 bytes)
```

Parameter:

* `--flat`: Puts the file in `src/app` instead of its own directory.
* `--module=app`: Tells `ng generate` to register it in the imports array of the `AppModule`.

File `src/app/app.module.ts`:

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here

import { AppComponent } from './app.component';
import { HeroesComponent } from './heroes/heroes.component';
import { HeroDetailComponent } from './hero-detail/hero-detail.component';
import { MessagesComponent } from './messages/messages.component';
import { AppRoutingModule } from './app-routing.module';

@NgModule({
  declarations: [
    AppComponent,
    HeroesComponent,
    HeroDetailComponent,
    MessagesComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

The file `src/app/app-routing.module.ts` that `ng generate` created looks like this:

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

@NgModule({
  declarations: [],
  imports: [
    CommonModule
  ]
})
export class AppRoutingModule { }
```

Replace it with the following:

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { HeroesComponent } from './heroes/heroes.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

First, the `app-routing.module.ts` file imports `RouterModule` and `Routes` so the application can have routing capability. The next import, `HeroesComponent`, gives the Router somewhere to go once you configure the routes.

Notice that the `CommonModule` references and declarations array are unnecessary, so are no longer part of `AppRoutingModule`. The following sections explain the rest of the `AppRoutingModule` in more detail.

## Routes

The next part of the file is where you configure your routes. *Routes* tell the Router which view to display when a user clicks a link or pastes a URL into the browser address bar.

Since `app-routing.module.ts` already imports `HeroesComponent`, you can use it in the routes array:

File `src/app/app-routing.module.ts`:

```
const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];
```

A typical Angular Route (see <https://angular.io/api/router/Route>) has two properties:

* `path`: A string that matches the URL in the browser address bar.
* `component`: The component that the router should create when navigating to this route.

This tells the router to match that URL to `path: 'heroes'` and display the `HeroesComponent` when the URL is something like <http://localhost:4200/heroes>.

## RouterModule.forRoot()

The `@NgModule` metadata initializes the router and starts it listening for browser location changes.

The following line adds the `RouterModule` to the `AppRoutingModule` imports array and configures it with the routes in one step by calling `RouterModule.forRoot()`.

File `src/app/app-routing.module.ts`:

```
imports: [ RouterModule.forRoot(routes)],
```

Note: The method is called `forRoot()` because you configure the router at the application's root level. The `forRoot()` method supplies the service providers and directives needed for routing, and performs the initial navigation based on the current browser URL.

Next, `AppRoutingModule` exports `RouterModule` to be available throughout the application.

File `src/app/app-routing.module.ts`:

```
exports: [ RouterModule ]
```

# Add RouterOutlet

Open the `AppComponent` template and replace the `<app-heroes>` element with a `<router-outlet>` element.

File `src/app/app.component.html`:

```
<h1>{% raw %}{{title}}{% endraw %}</h1>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

The `AppComponent` template no longer needs `<app-heroes>` because the application only displays the `HeroesComponent` when the user navigates to it.

The `<router-outlet>` tells the router where to display ("embed") routed views.

The [RouterOutlet](https://angular.io/api/router/RouterOutlet) is one of the router directives that became available to the `AppComponent` because `AppModule` imports `AppRoutingModule` which exported `RouterModule`. The `ng generate` command you ran at the start of this tutorial added this import because of the `--module=app` flag. If you didn't use the `ng generate` command to create `app-routing.module.ts`, import `AppRoutingModule` into `app.module.ts` and add it to the imports array of the [NgModule](https://angular.io/api/core/NgModule).

# Try it

If you're not still serving your application, run `ng serve` to see your application in the browser under <http://localhost:4200/>.

The browser should refresh and display the application title but not the list of heroes.

Look at the browser's address bar. The URL ends in `/`. The route path to `HeroesComponent` is `/heroes`.

Append `/heroes` to the URL in the browser address bar: <http://localhost:4200/heroes>.

You should see the familiar heroes overview/detail view.

Remove `/heroes` from the URL in the browser address bar. The browser should refresh and display the application title but not the list of heroes.

# Add a navigation link using routerLink

Ideally, users should be able to click a link to navigate rather than pasting a route URL into the address bar.

Add a `<nav>` element and, within that, an anchor element that, when clicked, triggers navigation to the `HeroesComponent`. The revised `AppComponent` template looks like this:

File `src/app/app.component.html`:

```
<h1>{% raw %}{{title}}{% endraw %}</h1>
<nav>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

A `routerLink` attribute is set to "/heroes", the string that the router matches to the route to `HeroesComponent`. The `routerLink` is the selector for the [RouterLink](https://angular.io/api/router/RouterLink) directive that turns user clicks into router navigations. It's another of the public directives in the `RouterModule`.

The browser refreshes and displays the application title and heroes link, but not the heroes list.

![heroes link](/assets/topics/development/javascript/angular-heroes-17.jpg)

Click the link. The address bar updates to `/heroes` and the list of heroes appears.

![heroes link clicked](/assets/topics/development/javascript/angular-heroes-18.jpg)

Make this and future navigation links look better by adding private CSS styles to `src/app/app.component.css`:

```
/* AppComponent's private CSS styles */
h1 {
  margin-bottom: 0;
}
nav a {
  padding: 1rem;
  text-decoration: none;
  margin-top: 10px;
  display: inline-block;
  background-color: #e8e8e8;
  color: #3d3d3d;
  border-radius: 4px;
}

nav a:hover {
  color: white;
  background-color: #42545C;
}
nav a:active {
  background-color: black;
}
```

# Add a dashboard view

Routing makes more sense when your application has more than one view, yet the *Tour of Heroes* application has only the heroes view.

To add a `DashboardComponent`, run `ng generate` as shown here:

```sh
$ ng generate component dashboard
CREATE src/app/dashboard/dashboard.component.css (0 bytes)
CREATE src/app/dashboard/dashboard.component.html (24 bytes)
CREATE src/app/dashboard/dashboard.component.spec.ts (580 bytes)
CREATE src/app/dashboard/dashboard.component.ts (214 bytes)
UPDATE src/app/app.module.ts (848 bytes)
```

`ng generate` creates the files for the `DashboardComponent` and declares it in `AppModule`.

Replace the default content in these files as shown here:

File `src/app/dashboard/dashboard.component.html`:

```
<h2>Top Heroes</h2>
<div class="heroes-menu">
  <a *ngFor="let hero of heroes">
    {{hero.name}}
  </a>
</div>
```

The template presents a grid of hero name links.

* The `*ngFor` repeater creates as many links as are in the component's `heroes` array.
* The links are styled as colored blocks by the `dashboard.component.css`.
* The links don't go anywhere yet.

File `src/app/dashboard/dashboard.component.ts`:

```
import { Component, OnInit } from '@angular/core';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: [ './dashboard.component.css' ]
})
export class DashboardComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit(): void {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes.slice(1, 5));
  }
}
```

The class is like the `HeroesComponent` class.

* It defines a `heroes` array property
* The constructor expects Angular to inject the `HeroService` into a private `heroService` property
* The `ngOnInit()` lifecycle hook calls `getHeroes()`
* This `getHeroes()` returns the sliced list of `heroes` at positions 1 and 5, returning only Heroes two, three, four, and five.


File `src/app/dashboard/dashboard.component.css`:

```
/* DashboardComponent's private CSS styles */

h2 {
  text-align: center;
}

.heroes-menu {
  padding: 0;
  margin: auto;
  max-width: 1000px;

  /* flexbox */
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  align-content: flex-start;
  align-items: flex-start;
}

a {
  background-color: #3f525c;
  border-radius: 2px;
  padding: 1rem;
  font-size: 1.2rem;
  text-decoration: none;
  display: inline-block;
  color: #fff;
  text-align: center;
  width: 100%;
  min-width: 70px;
  margin: .5rem auto;
  box-sizing: border-box;

  /* flexbox */
  order: 0;
  flex: 0 1 auto;
  align-self: auto;
}

@media (min-width: 600px) {
  a {
    width: 18%;
    box-sizing: content-box;
  }
}

a:hover {
  background-color: #000;
}
```

## Add the dashboard route

To navigate to the dashboard, the router needs an appropriate route.

Import the `DashboardComponent` in the `app-routing.module.ts` file
and add a route to the routes array that matches the path `dashboard` to the `DashboardComponent`.

File `src/app/app-routing.module.ts`:

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { HeroesComponent } from './heroes/heroes.component';
import { DashboardComponent } from './dashboard/dashboard.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent },
  { path: 'dashboard', component: DashboardComponent }
];
```

## Add a default route

When the application starts, the browser's address bar points to the web site's root (`''` or `/`). That doesn't match any existing route so the router doesn't navigate anywhere. The space below the `<router-outlet>` is blank.

To make the application navigate to the dashboard automatically, add the following route to the `routes` array.

File `src/app/app-routing.module.ts`:

```
{ path: '', redirectTo: '/dashboard', pathMatch: 'full' },
```

This route redirects a URL that fully matches the empty path to the route whose path is '/dashboard'.

After the browser refreshes, the router loads the `DashboardComponent` and the browser address bar shows the `/dashboard` URL.

![dashboard](/assets/topics/development/javascript/angular-heroes-19.jpg)

## Add dashboard link to the shell

The user should be able to navigate between the `DashboardComponent` and the `HeroesComponent` by clicking links in the navigation area near the top of the page.

Add a dashboard navigation link to the `AppComponent` shell template, just above the *Heroes* link.

File `src/app/app.component.html`:

```
<h1>{% raw %}{{title}}{% endraw %}</h1>
<nav>
  <a routerLink="/dashboard">Dashboard</a>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

After the browser refreshes you can navigate freely between the two views by clicking the links.

# Navigating to hero details

The `HeroDetailComponent` displays details of a selected hero. At the moment the `HeroDetailComponent` is only visible at the bottom of the `HeroesComponent`.

The user should be able to get to these details in three ways.

* By clicking a hero in the dashboard.
* By clicking a hero in the heroes list.
* By pasting a "deep link" URL into the browser address bar that identifies the hero to display.

This section enables navigation to the `HeroDetailComponent` and liberates it from the `HeroesComponent`.

## Delete hero details from HeroesComponent

When the user clicks a hero in `HeroesComponent`, the application should navigate to the `HeroDetailComponent`, replacing the heroes list view with the hero detail view. The heroes list view should no longer show hero details as it does now.

Open the `src/app/heroes/heroes.component.html` and delete the `<app-hero-detail>` element from the bottom.

Clicking a hero item now does nothing. You can fix that after you enable routing to the `HeroDetailComponent`.

## Add a hero detail route

An URL like `~/detail/11` would be a good URL for navigating to the Hero Detail view of the hero whose id is 11.

Open file `src/app/app-routing.module.ts` and

* import `HeroDetailComponent`
* add a `parameterized` route to the `routes` array (and we sorted it alphabetically for better overview) that matches the path pattern to the `hero detail view`

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { HeroesComponent } from './heroes/heroes.component';
import { DashboardComponent } from './dashboard/dashboard.component';
import { HeroDetailComponent } from './hero-detail/hero-detail.component';

const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'detail/:id', component: HeroDetailComponent },
  { path: 'heroes', component: HeroesComponent },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

The colon `:` character in the path indicates that `:id` is a placeholder for a specific hero `id`.

At this point, all application routes are in place.

## DashboardComponent hero links

The `DashboardComponent` hero links do nothing at the moment.

Now that the router has a route to `HeroDetailComponent`, fix the dashboard hero links to navigate using the parameterized dashboard route.

File `src/app/dashboard/dashboard.component.html`:

```
<h2>Top Heroes</h2>
<div class="heroes-menu">
  <a *ngFor="let hero of heroes" routerLink="/detail/{% raw %}{{hero.id}}{% endraw %}">
    {{hero.name}}
  </a>
</div>
```

You're using Angular *interpolation binding* (see <https://angular.io/guide/interpolation>) within the `*ngFor` repeater (see <https://angular.io/api/common/NgFor>) to insert the current iteration's `hero.id` into each `routerLink`.

## HeroesComponent hero links

The hero items in the `HeroesComponent` are `<li>` elements whose click events are bound to the component's `onSelect()` method.

Actual File `src/app/heroes/heroes.component.html`:

```
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <button type="button" (click)="onSelect(hero)" [class.selected]="hero === selectedHero">
      <span class="badge">{% raw %}{{hero.id}}{% endraw %}</span>
      <span class="name">{% raw %}{{hero.name}}{% endraw %}</span>
    </button>
  </li>
</ul>
```

Remove the inner HTML of `<li>`. Wrap the badge and name in an anchor `<a>` element. Add a `routerLink` attribute to the anchor that's the same as in the dashboard template.

New File `src/app/heroes/heroes.component.html`:

```
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{% raw %}{{hero.id}}{% endraw %}">
      <span class="badge">{% raw %}{{hero.id}}{% endraw %}</span> {% raw %}{{hero.name}}{% endraw %}
    </a>
  </li>
</ul>
```

Be sure to fix the private style sheet in `src/app/heroes/heroes.component.css` to make the list look as it did before:

```
/* HeroesComponent's private CSS styles */
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  position: relative;
  cursor: pointer;
}

.heroes li:hover {
  left: .1em;
}

.heroes a {
  color: #333;
  text-decoration: none;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
  display: block;
  width: 100%;
}

.heroes a:hover {
  color: #2c3a41;
  background-color: #e6e6e6;
}

.heroes a:active {
  background-color: #525252;
  color: #fafafa;
}

.heroes .badge {
  display: inline-block;
  font-size: small;
  color: white;
  padding: 0.8em 0.7em 0 0.7em;
  background-color: #405061;
  line-height: 1em;
  position: relative;
  left: -1px;
  top: -4px;
  height: 1.8em;
  min-width: 16px;
  text-align: right;
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}
```

## Remove dead code - optional

While the `HeroesComponent` class still works, the `onSelect()` method and `selectedHero` property are no longer used.

It's nice to tidy things up for your future self. Here's the class after pruning away the dead code.

File `src/app/heroes/heroes.component.ts`:

```
import { Component, OnInit } from '@angular/core';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit(): void {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes);
  }
}
```

# Routable HeroDetailComponent

The parent `HeroesComponent` used to set the `HeroDetailComponent.hero` property and the `HeroDetailComponent` displayed the hero.

`HeroesComponent` doesn't do that anymore. Now the router creates the `HeroDetailComponent` in response to a URL such as `~/detail/12`.

The `HeroDetailComponent` needs a new way to get the hero to display. This section explains the following:

* Get the route that created it
* Extract the `id` from the route
* Get the hero with that `id` from the server using the `HeroService`

Steps:

* Add the imports for `ActivatedRoute` (see <https://angular.io/api/router/ActivatedRoute>), `Location` (see <https://angular.io/api/common/Location>) and `HeroService`
* Inject the `ActivatedRoute`, `HeroService`, and `Location` services into the constructor, saving their values in private fields

File `src/app/hero-detail/hero-detail.component.ts`:

```
import { Component, Input } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-hero-detail',
  templateUrl: './hero-detail.component.html',
  styleUrls: ['./hero-detail.component.css']
})
export class HeroDetailComponent {
  @Input() hero?: Hero;
  
  constructor(
    private route: ActivatedRoute,
    private heroService: HeroService,
    private location: Location
  ) {}
}
```

The `ActivatedRoute` holds information about the route to this instance of the `HeroDetailComponent`. This component is interested in the route's parameters extracted from the URL. The "id" parameter is the `id` of the hero to display.

The `HeroService` gets hero data from the remote server and this component uses it to get the hero-to-display.

The `Location` is an Angular service for interacting with the browser. This service lets you navigate back to the previous view.

## Extract the id route parameter

* Add implements `OnInit` and add import for `OnInit`.
* Change hero `property`: remove `@Input()` and change line to `hero: Hero | undefined;`, remove `Input` import
* In the `ngOnInit()` lifecycle hook call `getHero()` and define it as follows.

File `src/app/hero-detail/hero-detail.component.ts`

```
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-hero-detail',
  templateUrl: './hero-detail.component.html',
  styleUrls: ['./hero-detail.component.css']
})
export class HeroDetailComponent implements OnInit {
  hero: Hero | undefined;

  constructor(
    private route: ActivatedRoute,
    private heroService: HeroService,
    private location: Location
  ) {}

  ngOnInit(): void {
    this.getHero();
  }

  getHero(): void {
    const id = Number(this.route.snapshot.paramMap.get('id'));
    this.heroService.getHero(id).subscribe(hero => this.hero = hero);
  }
}
```

The `route.snapshot` is a static image of the route information shortly after the component was created.

The `paramMap` is a dictionary of route parameter values extracted from the URL. The "id" key returns the `id` of the hero to fetch.

Route parameters are always strings. The JavaScript `Number` function converts the string to a number, which is what a hero `id` should be.

The browser refreshes and the application crashes with a compiler error. `HeroService` doesn't have a `getHero()` method. Add it now.

## Add HeroService.getHero()

Open `HeroService` and add the following `getHero()` method with the `id` after the `getHeroes()` method:

File `src/app/hero.service.ts`:

```
getHero(id: number): Observable<Hero> {
  // For now, assume that a hero with the specified `id` always exists.
  // Error handling will be added in the next step of the tutorial.
  const hero = HEROES.find(h => h.id === id)!;
  this.messageService.add(`HeroService: fetched hero id=${id}`);
  return of(hero);
}
```
    
IMPORTANT: The backtick (<code>`</code>) characters define a JavaScript template literal (see <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals>) for embedding the `id`.

Like `getHeroes()`, `getHero()` has an asynchronous signature. It returns a mock hero as an `Observable`, using the RxJS `of()` function.

You can rewrite `getHero()` as a real `Http` request without having to change the `HeroDetailComponent` that calls it (see next tutorial part).

## Try it

The browser refreshes and the application is working again. You can click a hero in the dashboard or in the heroes list and navigate to that hero's detail view.

If you paste `http://localhost:4200/detail/12` in the browser address bar, the router navigates to the detail view for the hero with `id: 12`, "Dr Nice".

# Find the way back

By clicking the browser's back button, you can go back to the previous page. This could be the hero list or dashboard view, depending upon which sent you to the detail view.

It would be nice to have a button on the `HeroDetail` view that can do that.

Add a *go back* button to the bottom of the component template and bind it to the component's `goBack()` method.

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
</div>
```

Add a `goBack()` *method* to the component class that navigates backward one step in the browser's history stack using the `Location` service that you used to inject.

File `src/app/hero-detail/hero-detail.component.ts`:

```
goBack(): void {
  this.location.back();
}
```

Refresh the browser and start clicking. Users can now navigate around the application using the new buttons.

The details look better when you add the private CSS styles to `src/app/hero-detail/hero-detail.component.css`:

```
/* HeroDetailComponent's private CSS styles */
label {
  color: #435960;
  font-weight: bold;
}
input {
  font-size: 1em;
  padding: .5rem;
}
button {
  margin-top: 20px;
  background-color: #eee;
  padding: 1rem;
  border-radius: 4px;
  font-size: 1rem;
}
button:hover {
  background-color: #cfd8dc;
}
button:disabled {
  background-color: #eee;
  color: #ccc;
  cursor: auto;
}
```

![Details page](/assets/topics/development/javascript/angular-heroes-20.jpg)