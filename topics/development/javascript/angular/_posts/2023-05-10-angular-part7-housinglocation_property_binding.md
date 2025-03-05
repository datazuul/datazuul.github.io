---
layout: post
title: Angular Javascript Frontend Framework - Part 7 (HousingLocation property binding)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-06>.

# What is a property binding?

> ... you added @Input decorators to properties in the HousingLocationComponent allowing the component to receive data. In this lesson, you'll continue the process of sharing data from the parent component to the child component by binding data to those properties in the template. There are several forms of data binding in Angular, in this lesson you'll use property binding.
>
> Property binding enables you to connect a variable to an Input in an Angular template. The data is then dynamically bound to the Input.

Further information can be found at <https://angular.io/guide/property-binding>.

> Property binding moves a value in one direction, from a component's property into a target element property.
> To read a target element property or call one of its methods, see the API reference for ViewChild (<https://angular.io/api/core/ViewChild>) and ContentChild (<https://angular.io/api/core/ContentChild>).

# Update tag in the `HomeComponent` template

This step adds property binding to the `<app-housing-location>` tag.

File `src/app/home/home.component.ts` - In the template property of the @Component decorator, update the code to match:

```
<section class="results">
  <app-housing-location [housingLocation]="housingLocation"></app-housing-location>
</section>
```

When adding a property binding to a component tag, we use the [attribute] = "value" syntax to notify Angular that the assigned value should be treated as a property from the component class and not a string value.

The value on the right handside is the name of the property from the HomeComponent:

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

Now, the `HousingLocationComponent` has access to data that it can use to customize the the component's display.

The app should still work without errors and still present:

![Homes housing location draft](/assets/topics/development/javascript/angular-homes-03.jpg)