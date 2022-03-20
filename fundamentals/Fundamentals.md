# Section 2: JavaScript Language Basics
* JavaScript can be used both client-side and server-side.
* Libraries like React, Angular and jQuery are 100% based on JavaScript.

> Most of this section's content was skipped since it is very basic knowledge.

## Function Declarations vs Expressions
Functions are values in JavaScript and can be stored in variables.
The main practical difference between function expressions and declarations is that we can call declarations BEFORE they are defined in the code (this is because of hoisting).
```JSX
// Function declaration
function calcAge1(birthYear) {
    return 2037 - birthYear;
}
const age1 = calcAge1(1991);

// Function expression
const calcAge2 = function (birthYear) {
    return 2037 - birthYear;
}
const age2 = calcAge2(1991);
```

## strict mode
You may have seen the line 'use strict'; at the top of JavaScript files. This is ALWAYS recommended to use because we get clear errors in the console instead of JavaScript failing silently.

## ES5, ES6+ and ESNext
During production: Use Babel to transpile and polyfill code (converting back to ES5 to ensure browser compatibility for all users). ES6 is supported for all browsers (down to IE 9 from 2011) which is why that would be our transpiling target in production.

> Check this ES6 compatibility table to see which features can be used with polyfill & transpiling: https://kangax.github.io/compat-table/es6/

## == vs ===
The difference between these two ways is that the "===" does not perform comparisons between types while "==" might be true even between types, i.e. '5' == 5 would be true.

Loose equality ("=="): allows type coercion.
Strict equality ("==="): does not allow type coercion.

## Template strings
```JSX
const name = 'Jacob';
const templateString = `I'm ${name}`; // template string
```

## let, const and var
These are the 3 ways of declaring variables in JavaScript. let and const were introduced in ES6 while the var keyword is the old way.

You should use the "let" keyword to declare variables that might change later while the "const" keyword should be used if a variable should not change, i.e. it is a constant. Thus, "const" should be the default way to declare variables if you are not sure that the variable will change.

### let vs var
The main difference between using let and var is the scope difference. let can only be used inside the scope where it is declared, like in a for loop, while var can be used outside of the loop.

```JSX
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // same variable!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

At the top level of programs and functions, let, unlike var, does not create a property on the global object. For example:

```JSX
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```
