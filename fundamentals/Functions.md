# Section 10: Functions

## Default values

```JSX
const testFunction = (param1, param2, param3 = 10) => {
    return [param1, param2, param3]
}
```

## First Class Functions

### Higher-order Functions

A HOC is a function that **receives** another function as an argument, that **returns** a new function, or both. This is possible because of first-class functions.

```JSX
const greet = () => console.log('Jacob')

// This is a HOC (greet is a callback function)
btnClose.addEventListener('click', greet)

// This is also a HOC
function count() {
    let counter = 0;
    return function() {
        counter++;
    }
}
```

#### Example 1

Functions can take functions as arguments, look at the code example below:

```JSX
var years = [1990, 1965, 1937, 2005, 1998];

function arrayCalc(arr, fn) {
    var arrRes = [];
    for (var i = 0; i < arr.length; i++) {
        arrRes.push(fn(arr[i]));
    }
    return arrRes;
}

function calculateAge(el) {
    return 2016 - el;
}

function isFullAge(el) {
    return el >= 18;
}

function maxHeartRate(el) {
    if (el >= 18 && el <= 81) {
        return Math.round(206.9 - (0.67 * el));
    } else {
        return -1;
    }
}

var ages = arrayCalc(years, calculateAge);
var fullAges = arrayCalc(ages, isFullAge);
var rates = arrayCalc(ages, maxHeartRate);

console.log(ages);
console.log(rates);
```

#### Example 2

First class functions means that in JavaScript, functions are treated as values which enables Higher-order-functions or HOCs.

Functions can also return functions (a HOC), look at the code example below:

```JSX
const greet = function (greeting) {
  return function (name) {
    console.log(`${greeting} ${name}`);
  };
};

const greeterHey = greet('Hey');
greeterHey('Jonas');
greeterHey('Steven');

greet('Hello')('Jonas'); // works just like the above, except we don't assign the function to a variable first

// Challenge
const greetArr = greeting => name => console.log(`${greeting} ${name}`);

greetArr('Hi')('Jonas');
```

## Immediately Invoked Function Expressions (IIFE)

If we wrap a function in parantheses and omit the function name, we get an IIFE. This lets JavaScript know that it should treat it as an expression and not a function. The last parantheses is how we "invoke" the function in order to be an expression.

IIFE's can be used for data privacy, there is no way we can access the score variable in the code example below for example. The variables we create in the IIFE scope do not interfere with global variables:

```JSX
(function () {
    // score is encapsulated in this function scope
    var score = Math.random() * 10;
    console.log(score >= 5);
})();

// DOES NOT WORK
//console.log(score);

(function (goodLuck) {
    var score = Math.random() * 10;
    console.log(score >= 5 - goodLuck);
})(5);

(() => console.log('This will ALSO never run again'))();
```

> IIFEs are NOT used much anymore because the let and const keywords were added. Earlier, variables defined with var inside functions could be accessed by the global scope.

## Value vs Reference parameters

Primitive types (int, string etc.) passed to functions is like creating a copy of the variable, if we try to change the value inside the function, nothing will happen to the variable being passed in.

Arrays, functions and objects are passed by reference so changing those values will change the original variable being sent in.

## Callback functions

```JSX
const oneWord = function (str) {
  return str.replace(/ /g, '').toLowerCase();
};

const upperFirstWord = function (str) {
  const [first, ...others] = str.split(' ');
  return [first.toUpperCase(), ...others].join(' ');
};

// Higher-order function
const transformer = function (str, fn) {
  console.log(`Original string: ${str}`);
  console.log(`Transformed string: ${fn(str)}`);

  console.log(`Transformed by: ${fn.name}`);
};

transformer('JavaScript is the best!', upperFirstWord);
transformer('JavaScript is the best!', oneWord);

// JS uses callbacks all the time
const high5 = function () {
  console.log('👋');
};
document.body.addEventListener('click', high5);
['Jonas', 'Martha', 'Adam'].forEach(high5);
```

## Call and Apply

Call and apply allows us to change what the this keyword points to. The apply method does exactly the same thing as the call method, the only differenc is that apply does not receive a list of arguments after the "this" keyword, it takes an array.

```JSX
const lufthansa = {
  airline: 'Lufthansa',
  iataCode: 'LH',
  bookings: [],
  // book: function() {}
  book(flightNum, name) {
    console.log(
      `${name} booked a seat on ${this.airline} flight ${this.iataCode}${flightNum}`
    );
    this.bookings.push({ flight: `${this.iataCode}${flightNum}`, name });
  },
};

lufthansa.book(239, 'Jonas Schmedtmann');
lufthansa.book(635, 'John Smith');

const eurowings = {
  airline: 'Eurowings',
  iataCode: 'EW',
  bookings: [],
};

// book is no longer a method here, it is a function
const book = lufthansa.book;

// Does NOT work
// book(23, 'Sarah Williams');

// Call method
// set the this keyword to eurowings
book.call(eurowings, 23, 'Sarah Williams');
console.log(eurowings);

book.call(lufthansa, 239, 'Mary Cooper');
console.log(lufthansa);

const swiss = {
  airline: 'Swiss Air Lines',
  iataCode: 'LX',
  bookings: [],
};

book.call(swiss, 583, 'Mary Cooper');

// Apply method
const flightData = [583, 'George Cooper'];
book.apply(swiss, flightData);
console.log(swiss);

// the call method is identical to apply if used like
book.call(swiss, ...flightData);
```

## Bind

The difference between call & apply and bind is that bind does not immediately call the function, it returns a new function with the this keyword bound.

```JSX
// book.call(eurowings, 23, 'Sarah Williams');

const bookEW = book.bind(eurowings);
const bookLH = book.bind(lufthansa);
const bookLX = book.bind(swiss);

bookEW(23, 'Steven Williams');

// new function only needs the name parameter because the number is predefined in the bind method
const bookEW23 = book.bind(eurowings, 23);
bookEW23('Jonas Schmedtmann');
bookEW23('Martha Cooper');
```

### Real world example 1

```JSX

// With Event Listeners
lufthansa.planes = 300;
lufthansa.buyPlane = function () {
  console.log(this);

  this.planes++;
  console.log(this.planes);
};

// In this call the this keyword would be lufthansa
// lufthansa.buyPlane();

// In event handler functions the this keyword always points to the element on which that element is attached to.The 'this' keyword refers to the buy-button unless we specify specifically that it should refer to the lufthansa object by using bind.
document
  .querySelector('.buy')
  .addEventListener('click', lufthansa.buyPlane.bind(lufthansa));
```

### Real world example 2

```JSX
// Partial application (preset parameters)
const addTax = (rate, value) => value + value * rate;
console.log(addTax(0.1, 200));

// we don't care about the this keyword
const addVAT = addTax.bind(null, 0.23);
// addVAT = value => value + value * 0.23;

console.log(addVAT(100));
console.log(addVAT(23));


// Same thing as above without using bind
const addTaxRate = function (rate) {
  return function (value) {
    return value + value * rate;
  };
};
const addVAT2 = addTaxRate(0.23);
console.log(addVAT2(100));
console.log(addVAT2(23));
```

## Closures

```JSX
// Example 1
const secureBooking = function () {
  let passengerCount = 0;

  return function () {
    passengerCount++;
    console.log(`${passengerCount} passengers`);
  };
};

// secureBooking finishes execution after assignment but a CLOSURE is created in order for the booker function to have all the variables from the original function available to it.
const booker = secureBooking();

booker(); // 1
booker(); // 2
booker(); // 3

console.dir(booker);

// Example 2
let f;

const g = function () {
  const a = 23;
  f = function () {
    console.log(a * 2);
  };
};

const h = function () {
  const b = 777;
  f = function () {
    console.log(b * 2);
  };
};

g();
f();
console.dir(f);

// Re-assigning f function
h();
f();
console.dir(f);

// Example 3
const boardPassengers = function (n, wait) {
  const perGroup = n / 3;

  setTimeout(function () {
    console.log(`We are now boarding all ${n} passengers`);
    console.log(`There are 3 groups, each with ${perGroup} passengers`);
  }, wait * 1000);

  console.log(`Will start boarding in ${wait} seconds`);
};

const perGroup = 1000;
boardPassengers(180, 3);
```
