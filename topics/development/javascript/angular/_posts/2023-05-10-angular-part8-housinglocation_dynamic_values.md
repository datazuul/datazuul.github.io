---
layout: post
title: Angular Javascript Frontend Framework - Part 8 (Dynamic values in templates)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-07>.

>This tutorial lesson demonstrates how to add interpolation to Angular templates in order to display dynamic data in a template.

# What is interpolation?

> In the previous part, you added data binding to the template to enable developers to pass data from the HomeComponent to the HousingLocationComponent. The next step is to display values (properties and Input values) in a template. In order to accomplish this task you have to use interpolation.
>
> The Angular template syntax supports mixing static template content with dynamic values and expressions.
>
> Using the {% raw %}`{{ expression }}`{% endraw %} in Angular templates, you can render values from properties, Inputs and valid JavaScript expressions.

For further informations read [Displaying values with interpolation](https://angular.io/guide/interpolation)

# Update HousingLocationComponent template to include iterpolated values

File `src/app/housing-location/housing-location.component.ts`:

```
template: `
  <section class="listing">
    <img class="listing-photo" [src]="housingLocation.photo" alt="Exterior photo of {{housingLocation.name}}">
    <h2 class="listing-heading">{{ housingLocation.name }}</h2>
    <p class="listing-location">{{ housingLocation.city}}, {{housingLocation.state }}</p>
  </section>
  `,
```

In this updated template code you have used property binding to bind the `housingLocation.photo` to the `src` attribute. The `alt` attribute uses interpolation to give more context to the alt text of the image.

You use interpoloation to include the values for name, city and state of the housingLocation property.

The in `HomeComponent` defined example house should be displayed. As the images are not part of the downloaded skeleton, we download one from <https://unsplash.com> and save it under `assets/example-house.jpg`

The app should still work without errors and now present:

![Homes housing location example](/assets/topics/development/javascript/angular-homes-04.jpg)