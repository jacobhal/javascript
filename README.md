# Javascript tips

## Console log
Use `console.log({<variable-name>})` all the time in order to know what you are logging.

## Function definitions
Use `function printName({firstName, lastName, middleName}){}` with brackets around the parameters in order to create a function that destructures the object you send in automatically. In this way, the order of the parameters does not matter.

### Basic Arrow Functions
An *arrow function* expression is a compact alternative to a traditional function expression, but is limited and can't be used in all situations.

Differences & Limitations:

* Does not have its own bindings to this or super, and should not be used as methods.
* Does not have arguments, or new.target keywords.
* Not suitable for call, apply and bind methods, which generally rely on establishing a scope.
* Can not be used as constructors.
* Can not use yield, within its body.

## Array methods
Array methods like splice changes the array in all places. Instead, assign a new array with `const arr2 = [...arr]`.

## lodash
Lodash is the most useful javascript library!

### cloneDeep
```javascript
const user = {
    id: 1,
    details: {
        fullName: 'Jacob Hallman',
        siblings: 0
    }
}

const userCopy = Object.assign({}, user);
```

Object.assign only creates a shallow copy, meaning that any changes deeper than the first level will be changed in both the copy and the original object. 

Use cloneDeep from lodash instead:
```javascript
import {cloneDeep} from 'lodash';

const userCopy = cloneDeep(user);
```

### debounce
If you have an ajax search and make server requests on every keystroke, it is very computation-heavy. We might need a debounce function in order to only send requests when the user has stopped typing.
Debounce takes the last result that the user typed.

```javascript
import {debounce} from 'lodash';

function getSuggestions(str) {
    // Make GET request to server
}

const getDebounced = debounce(getSuggestions, 1000) 
```

### throttle
throttle is often used for rate-limiting. Let's say you have a form on your website that the user is going to fill out and then it will do something in your backend. You only want to receive that form once, that is where throttle comes in. If the user is clicking several times before it has a change to submit we can throttle the submit.
throttle takes the first request made within the time interval. 
If we submit the form multiple times before it has sent anything to the server, we only send the first one.

```javascript
import {throttle} from 'lodash';

function submitForm() {
	// POST request to server
}

const postThrottled = throttle(submitForm, 1000)
```

### get and set
If you access an object multiple levels deep like `user.profile.fullName` and profile is null, then your application is going to break which you don't want. The safer way to do this is to use lodash functions get and set.

```javascript
import {get, set} from 'lodash';

const user = {
    profile: {
        fullName: 'Jacob Hallman',
        age: 25
    }
}

const fullName = user.profile.fullName // Error if profile is null
const fullNameSafe = get(user, 'profile.fullName') // undefined if profile is null
const fullNameSafeDefaultValue = get(user, 'profile.fullName', '') // '' if profile is null

set(user, 'profile.fullName', 'New name') // Safely set the fullName property. Nothing happens if profile is null.
```

### capitalize
```javascript
import {capitalize} from 'lodash';

capitalize("jacob") // Jacob
```

### random
```javascript
import {random} from 'lodash';

_.random(0, 5); // Random number between 0 and 5
_.random(1.2, 5.2); // Random floating point number between 1.2 and 5.2
```

### filter
Many times we need to filter a collection based on some specific properties of the object.
You can also provide a function as the second parameter instead of an object. This comes very handy when you have multiple clauses or a complex filter logic.

For example: filter library members who are active. Using lodash, this can be simply written like:
```javascript
import {filter} from 'lodash';

_.filter(members, { active: true });

// filter library members who are active and older than 50 years. Below is the code to do that using lodash.
_.filter(memberList, function(member) { 
	return member.active && member.age > 50; 
});
```

### pick
Sometimes we would like to compose a new object by selecting only some specific properties of the original object. Instead of iterating through each property and picking matched property, lodash provides a simple utility method just for this purpose.

```javascript
import {pick} from 'lodash';

var member = { name: 'foo', age: 50, address: 'skyline road' };
 
var pickedMember = _.pick(member, ['name', 'age']);
console.log(pickedMember); // { name: 'foo', age: 50 }
});
```

### omit
This is just the opposite of the “pick” function. Instead of picking specific properties, “omit” function deletes the specified properties from the object.

This one can be useful when you want to streamline the response object returned from your API before it reaches the controller.

```javascript
import {omit} from 'lodash';

var member = { name: 'foo', age: 50, address: 'skyline road' };
 
var omittedMember = _.omit(member, [address']);
console.log(omittedMember); // { name: 'foo', age: 50 }
});
```

## Modules
```javascript
import {set as setLodash} from 'lodash' // Rename imported functions
```

```javascript
import {set} from 'lodash'

export function set() {} // Named exports gets turned into modules (only need to import this specific function)
```

```javascript
import _ from 'lodash' // Imports the whole library (does NOT use tree-shaking)

export function _() {}
```
