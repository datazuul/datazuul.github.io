---
layout: post
title: Angular Javascript Frontend Framework - Part 11 (Add routes to the application)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-10>.

> This tutorial lesson demonstrates how to add routes to your app.
> At the end of this lesson your application will have support for routing.

## Routing

> This tutorial introduces routing in Angular. Routing is the ability to navigate from one component in the application to another.
> In single page applications (SPA), only parts of the page is updated to represent the requested view for the user.
>
> The Angular Router enables users to declare routes and specify which component should be displayed on the screen if that route is requested by the application.
> 
> In this lesson, you will enable routing in your application to navigate to the details page.

# Create a default details component

Create the `DetailsComponent`:

```
$ cd angular-hello-world
$ ng generate component details --standalone --inline-template --skip-tests
CREATE src/app/details/details.component.css (0 bytes)
CREATE src/app/details/details.component.ts (315 bytes)
```

# Add routing to the application

> In the `src/app` directory, create a file called `routes.ts`. This file is where we will define the routes in the application.

File `src/app/routes.ts`:

```

```

(empty file)

Import the `routes` file and the `provideRouter` function and update the call to `bootstrapApplication` by adding `provideRouter` to include the routing configuration in `src/main.ts`:

```
import { bootstrapApplication, provideProtractorTestingSupport } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';
import { provideRouter } from '@angular/router';
import routeConfig from './app/routes';

bootstrapApplication(AppComponent,
  {
    providers: [
      provideProtractorTestingSupport(),
      provideRouter(routeConfig)
    ]
  }
).catch(err => console.error(err));
```

Update the `app` component to use routing in `src/app/app.component.ts`:

* Add a file level import for `RouterModule`
* Add `RouterModule` to the `@Component` metadata imports
* In the template property, replace the `<app-home></app-home>` tag with the `<router-outlet></router-outlet>` directive and add a link back to the home page on the logo.

```
import { Component } from '@angular/core';
import { HomeComponent } from './home/home.component';
import { RouterModule } from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [
    HomeComponent,
    RouterModule
  ],
  template: `
    <main>
      <a [routerLink]="['/']">
        <header class="brand-name">
          <img class="brand-logo" src="/assets/logo.svg" alt="logo" aria-hidden="true">
        </header>
      </a>
      <section class="content">
        <router-outlet></router-outlet>
      </section>
    </main>
  `,
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'homes';
}
```

# Add route to new component

In the previous step you removed the reference to the `<app-home>` component in the template. In this step, you will add a new route to that component.

* Add a file level imports for the `HomeComponent`, `DetailsComponent` and the `Routes` type that you'll use in the route definitions.
* Define a variable called `routeConfig` of type `Routes` and define two routes for the app

File `src/routes.ts`:

```
import { Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { DetailsComponent } from './details/details.component';

const routeConfig: Routes = [
    {
        path: '',
        component: HomeComponent,
        title: 'Home page'
    },
    {
        path: 'details/:id',
        component: DetailsComponent,
        title: 'Home details'
    }
];

export default routeConfig;
```

The entries in the routeConfig array represent the routes in the application.
The first entry navigates to the HomeComponent whenever the url matches ''.
The second entry uses some special formatting that will be revisited in a future lesson.

The application should still display the list of housing locations.

It is already possible to navigate to details page by manually entering URL <http://localhost:4200/details/1> and back to homepage by clicking on logo.