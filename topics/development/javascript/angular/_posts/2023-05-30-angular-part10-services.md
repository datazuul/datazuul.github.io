---
layout: post
title: Angular Javascript Frontend Framework - Part 10 (Angular services)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-09>.

> This tutorial lesson demonstrates how to create an Angular service and use dependency injection to include it in your app.
> Your app will get a service to serve the data to your app.
> At the end of this lesson, the service reads data from local, static data.
> In a later lesson, you update the service to get data from a web service.
> This tutorial introduces Angular services and dependency injection.

## Angular services

> Angular services provide a way for you to separate Angular app data and functions that can be used by multiple components in your app.
> To be used by multiple components, a service must be made injectable. Services that are injectable and used by a component become dependencies of that component.
> The component depends on those services and can't function without them.

## Dependency injection

> Dependency injection is the mechanism that manages the dependencies of an app's components and the services that other components can use.

# Create a new service for your app

This step creates an injectable service for your app.

```
$ cd tutorial-angular-homes
$ ng generate service housing --skip-tests
CREATE src/app/housing.service.ts (136 bytes)
```

# Add static data to the new service

> This step adds some sample data to your new service. In a later lesson, you replace the static data with a web interface to get data as you might in a real app.
> For now, your app's new service uses the data that has, so far, been created locally in HomeComponent.

From file `src/app/home/home.component.ts` copy `housingLocationList` variable (with the array list value) into `src/app/housing.service.ts`.
(Later on we will replace getting the `housingLocationList` list in `home.component.ts` with our new service.)
Add two access methods (`getAllHousingLocations` and `getHousingLocationById`) for the copied list in `src/app/housing.service.ts`:

File `src/app/housing.service.ts`:

```
import { Injectable } from '@angular/core';
import { HousingLocation } from './housinglocation';

@Injectable({
  providedIn: 'root'
})
export class HousingService {
  housingLocationList: HousingLocation[] = [
    {
      id: 0,
      name: 'Acme Fresh Start Housing',
      city: 'Chicago',
      state: 'IL',
      photo: '/assets/abbilyn-rurenko-uOYak90r4L0-unsplash.jpg',
      availableUnits: 4,
      wifi: true,
      laundry: true
    },
    {
      id: 1,
      name: 'A113 Transitional Housing',
      city: 'Santa Monica',
      state: 'CA',
      photo: '/assets/brian-babb-XbwHrt87mQ0-unsplash.jpg',
      availableUnits: 0,
      wifi: false,
      laundry: true
    },
    {
      id: 2,
      name: 'Warm Beds Housing Support',
      city: 'Juneau',
      state: 'AK',
      photo: '/assets/cayetano-gros-ONea6ruvc50-unsplash.jpg',
      availableUnits: 1,
      wifi: false,
      laundry: false
    },
    {
      id: 3,
      name: 'Homesteady Housing',
      city: 'Chicago',
      state: 'IL',
      photo: '/assets/luke-stackpoole-eWqOgJ-lfiI-unsplash.jpg',
      availableUnits: 1,
      wifi: true,
      laundry: false
    },
    {
      id: 4,
      name: 'Happy Homes Group',
      city: 'Gary',
      state: 'IN',
      photo: '/assets/ralph-ravi-kayden-2d4lAQAlbDA-unsplash.jpg',
      availableUnits: 1,
      wifi: true,
      laundry: false
    },
    {
      id: 5,
      name: 'Hopeful Apartment Group',
      city: 'Oakland',
      state: 'CA',
      photo: '/assets/seth-kane-XOEAHbE_vO8-unsplash.jpg',
      availableUnits: 2,
      wifi: true,
      laundry: true
    },
    {
      id: 6,
      name: 'Seriously Safe Towns',
      city: 'Oakland',
      state: 'CA',
      photo: '/assets/todd-kent-178j8tJrNlc-unsplash.jpg',
      availableUnits: 5,
      wifi: true,
      laundry: true
    },
    {
      id: 7,
      name: 'Hopeful Housing Solutions',
      city: 'Oakland',
      state: 'CA',
      photo: '/assets/webaliser-_TPTXZd9mOo-unsplash.jpg',
      availableUnits: 2,
      wifi: true,
      laundry: true
    },
    {
      id: 8,
      name: 'Seriously Safe Towns',
      city: 'Oakland',
      state: 'CA',
      photo: '/assets/zak-boca-w504XUUximQ-unsplash.jpg',
      availableUnits: 10,
      wifi: false,
      laundry: false
    },
    {
      id: 9,
      name: 'Capital Safe Towns',
      city: 'Portland',
      state: 'OR',
      photo: '/assets/josh-hild-ra7Gw5sjcYc-unsplash.jpg',
      availableUnits: 6,
      wifi: true,
      laundry: true
    }
  ];

  constructor() { }

  getAllHousingLocations(): HousingLocation[] {
    return this.housingLocationList;
  }

  getHousingLocationById(id: number): HousingLocation | undefined {
    return this.housingLocationList.find(housingLocation => housingLocation.id === id);
  }
}
```

Note: We had to add file level import `import { HousingLocation } from './housinglocation';` to make type `HousingLocation` known.

The app should still work without errors.

# Inject the new service into HomeComponent

> This step injects the new service into your app's HomeComponent so that it can read the app's data from a service.
> In a later lesson, you replace the static data with a live data source to get data as you might in a real app.

Import the `inject` function and the `HousingService` into the HomeComponent class.

File `src/app/home/home.component.ts`:

```
import { Component, inject } from '@angular/core';
import { HousingService } from '../housing.service';
```

> From HomeComponent, delete the housingLocationList delete the array entries and assign housingLocationList the value of empty array ([]).
> In a few steps you will update the code to pull the data from the HousingService.

File `src/app/home/home.component.ts`:

```
export class HomeComponent {
  housingLocationList: HousingLocation[] = [];
}
```

In HomeComponent, add this code to inject the new service and initialize the data for the app.
The constructor is the first function that runs when this component is created.
The code in the constructor will assign the housingLocationList the value returned from the call to getAllHousingLocations:

File `src/app/home/home.component.ts`:

```
export class HomeComponent {
  housingLocationList: HousingLocation[] = [];
  housingService: HousingService = inject(HousingService);

  constructor() {
    this.housingLocationList = this.housingService.getAllHousingLocations();
  }
}
```

Now the UI will again show the list of all houses (like before :-) ):

![Homes housing location example](/assets/topics/development/javascript/angular-homes-05.jpg)