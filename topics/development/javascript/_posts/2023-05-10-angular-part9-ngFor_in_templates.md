---
layout: post
title: Angular Javascript Frontend Framework - Part 9 (Use *ngFor to list objects in component)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-08>.

>This tutorial lesson demonstrates how to use `ngFor` directive in Angular templates in order to display dynamically repeated data in a template.

We will add a data set to the app and will display a list of elements from the new data set using ngFor.


# Add housing data to the HomeComponent

In the `HomeComponent` there is only a single housing location. In this step, you will add an array of `HousingLocation` entries.

Remove the `housingLocation` property from the `HomeComponent` class and  add a property called `housingLocationList`.

As the downloaded skeleton did not contain the images, we downloaded 10 "house" images from <https://unsplash.com/> and saved it in `src/assets`.

File `src/app/home/home.component.ts`:

```
export class HomeComponent {
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
}
```

Note: Now we get a red error code for "housingLocation" which no longer exists. But do not change the @Component decorator, you will update that code in the next step.

# Update the HomeComponent template to use *ngFor

Now the app has a dataset that you can use to display the entries in the browser using the `ngFor` directive.

Update the `<app-housing-location>` tag in the template code.

File `src/app/home/home.component.ts`:

```
<app-housing-location
  *ngFor="let housingLocation of housingLocationList"
  [housingLocation]="housingLocation">
</app-housing-location>
```

The app should still work without errors and now present:

![Homes housing location example](/assets/topics/development/javascript/angular-homes-05.jpg)