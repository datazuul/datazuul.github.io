---
layout: post
title: Build a IIIF Mirador viewer with plugins
author: Ralf Eichinger
toc: true
---

Checkout project <https://github.com/ProjectMirador/mirador-integration> and customize it by adding plugins.
Make sure you have all tools in place: see ["Installation and usage of webpack with npm (and nvm)"]({% post_url topics/development/javascript/2023-08-09-npm-webpack-installation %})

The basic Mirador integration already has the `mirador-image-tools` integrated.

Here is the list of available plugins (at time of writing this post):

* Mirador Annotations Plugin:  adds annotation creation tools to the user interface. Users can` create rectangle, oval, and polygon annotations and add text descriptors - <https://github.com/ProjectMirador/mirador-annotations>
* Mirador Download Plugin: adds manifest-provided download links (e.g. rendering) to the window options menu - <https://github.com/ProjectMirador/mirador-dl-plugin>
* Mirador Image Cropper Plugin: to retrieve the image url for the selected area - <https://github.com/dbmdz/mirador-imagecropper>
* Mirador Image Tools Plugin: adds image manipulation tools - <https://github.com/ProjectMirador/mirador-image-tools>
* Mirador Keyboard Shortcuts Plugin: adds keyboard shortcuts - <https://github.com/dbmdz/mirador-keyboardshortcuts>
* Mirador Share Plugin: adds several options for sharing a resource to the window options menu. Options include specifying embedded `<iframe>` options and a preferred share link - <https://github.com/ProjectMirador/mirador-share-plugin>
* Mirador Text Overlay Plugin: display a selectable text overlay based on OCR or transcriptions - <https://github.com/dbmdz/mirador-textoverlay>

To add a plugin to the `mirador-integration` project, e.g. `mirador-share-plugin`:

```
$ npm install mirador-share-plugin

added 1 package, changed 1 package, and audited 1109 packages in 7s
...
$
```

Now the plugin is added as a dependency to `package.json`:

```
"dependencies": {
    "css-loader": "^3.6.0",
    "mirador": "^3.3.0",
    "mirador-image-tools": "^0.11.0",
    "mirador-share-plugin": "^0.11.0",
    "parcel-bundler": "^1.12.4",
    "react": "^16.13.1",
    "react-dom": "^16.13.1",
    "style-loader": "^1.2.1",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12"
  }
```

Now we can add it to the initialisation code for Mirador in `src/index.js`:

```
import Mirador from 'mirador/dist/es/src/index';
import { miradorImageToolsPlugin } from 'mirador-image-tools';
import miradorSharePlugin from 'mirador-share-plugin';

const config = {
  id: 'demo',
  windows: [{
    imageToolsEnabled: true,
    imageToolsOpen: true,
    manifestId: 'https://purl.stanford.edu/sn904cj3429/iiif/manifest',
  }],
  theme: {
    palette: {
      primary: {
        main: '#1967d2',
      },
    },
  },
};

Mirador.viewer(config, [
  ...miradorImageToolsPlugin,
  ...miradorSharePlugin
]);
```

Lets pack the changed Mirador:

```
$ npm run webpack
```

And test the new plugin by opening `webpack/index.html` in a browser.