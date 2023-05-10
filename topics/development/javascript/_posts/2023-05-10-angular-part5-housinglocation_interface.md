---
layout: post
title: Angular Javascript Frontend Framework - Part 5 (HousingLocation Interface)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-04>.

# What is an interface?

> Interfaces are custom data types for your app.
>
> Angular uses TypeScript to take advantage of working in a strongly typed programming environment. Strong type checking reduces the likelihood of one element in your app sending incorrectly formatted data to another. Such type-mismatch errors are caught by the TypeScript compiler and many such errors can also be caught in your IDE.

Further information can be found at <https://www.typescriptlang.org/docs/handbook/2/objects.html>

# Create interface `HousingLocation`

Let's create an interface to define properties that represent data about a single housing location.

```
$ cd angular-hello-world
$ ng generate interface housinglocation
CREATE src/app/housinglocation.ts (37 bytes)
```

File `src/app/housinglocation.ts`:

```
export interface Housinglocation {
}
```

# Add properties to interface `HousingLocation`

Adds the properties to the interface that our app needs to represent a housing location:

File `src/app/housinglocation.ts`:

```
export interface HousingLocation {
  id: number;
  name: string;
  city: string;
  state: string;
  photo: string;
  availableUnits: number;
  wifi: boolean;
  laundry: boolean;
}
```

# Create a test house for your app

You have an interface, but you aren't using it yet.

Now create an instance of the interface and assign some sample data to it. (You won't see this sample data appear in your app yet.)

File `src/app/home/home.component.ts`:

```
import { HousingLocation } from '../housinglocation';
```

and replace the empty export class HomeComponent {} definition with this code to create a single instance of the new interface in the component:
 
```
export class HomeComponent {
  housingLocation: HousingLocation = {
    id: 9999,
    name: 'Test Home',
    city: 'Test city',
    state: 'ST',
    photo: 'assets/example-house.jpg',
    availableUnits: 99,
    wifi: true,
    laundry: false,
  };
}
```

By adding the housingLocation property of type HousingLocation to the HomeComponent class, we're able to confirm that the data matches the description of the interface. If the data didn't satisfy the description of the interface the IDE has enough information to give us helpful errors.

Confirm that the app still works and still looks like this:

![Homes housing location draft](/assets/topics/development/javascript/angular-homes-03.jpg)