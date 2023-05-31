---
layout: post
title: Angular Javascript Frontend Framework - Part 15 (Add HTTP-API communication to backend of your app)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-14>.

> This tutorial demonstrates how to integrate HTTP and an API into your app.
>
> Up until this point your app has read data from a static array in an Angular service. The next step is to use a JSON server that your app will communicate with over HTTP. The HTTP request will simulate the experience of working with data from a server.
>
> Your app will use data from a JSON server.

# Install and Configure the JSON server

JSON Server is an open source tool used to create mock REST APIs.
You'll use it to serve the housing location data that is currently stored statically in the housing service.

Install `json-server` from npm:

```
$ sudo npm install -g json-server

added 315 packages in 5s

92 packages are looking for funding
  run `npm fund` for details
npm notice 
npm notice New minor version of npm available! 9.5.1 -> 9.6.7
npm notice Changelog: https://github.com/npm/cli/releases/tag/v9.6.7
npm notice Run npm install -g npm@9.6.7 to update!
npm notice 
```

It got installed in `/usr/local/lib/node_modules`:

```
$ ls -al /usr/local/lib/node_modules/
insgesamt 16
drwxr-xr-x 4 root root 4096 Mai 31 17:11 .
drwxr-xr-x 4 root root 4096 Mai 10 09:01 ..
drwxr-xr-x 3 root root 4096 Mai 10 09:01 @angular
drwxr-xr-x 5 root root 4096 Mai 31 17:12 json-server
```

* In the root directory of your project, create a file called `db.json`. This is where you will store the data for the `json-server`.
* Copy the data from `src/app/housing.service.ts` array `housingLocationList` to the file and assign a new array variable to it:
* Put property keys in double quotes and replce single quotes with double quotes (search and replace...)

Final file `db.json` in top of app directory `angular-hello-world`:

```
{
    "locations": [
        {
            "id": 0,
            "name": "Acme Fresh Start Housing",
            "city": "Chicago",
            "state": "IL",
            "photo": "/assets/abbilyn-rurenko-uOYak90r4L0-unsplash.jpg",
            "availableUnits": 4,
            "wifi": true,
            "laundry": true
        },
        {
            "id": 1,
            "name": "A113 Transitional Housing",
            "city": "Santa Monica",
            "state": "CA",
            "photo": "/assets/brian-babb-XbwHrt87mQ0-unsplash.jpg",
            "availableUnits": 0,
            "wifi": false,
            "laundry": true
        },
        {
            "id": 2,
            "name": "Warm Beds Housing Support",
            "city": "Juneau",
            "state": "AK",
            "photo": "/assets/cayetano-gros-ONea6ruvc50-unsplash.jpg",
            "availableUnits": 1,
            "wifi": false,
            "laundry": false
        },
        {
            "id": 3,
            "name": "Homesteady Housing",
            "city": "Chicago",
            "state": "IL",
            "photo": "/assets/luke-stackpoole-eWqOgJ-lfiI-unsplash.jpg",
            "availableUnits": 1,
            "wifi": true,
            "laundry": false
        },
        {
            "id": 4,
            "name": "Happy Homes Group",
            "city": "Gary",
            "state": "IN",
            "photo": "/assets/ralph-ravi-kayden-2d4lAQAlbDA-unsplash.jpg",
            "availableUnits": 1,
            "wifi": true,
            "laundry": false
        },
        {
            "id": 5,
            "name": "Hopeful Apartment Group",
            "city": "Oakland",
            "state": "CA",
            "photo": "/assets/seth-kane-XOEAHbE_vO8-unsplash.jpg",
            "availableUnits": 2,
            "wifi": true,
            "laundry": true
        },
        {
            "id": 6,
            "name": "Seriously Safe Towns",
            "city": "Oakland",
            "state": "CA",
            "photo": "/assets/todd-kent-178j8tJrNlc-unsplash.jpg",
            "availableUnits": 5,
            "wifi": true,
            "laundry": true
        },
        {
            "id": 7,
            "name": "Hopeful Housing Solutions",
            "city": "Oakland",
            "state": "CA",
            "photo": "/assets/webaliser-_TPTXZd9mOo-unsplash.jpg",
            "availableUnits": 2,
            "wifi": true,
            "laundry": true
        },
        {
            "id": 8,
            "name": "Seriously Safe Towns",
            "city": "Oakland",
            "state": "CA",
            "photo": "/assets/zak-boca-w504XUUximQ-unsplash.jpg",
            "availableUnits": 10,
            "wifi": false,
            "laundry": false
        },
        {
            "id": 9,
            "name": "Capital Safe Towns",
            "city": "Portland",
            "state": "OR",
            "photo": "/assets/josh-hild-ra7Gw5sjcYc-unsplash.jpg",
            "availableUnits": 6,
            "wifi": true,
            "laundry": true
        }
    ]
}
```

Test: start `json-server`:

```
$ json-server --watch db.json

  \{^_^}/ hi!

  Loading db.json
  Done

  Resources
  http://localhost:3000/locations

  Home
  http://localhost:3000

  Type s + enter at any time to create a snapshot of the database
  Watching...
```

In your web browser, navigate to the <http://localhost:3000/locations> and confirm that the response includes the data stored in `db.json`.

# Update service to use web service instead of local array

The data source has been configured, the next step is to update your web app to connect to it and use the data.

Changes in `HousingService`:

* Update the code to remove `housingLocationList` property and the array containing the data
* Add a string property called `url` and set the value to `http://localhost:3000/locations`
* Update the `getAllHousingLocations` function to make a call to the web service you configured
* Update the `getHousingLocationsById` function to make a call to the web service you configured

Final file `src/app/housing.service.ts`:

```
import { Injectable } from '@angular/core';
import { HousingLocation } from './housinglocation';

@Injectable({
  providedIn: 'root'
})
export class HousingService {
  url = 'http://localhost:3000/locations';

  constructor() { }

  async getAllHousingLocations(): Promise<HousingLocation[]> {
    const data = await fetch(this.url);
    return await data.json() ?? [];
  }

  async getHousingLocationById(id: number): Promise<HousingLocation | undefined> {
    const data = await fetch(`${this.url}/${id}`);
    return await data.json() ?? {};
  }

  submitApplication(firstName: string, lastName: string, email: string) {
    console.log(`Homes application received: firstName: ${firstName}, lastName: ${lastName}, email: ${email}.`);
  }
}
```

The code now uses asynchronous code (notice `async`, `Promise` and `await`) to make a `GET` request over `HTTP` using the native JavaScript-fetch-API.
For more advanced use cases consider using [HttpClient](https://angular.io/api/common/http/HttpClient) provided by Angular.

# Update the components to use asynchronous calls to the housing service

The server is now reading data from the HTTP request but the components that rely on the service now have errors because they were programmed to use the synchronous version of the service.

```
Error: src/app/details/details.component.ts:59:5 - error TS2740: Type 'Promise<HousingLocation | undefined>'

  this.housingLocation = this.housingService.getHousingLocationById(housingLocationId);
```

* update the components' constructors to use the new asynchronous version of the `getAllHousingLocations` and `getHousingLocationById`

File `src/app/home/home.component.ts`:

```
constructor() {
  this.housingService.getAllHousingLocations().then((housingLocationList: HousingLocation[]) => {
    this.housingLocationList = housingLocationList;
    this.filteredLocationList = housingLocationList;
  });
}
```

File `src/app/details/details.component.ts`:

```
constructor() {
  const housingLocationId = parseInt(this.route.snapshot.params['id'], 10);
  // parseInt: "10" is the radix: A value between 2 and 36 that specifies the base of the number in `string`.
  // If this argument is not supplied, strings with a prefix of '0x' are considered hexadecimal.
  // All other strings are considered decimal.
  this.housingService.getHousingLocationById(housingLocationId).then(housingLocation => {
    this.housingLocation = housingLocation;
  });
}
```

Notice: We also replaced `Number` with `parseInt`.

Congratulations! Your first Angular App "Homes" is finished!

Homepage:

![Homes housing location application homepage](/assets/topics/development/javascript/angular-homes-final-01.png)

Details page:

![Homes housing location application details](/assets/topics/development/javascript/angular-homes-final-02.png)