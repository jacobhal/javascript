# Section 9: Data Structures and Modern Operators

## Destructuring

Destructuring is a way of unpacking values into separate variables.

```JSX
// DESTRUCTURING ARRAYS

const getArrayValues = () => {
  return [1, 2]
}
// Destructure array from method into 2 different variables
const [variable1, variable2] = getArrayValues()

const nested = [2, 4, [5, 6]]
const [i, , [j, k]] = // 2, 5, 6 as separate variables
```

```JSX
// DESTRUCTURING OBJECTS
const openingHours = {
  [weekdays[3]]: {
    open: 12,
    close: 22,
  },
  [weekdays[4]]: {
    open: 11,
    close: 23,
  },
  [weekdays[5]]: {
    open: 0, // Open 24 hours
    close: 24,
  },
};

const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  // ES6 enhanced object literals
  openingHours,

  // destructuring parameters
  orderDelivery({ starterIndex = 1, mainIndex = 0, time = '20:00', address }) {
    console.log(
      `Order received! ${this.starterMenu[starterIndex]} and ${this.mainMenu[mainIndex]} will be delivered to ${address} at ${time}`
    );
  }
```

# The Spread Operator

```JSX
const arr = [7, 8, 9];
const badNewArr = [1, 2, arr[0], arr[1], arr[2]];
console.log(badNewArr);

// badNewArr === newArr
const newArr = [1, 2, ...arr];
console.log(newArr);

console.log(...newArr);
console.log(1, 2, 7, 8, 9);

const newMenu = [...restaurant.mainMenu, 'Gnocci'];
console.log(newMenu);

// Copy array
const mainMenuCopy = [...restaurant.mainMenu];

// Join 2 arrays
const menu = [...restaurant.starterMenu, ...restaurant.mainMenu];
console.log(menu);

// Objects
const newRestaurant = { foundedIn: 1998, ...restaurant, founder: 'Guiseppe' };
console.log(newRestaurant);

// Instead of Object.assign we can use the spread syntax to get a deep copy
const restaurantCopy = { ...restaurant };
restaurantCopy.name = 'Ristorante Roma';
console.log(restaurantCopy.name);
console.log(restaurant.name);
```

### The Rest Operator Syntax

```JSX
// SPREAD the arr [3, 4] into separate variables, because on RIGHT side of =
const arr = [1, 2, ...[3, 4]];

// REST, because on LEFT side of =
const [a, b, ...others] = [1, 2, 3, 4, 5];
console.log(a, b, others); // 1 2 [3, 4, 5]. We get 1 and 2 as separate variables and the rest as a new array

const [pizza, , risotto, ...otherFood] = [
  ...restaurant.mainMenu,
  ...restaurant.starterMenu,
];
// ['Pizza' 'Risotto' ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad']
console.log(pizza, risotto, otherFood);
```

## for of loop

```JSX
const array1 = ['a', 'b', 'c'];

for (const element of array1) {
  console.log(element);
}
```

# Optional chaining

```JSX
for (const day of days) {
  const open = restaurant.openingHours[day]?.open ?? 'closed';
  console.log(`On ${day}, we open at ${open}`);
}
```

# Object methods

```JSx
Object.keys(obj) // returns an array of keys.
Object.values(obj) // returns an array of values.
Object.entries(obj) // returns an array of [key, value] pairs.
```

# Sets

```JSX
const ordersSet = new Set([
  'Pasta',
  'Pizza',
  'Pizza',
  'Risotto',
  'Pasta',
  'Pizza',
]);

// Duplicates are gone in a set, elements are unique and the order in a set is irrelevant
console.log(ordersSet);

console.log(ordersSet.size);
console.log(ordersSet.has('Pizza'));
console.log(ordersSet.has('Bread'));
ordersSet.add('Garlic Bread');
ordersSet.delete('Risotto');
// ordersSet.clear(); // delete all elements in the set

const staff = ['Waiter', 'Chef', 'Waiter', 'Manager', 'Chef', 'Waiter'];
const staffUnique = [...new Set(staff)];
```

# Maps

## Fundamentals

```JSX
const rest = new Map();
rest.set('name', 'Classico Italiano');
rest.set(1, 'Firenze, Italy');
console.log(rest.set(2, 'Lisbon, Portugal'));

rest
  .set('categories', ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'])
  .set('open', 11)
  .set('close', 23)
  .set(true, 'We are open :D')
  .set(false, 'We are closed :(');

console.log(rest.get('name'));
console.log(rest.get(true));
console.log(rest.get(1));

const time = 8;
console.log(rest.get(time > rest.get('open') && time < rest.get('close')));

console.log(rest.has('categories'));
rest.delete(2);
// rest.clear();

const arr = [1, 2];
rest.set(arr, 'Test');
rest.set(document.querySelector('h1'), 'Heading');
console.log(rest);
console.log(rest.size);

console.log(rest.get(arr));
```

## Iteration

```JSX
const question = new Map([
  ['question', 'What is the best programming language in the world?'],
  [1, 'C'],
  [2, 'Java'],
  [3, 'JavaScript'],
  ['correct', 3],
  [true, 'Correct 🎉'],
  [false, 'Try again!'],
]);
console.log(question);

// Convert object to map
console.log(Object.entries(openingHours));
const hoursMap = new Map(Object.entries(openingHours));
console.log(hoursMap);

// Quiz app
console.log(question.get('question'));
for (const [key, value] of question) {
  if (typeof key === 'number') console.log(`Answer ${key}: ${value}`);
}
// const answer = Number(prompt('Your answer'));
const answer = 3;
console.log(answer);

console.log(question.get(question.get('correct') === answer));
```
