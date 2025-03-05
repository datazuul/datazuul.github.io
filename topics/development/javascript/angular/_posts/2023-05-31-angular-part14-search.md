---
layout: post
title: Angular Javascript Frontend Framework - Part 14 (Add a search feature to your app)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-13>.

> This tutorial lesson demonstrates how to add a search functionality to your Angular app.
> The app will enable users to search through the data provided by your app and display only the results that match the entered term.
>
> Your app will use data from a form to search for matching housing locations
> Your app will display only the matching housing locations

# Add a filtered list property to `HomeComponent` component

In this step, you'll update the `HomeComponent` class to store data in a new array property that you will use for filtering.

* add new property to the class called `filteredLocationList`
* init property in constructor to hold the full list in the beginning

File `src/app/home/home.component.ts`:

```
...
export class HomeComponent {
  filteredLocationList: HousingLocation[] = [];
  housingLocationList: HousingLocation[] = [];
  housingService: HousingService = inject(HousingService);

  constructor() {
    this.housingLocationList = this.housingService.getAllHousingLocations();
    this.filteredLocationList = this.housingLocationList;
  }
}
```

The `filteredLocationList` will hold the values that match the search criteria entered by the user.

The `filteredLocationList` should contain the total set of housing locations values by default when the page loads. Therefore it gets set in the constructor to full list.

# Wire up search field and button to search action

The `HomeComponent` already contains an input field that you will use to capture input from the user.
That string text will be used to filter the results.

* Update the `HomeComponent` template to include a [template variable](https://angular.io/guide/template-reference-variables) in the input called `#filter`. (Can be seen as "tagging" the field for later reference)
* Next, update the component template to attach an event handler to the "search" button.
* The last template update is to the `ngFor` directive. Update the `ngFor` value to iterate over values from the `filteredLocationList` array.  

File `src/app/home/home.component.ts`:

```
template: `
  <section>
    <form>
      <input type="text" placeholder="Filter by city" #filter>
      <button class="primary" type="button" (click)="filterResults(filter.value)">Search</button>
    </form>
  </section>
  <section class="results">
    <app-housing-location
      *ngFor="let housingLocation of filteredLocationList"
      [housingLocation]="housingLocation">
    </app-housing-location>
  </section>
`,
```

* This example uses a template variable to get access to the input as its value.
* By binding to the click event on the button, you are able to call the `filterResults` function. The argument to the function is the value property of the `filter` template variable. Specifically, the `.value` property from the input HTML element.

# Implement the event handler function

The template has been updated to bind the `filterResults` function to the `click` event. Next, your task is to implement the `filterResults` function in the `HomeComponent` class.

* Update the `HomeComponent` class to include the implementation of the `filterResults` function

File `src/app/home/home.component.ts`:

```
export class HomeComponent {
  filteredLocationList: HousingLocation[] = [];
  housingLocationList: HousingLocation[] = [];
  housingService: HousingService = inject(HousingService);

  constructor() {
    this.housingLocationList = this.housingService.getAllHousingLocations();
    this.filteredLocationList = this.housingLocationList;
  }

  filterResults(text: string) {
    if (!text) {
      this.filteredLocationList = this.housingLocationList;
    }

    this.filteredLocationList = this.housingLocationList.filter(
      housingLocation => housingLocation?.city.toLowerCase().includes(text.toLowerCase())
    );
  }
}
```

This function uses the String `filter` function to compare the value of the text parameter against the `housingLocation.city` property.
You can update this function to match against any property or multiple properties for a fun exercise.

Notice: Only a click on the search button submits the search correctly (hitting "Enter" does not....)