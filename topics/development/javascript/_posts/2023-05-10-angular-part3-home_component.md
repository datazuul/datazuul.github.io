---
layout: post
title: Angular Javascript Frontend Framework - Part 3 (Home Component)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-02>. For further information about components see <https://angular.io/guide/component-overview>.

In this part  of our series we will create a new component for our Angular "Homes" app.

# What is a component?

> "Angular apps are built around components, which are Angular's building blocks. Components contain the code, HTML layout, and CSS style information that provide the function and appearance of an element in the app. In Angular, components can contain other components. An app's functions and appearance can divided and partitioned into components."

Each component consists of:

* An HTML template that declares what renders on the page
* A TypeScript class that defines behavior
* A CSS selector that defines how the component is used in a template
* Optionally, CSS styles applied to the template

Until now our app contains only the component `AppComponent`, whose function is to contain all the other components.

The new `HomeComponent` we will create will display the home page of the app. Later on we will create more components to provide more features to the app.

> In Angular, components have metadata that define its properties. When you create your HomeComponent, you use these properties:
>
> `selector`: to describe how Angular refers to the component in templates.
>
> `standalone`: to describe whether the component requires a ngModule.
>
> `imports`: to describe the component's dependencies.
>
> `template`: to describe the component's HTML markup and layout.
>
> `styleUrls`: to list the URLs of the CSS files that the component users in an array.
>
> Components have other properties, but these are the ones used by HomeComponent.

# Create homepage component `HomeComponent`

Open the VS Code terminal with "Terminal - New Terminal". In terminal change to the apps directory and run the command to create the new `HomeComponent`:

```
$ cd angular-hello-world
$ ng generate component Home --standalone --inline-template --skip-tests
CREATE src/app/home/home.component.css (0 bytes)
CREATE src/app/home/home.component.ts (303 bytes)
```

The new component is created in the new subdirectory `src/app/home`.

Code of `home.component.ts`:

```
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-home',
  standalone: true,
  imports: [CommonModule],
  template: `
    <p>
      home works!
    </p>
  `,
  styleUrls: ['./home.component.css']
})
export class HomeComponent {

}
```

If app is not running, yet, issue `ng serve`.

When you browse to <http://localhost:4200/> the app does not show any changes, because even though we added a new component, we haven't included it in any of the app's templates, yet.

# Add the new component to the app's layout

Now we add the new component `HomeComponent` to the app's root component `AppComponent`, so that it displays in our app's layout.

## File `src/app/app.component.ts`

Import `HomeComponent` by adding to the file level imports:

```
import { Component } from '@angular/core';
import { HomeComponent } from './home/home.component';
```

In `@Component` section update the imports array property and add `HomeComponent` and update the `template` property to include the following HTML code:

```
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [
    HomeComponent,
  ],
  template: `
    <main>
      <header class="brand-name">
        <img class="brand-logo" src="/assets/logo.svg" alt="logo" aria-hidden="true">
      </header>
      <section class="content">
        <app-home></app-home>
      </section>
    </main>
  `,
  styleUrls: ['./app.component.css'],
})
```

The template includes the `HomeComponent` with tags `<app-home></app-home>`. The tag name is defined in `src/app/home/home.component.ts` in the component's property `selector`:

```
@Component({
  selector: 'app-home',
```

If `ng serve` is running, the app should update. If `ng serve` is not running, start it again. `Hello world!` in your app should change to `home works!` from the `HomeComponent` and the header section with a logo has been added:

![Hello Homes!](/assets/topics/development/javascript/angular-homes-01.jpg)

# Add search elements to `HomeComponent`

Now we will remove the static default text and add HTML for a search filter and a button that is used later on. For now, that's all that `HomeComponent` has. Note that this just adds the search elements to the layout without any function, yet.

## File `src/app/home/home.component.ts`

```
template: `
  <section>
    <form>
      <input type="text" placeholder="Filter by city">
      <button class="primary" type="button">Search</button>
    </form>
  </section>
`,
```

## File `src/app/home/home.component.css`

```
.results {
  display: grid;
  column-gap: 14px;
  row-gap: 14px;
  grid-template-columns: repeat(auto-fill, minmax(400px, 400px));
  margin-top: 50px;
  justify-content: space-around;
}

input[type="text"] {
  border: solid 1px var(--primary-color);
  padding: 10px;
  border-radius: 8px;
  margin-right: 4px;
  display: inline-block;
  width: 30%;
}

button {
  padding: 10px;
  border: solid 1px var(--primary-color);
  background: var(--primary-color);
  color: white;
  border-radius: 8px;
}

@media (min-width: 500px) and (max-width: 768px) {
  .results {
      grid-template-columns: repeat(2, 1fr);
  }
  input[type="text"] {
      width: 70%;
  }   
}

@media (max-width: 499px) {
  .results {
      grid-template-columns: 1fr;
  }
}
```

Final result:

![Homes with search elements](/assets/topics/development/javascript/angular-homes-02.jpg)