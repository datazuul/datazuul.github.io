---
layout: post
title: Angular Javascript Frontend Framework - Part 12 (Integrate and customize details page)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-11>.

> This tutorial lesson demonstrates how to connect the details page to your app.
> At the end of this lesson your application will have support for routing to the details page.

## Routing with route parameters

> In the previous lesson, you added routing to your app and in this lesson you will expand the types of routing your app supports.
> Each housing location has specific details that should be displayed when a user navigates to the details page for that item.
> To accomplish this goal, you will need to use route parameters.
> 
> Route parameters enable you to include dynamic information as a part of your route URL.
> To identify which housing location a user has clicked on you will use the `id` property of the `HousingLocation` type.

# Add link with param to details page in `HousingLocationComponent`

In previous lesson we added a second route to `src/app/routes.ts`, this route includes a special segment that identifies the route parameter, `id`:

```
path: 'details/:id',
```

In this case, `:id` is dynamic and will change based on how the route is requested by the code.

Add necessary imports for `RouterModule` on top and in @Component.imports.

Add a hyperlink using `routerLink` directive with dynamic route param to the template section in `HousingLocationComponent`:

File `src/app/housing-location/housing-location.component.ts`:

```
import { Component, Input } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HousingLocation } from '../housinglocation';
import { RouterModule } from '@angular/router';

@Component({
  selector: 'app-housing-location',
  standalone: true,
  imports: [CommonModule, RouterModule],
  template: `
    <section class="listing">
      <img class="listing-photo" [src]="housingLocation.photo" alt="Exterior photo of {{housingLocation.name}}">
      <h2 class="listing-heading">{{ housingLocation.name }}</h2>
      <p class="listing-location">{{ housingLocation.city}}, {{housingLocation.state }}</p>
      <a [routerLink]="['/details', housingLocation.id]">Learn More</a>
    </section>
  `,
  styleUrls: ['./housing-location.component.css']
})
export class HousingLocationComponent {
  @Input() housingLocation!: HousingLocation;
}
```

The routerLink directive enables Angular's router to create dynamic links in the application.
The value assigned to the routerLink is an array with two entries: the static portion of the path and the dynamic data.

At this point you can confirm that the routing is working in your app.
In the browser, refresh the home page and click the "learn more" button for a housing location details page.

# Get route parameters

