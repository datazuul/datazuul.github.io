---
layout: post
title: Angular Javascript Frontend Framework - Part 6 (HousingLocation Input param)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-05>.

# What is an Input?

> Inputs allow components to share data. The direction of the data sharing is from parent component to child component.
>
> To receive data from a parent component, a child component must mark a class property with the @Input() decorator. This decorator can be used in components and directives.

Thoughts of a Java developer:

* "Decorator" seems to be analogous to "Annotation"
* "Input param" seems to be analogous to "Constructor argument"

Further information can be found at <https://angular.io/api/core/Input> and [Sharing data between child and parent directives and components](https://angular.io/guide/inputs-outputs)

We'll define @Input() properties in the HousingLocationComponent component which will enable you to customize the data displayed in the component.

# Add an input parameter to `HousingLocationComponent`

Update the imports to include `Input` decorator and `HousingLocation` interface in file `src/app/housing-location/housing-location.component.ts`:

```
import { Component, Input } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HousingLocation } from '../housinglocation';
```

Add a property called `housingLocation` of type `HousingLocation` to the `HousingLocationComponent` class. Add an "!" after the property name and prefix it with the `@Input()` decorator:

```
export class HousingLocationComponent {
  @Input() housingLocation!: HousingLocation;
}
```

You have to add the "!" because the input is expecting the value to be passed. In this case, there is no default value. In our example application case we know that the value will be passed in - this is by design. The exclamation point is called the non-null assertion operator and it tells the TypeScript compiler that the value of this property won't be `null` or `undefined`.

The app should still work without errors and still present:

![Homes housing location draft](/assets/topics/development/javascript/angular-homes-03.jpg)