---
layout: post
title: Angular Javascript Frontend Framework - Part 4 (HousingLocation Component)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-03>.

## Create the `HousingLocationComponent`

```
$ cd tutorial-angular-homes
$ ng generate component HousingLocation --standalone --inline-template --skip-tests
CREATE src/app/housing-location/housing-location.component.css (0 bytes)
CREATE src/app/housing-location/housing-location.component.ts (350 bytes)
```

### Add component styling

Add CSS styling for later steps.

File `src/app/housing-location/housing-location.component.css`:

```
.listing {
  background: var(--accent-color);
  border-radius: 30px;
  padding-bottom: 30px;
}
.listing-heading {
  color: var(--primary-color);
  padding: 10px 20px 0 20px;
}
.listing-photo {
  height: 250px;
  width: 100%;
  object-fit: cover;
  border-radius: 30px 30px 0 0;
}
.listing-location {
  padding: 10px 20px 20px 20px;
}
.listing-location::before {
  content: url("/assets/location-pin.svg") / "";
}

section.listing a {
  padding-left: 20px;
  text-decoration: none;
  color: var(--primary-color);
}
section.listing a::after {
  content: "\203A";
  margin-left: 5px;
}
```

Confirm that the app builds without error and is still working: `ng serve` -> <http://localhost:4200/>

## Add `HousingLocationComponent` to `HomeComponent`


### File `src/app/home/home.component.ts`

Import `HousingLocationComponent`:

```
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HousingLocationComponent } from '../housing-location/housing-location.component';
```

Update the imports property of the `@Component` metadata by adding `HousingLocationComponent` to the array:

```
imports: [
  CommonModule,
  HousingLocationComponent
],
```

Now the component is ready for use in the template. Update the template property of the @Component metadata to include a reference to the `<app-housing-location>` tag:

```
template: `
  <section>
    <form>
      <input type="text" placeholder="Filter by city">
      <button class="primary" type="button">Search</button>
    </form>
  </section>
  <section class="results">
    <app-housing-location></app-housing-location>
  </section>
`,
```

Final result:

![Homes housing location draft](/assets/topics/development/javascript/angular-homes-03.jpg)