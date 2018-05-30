# Adding External Libraries
* [Including JavaScript files](#including-javascript-files)
* [Including CSS files](#including-css-files)

## Including JavaScript files
1. Install an external JavaScript library by using any package manager (npm, yarn, etc.)
2. Include a path of the library to the ```externalJS``` property of the ```pbiviz.json```

Please pay attention to [Types](@Types.md) if you'd like to add typings for your JavaScript file to get intellisense and compile time safety on them.

### Example
* Installing [d3](https://www.npmjs.com/package/d3) by using [npm](https://www.npmjs.com/)

```bash
npm install d3@5 --save
npm install @types/d3 --save-dev
```

* Including ```d3``` to the ```pbiviz.json```

```json
{
  "visual": {...},
  "apiVersion": ...,
  "author": {...},
  "assets": {...},
  "externalJS": [
    "node_modules/d3/dist/d3.min.js"
  ],
  "style": ...,
  "capabilities": ...,
  "dependencies": ...
}
```
> Note: Please make sure the d3.min.js is indeed in the path that you specify in the `externalJS` property. For example, d3 publisher changed the build directory name from `build` to `dist` during v4 to v5 upgrade.

* Accessing the d3 object

To access the d3 object exposed by UMD, you have to explicitly access it through the `window` object.  Like this:

```ts
// visual.ts

module powerbi.extensibility.visual {
  export class Visual {
    ...
    constructor () {
      const d3 = window.d3 // <-- accessing d3
    }
  }
}
```

* Adding Types to d3

When you do this, however, Typescript does not know the type of `d3` on the window object.  You can declare the type like so:

```ts
interface Window {
  d3: typeof d3; //<-- declaring the type like so
}

module powerbi.extensibility.visual {
  ...
}
```

## 
Please visit [this repo to find the real example](https://github.com/Microsoft/powerbi-visuals-sankey/blob/c8200da56913cd8b253be949a35fad0f4472b6de/pbiviz.json#L22) page .

## Additional notes on including JS Libraries
There are some things to watch out for when including other JS Libraries.
* The library has to have UMD enabled.  Meaning it has to add a predefined variable to the global variable (`window`). Otherwise, you'd need to create a wrapper library to expose the library yourself and publish to npm.
* The library needs to be targeting the browser, not NodeJS.  i.e. `express` cannot be included.
* The library should not make external HTTP calls.  This is because in order for your visual to be certified, custom visuals cannot make external HTTP calls.

If the JS library uses UMD, then you should be able to follow similar installation steps as d3.

## Including CSS files
1. Install an external CSS framework by using any package manager (npm, yarn, etc.)
2. Include the ```import``` statement to the ```.less``` file of the visual

### Example
* Installing [bootstrap](https://www.npmjs.com/package/bootstrap) by using [npm](https://www.npmjs.com/)

```bash
npm install bootstrap --save
```

* Include the ```import``` statement to the ```visual.less```

```less
@import (less) "node_modules/bootstrap/dist/css/bootstrap.css";
```

Please visit [this](https://github.com/Microsoft/powerbi-visuals-sankey/blob/c8200da56913cd8b253be949a35fad0f4472b6de/style/visual.less#L32) page to find the real example.
