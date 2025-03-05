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

In this step, you will get the route parameter (housing location `id`) in the `DetailsComponent`.
Currently, the app displays "details works!", next you'll update the code to display the `id` value passed using the route parameters.

* Update the typescript to import the functions, classes and services (ActivatedRoute, and for later steps: HousingService, HousingLocation) that you'll need to use in the `DetailsComponent`.
* Update the template property of the `@Component` decorator to display the value `housingLocationId`
* Update the body of the `DetailsComponent` to set `housingLocationId` by getting the route param

File `src/app/details/details.component.ts`:

```
import { Component, inject } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ActivatedRoute } from '@angular/router';
import { HousingService } from '../housing.service';
import { HousingLocation } from '../housinglocation';

@Component({
  selector: 'app-details',
  standalone: true,
  imports: [CommonModule],
  template: `
    <p>details works! {{ housingLocationId }}</p>
  `,
  styleUrls: ['./details.component.css']
})
export class DetailsComponent {
  route: ActivatedRoute = inject(ActivatedRoute);
  housingLocationId = -1;
  constructor() {
    this.housingLocationId = Number(this.route.snapshot.params['id']);
  }
}
```

This code give the `DetailsComponent` access to the `ActivatedRoute` router feature that enables you to have access to the data about the current route.
In the constructor, the code converts the id parameter from the route to a number.

Test: In the browser, click on one of the housing location "learn more" links and confirm that the numeric value displayed on the page matches the id property for that location in the data.

# Customize the `DetailComponent`

Now that routing is working properly in the application this is a great time to update the template of the `DetailsComponent` to display the specific data represented by the housing location for the route parameter.

To access the data you will add a call to the `HousingService`.

* Update the template code to show data
* Update the body of the DetailsComponent class to use HousingService and setting `housingLocation`

File `src/app/details/details.component.ts`:

```
import { Component, inject } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ActivatedRoute } from '@angular/router';
import { HousingService } from '../housing.service';
import { HousingLocation } from '../housinglocation';

@Component({
  selector: 'app-details',
  standalone: true,
  imports: [CommonModule],
  template: `
    <article>
      <img class="listing-photo" [src]="housingLocation?.photo"
        alt="Exterior photo of {{housingLocation?.name}}"/>
      <section class="listing-description">
        <h2 class="listing-heading">{{housingLocation?.name}}</h2>
        <p class="listing-location">{{housingLocation?.city}}, {{housingLocation?.state}}</p>
      </section>
      <section class="listing-features">
        <h2 class="section-heading">About this housing location</h2>
        <ul>
          <li>Units available: {{housingLocation?.availableUnits}}</li>
          <li>Does this location have wifi: {{housingLocation?.wifi}}</li>
          <li>Does this location have laundry: {{housingLocation?.laundry}}</li>
        </ul>
      </section>
    </article>
  `,
  styleUrls: ['./details.component.css']
})
export class DetailsComponent {
  route: ActivatedRoute = inject(ActivatedRoute);
  housingService = inject(HousingService);
  housingLocation: HousingLocation | undefined;

  constructor() {
    const housingLocationId = Number(this.route.snapshot.params['id']);
    this.housingLocation = this.housingService.getHousingLocationById(housingLocationId);
  }
}
```

Now the component has the code to display the correct information based on the selected housing location.
The constructor now includes a call to the HousingService to pass the route parameter as an argument to the getHousingLocationById service function.

Notice that the `housingLocation` properties are being accessed with the optional chaining operator `?`.
This ensures that if the `housingLocation` value is `null` or `undefined` the application doesn't crash.

To style the page nicely, add CSS style to `src/app/details/details.component.css`:

```
.listing-photo {
    height: 600px;
    width: 50%;
    object-fit: cover;
    border-radius: 30px;
    float: right;
}

.listing-heading {
    font-size: 48pt;
    font-weight: bold;
    margin-bottom: 15px;
}

.listing-location::before {
    content: url('/assets/location-pin.svg') / '';
}

.listing-location {
    font-size: 24pt;
    margin-bottom: 15px;
}

.listing-features>.section-heading {
    color: var(--secondary-color);
    font-size: 24pt;
    margin-bottom: 15px;
}

.listing-features {
    margin-bottom: 20px;
}

.listing-features li {
    font-size: 14pt;
}

li {
    list-style-type: none;
}

.listing-apply .section-heading {
    font-size: 18pt;
    margin-bottom: 15px;
}

label,
input {
    display: block;
}

label {
    color: var(--secondary-color);
    font-weight: bold;
    text-transform: uppercase;
    font-size: 12pt;
}

input {
    font-size: 16pt;
    margin-bottom: 15px;
    padding: 10px;
    width: 400px;
    border-top: none;
    border-right: none;
    border-left: none;
    border-bottom: solid .3px;
}

@media (max-width: 1024px) {
    .listing-photo {
        width: 100%;
        height: 400px;
    }
}
```

Test: In the browser refresh the page and confirm that when you click on the "learn more" link for a given housing location the details page displays the correct information based on the data for that selected item.
