# Section 17: Modern JavaScript Development

## Overview

We divide our code into modules in order to split logic and write clean code. When we distribute the code to the browser, all the modules are typically bundled together and compressed in order to reduce computations necessary by the browser. Less amount of files typically means faster loading by the browser client.

Before creating the final JS bundle, transpiling/polyfilling is used in order to convert modern javascript into ES5 for browser compatibility. Transpiling/polyfilling is usually done by a tool called Babel.

## Modules

A module can be described as a reusable piece of code that enpasulates implementation details.  
A module is usually is a standalone file.  
A module usually contains imports and exports.  
ES6 modules have exactly _one module_ per file.

> A module can be imported into a HTML file in the following way: `<script type="module" defer src="script.js"></script> `. The type has to be set to module. The "defer" keyword specifies that the script should load only when the page has finished loading.

```JSX
// Exporting module shoppingCart.js
const shippingCost = 10;
export const cart = [];

export const addToCart = function (product, quantity) {
  cart.push({ product, quantity });
  console.log(`${quantity} ${product} added to cart`);
};

const totalPrice = 237;
const totalQuantity = 23;

export { totalPrice, totalQuantity as tq };

export default function (product, quantity) {
  cart.push({ product, quantity });
  console.log(`${quantity} ${product} added to cart`);
}

// Importing module
import * as ShoppingCart from './shoppingCart.js'; // import exerything from module
import { addToCart, totalPrice as price, tq } from './shoppingCart.js'; // import variables and create aliases
import add, { cart } from './shoppingCart.js'; // we call the default export "add" and the named export is "cart"
add('pizza', 2);
add('bread', 5);
add('apples', 4);

console.log(cart);
```

## CommonJS Modules

CommonJS modules have been used in NodeJS for a very long time, it is only recently that ES6 modules were added instead. NodeJS is a way of running JS on a web server outside of a browser. npm STILL uses the CommonJS module system because npm was originally only intended for Node.

This is why you might still encounter CommonJS modules code. Just like ES6 modules, each file is a standalone module.

Below is an example of using CommonJS modules:

```JSX
// Export
export.addTocart = function (product, quantity) {
  cart.push({ product, quantity });
  console.log(
    `${quantity} ${product} added to cart (sipping cost is ${shippingCost})`
  );
};

// Import
const { addTocart } = require('./shoppingCart.js');

```

## NPM

Node Package Manager is a way to manage packages/dependencies in our projects. This is a neat way of keeping track of package versions and dependencies instead of referring to packages in our HTML files directly like in the old days.

### package.json vs package-lock.json

When you install any package in your project by executing the command `npm install <package-name> --save`, it will install the exact latest version of that package in your project and save the dependency in pack age.json with a carat (^) sign. If the current version of a package is 5.2.3 then the installed version will be 5.2.3 and the saved dependency will be ^5.2.3. Carat (^) means it will support any higher version with major version 5 like 5.3.1 and so on. A tilde (~) instead of a carat means an **exact** version must be used.

Another file, package-lock.json is created for locking the dependency with the installed version.
To avoid differences in installed dependencies on different environments and to generate the same results on every environment we should use the package-lock.json file to install dependencies.

Ideally, this file should be on your source control with the package.json file so when you or any other user will clone the project and run the command “npm i”, it will install the exact same version saved in package-lock.json file and you will able to generate the same results as you developed with that particular package.

## Bundling

If we want to use npm packages that are still using CommonJS modules, we have to use a module bundler. The most common module bundler is probably Webpack, especially in the React world.

## npx

npx is a npm package runner which can be used to run any NodeJS executable.

## Babel and polyfilling

The reason why we need to transpile/polyfill our ES6 code into ES5 even many years after ES6 was introduced is because of older browsers. Some people have older computers that will not let them upgrade their browsers any further, for instance. If we want our applications to work for everyone

With Babel, we can choose what plugins that we want to transpile back to ES2015, for example we might want to transpile arrow functions to ES2015 but leave everything else in ES6 but that usually does not make much sense. It makes more sense to make use of predefined Babel presets.
