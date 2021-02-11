# Javascript tips

## Console log
Use `console.log({<variable-name>})` all the time in order to know what you are logging.

## Function definitions
Use `function printName({firstName, lastName, middleName}){}` with brackets around the parameters in order to create a function that destructures the object you send in automatically. In this way, the order of the parameters does not matter.

### Traditional functions
Traditional functions default this to the window scope:

```JSX
window.age = 10; // <-- notice me?
function Person() {
  this.age = 42; // <-- notice me?
  setTimeout(function () { // <-- Traditional function is executing on the window scope
    console.log("this.age", this.age); // yields "10" because the function executes on the window scope
  }, 100);
}

var p = new Person();
```

Arrow functions do not default this to the window scope, rather they execute in the scope they are created:

```JSX
window.age = 10; // <-- notice me?
function Person() {
  this.age = 42; // <-- notice me?
  setTimeout(() => { // <-- Arrow function executing in the "p" (an instance of Person) scope
    console.log("this.age", this.age); // yields "42" because the function executes on the Person scope
  }, 100);
}

var p = new Person();
```

> In our example above, the arrow function does not have its own this. The this value of the enclosing lexical scope is used; arrow functions follow the normal variable lookup rules. So while searching for this which is not present in the current scope, an arrow function ends up finding the this from its enclosing scope

No matter how or where being executed, this value inside of an arrow function always equals this value from the outer function. In other words, the arrow function resolves this lexically. In another words, the arrow function doesn’t define its own execution context.

### Basic Arrow functions
An *arrow function* expression is a compact alternative to a traditional function expression, but is limited and can't be used in all situations.

```JSX
const calcAge = birthYear => 2037 - birthYear;
const yearsUntilRetirement = (birthYear, name) =>  {
    const age = 2037 - birthYear;
    const retirement = 65 - age; 
    return `${name} retires in ${retirement} years`;
}
```

Differences & Limitations:

* Does not have its own bindings to this or super, and should not be used as methods.
* Does not have arguments, or new.target keywords.
* Not suitable for call, apply and bind methods, which generally rely on establishing a scope.
* Can not be used as constructors.
* Can not use yield, within its body.

### IIFE (Immediately Invoked Function Expression)
An IIFE (Immediately Invoked Function Expression) is a JavaScript function that runs as soon as it is defined.

It is a design pattern which is also known as a Self-Executing Anonymous Function and contains two major parts:

1. The first is the anonymous function with lexical scope enclosed within the Grouping Operator (). This prevents accessing variables within the IIFE idiom as well as polluting the global scope.
2. The second part creates the immediately invoked function expression () through which the JavaScript engine will directly interpret the function.

```JSX
(function () {
    var aName = "Barry";
})();
// Variable aName is not accessible from the outside scope
aName // throws "Uncaught ReferenceError: aName is not defined"
```

The function becomes a function expression which is immediately executed. The variable within the expression can not be accessed from outside it.

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

## Operators

### Arithmetic operators
JavaScript has the arithmetic operators +, -, *, /, and %. These operators function as the addition, subtraction, multiplication, division, and modulus operators, and operate very similarly to other languages. Multiplication and division operators will be calculated before addition and subtraction. Operations in parenthesis will be calculated first.

```JSX
var a = 12 + 5;    // 17
var b = 12 - 5;    // 7
var c = 12*5;      // 60
var d = 12/5;      // 2.4 - division results in floating point numbers.
var e = 12%5;      // 2 - the remainder of 12/5 in integer math is 2.
var f = 5 -2 * 4   // -3 - multiplication is calculated first.
var g = (2+2) / 2  // 2 - Parenthesis are calculated first.
```

Some mathematical operations, such as dividing by zero, cause the returned variable to be one of the error values - for example, infinity, or NaN.

The return value of the modulus operator maintains the sign of the first operand.

The + and - operators also have unary versions, where they operate only on one variable. When used in this fashion, + returns the number representation of the object, while - returns its negative counterpart.

```JSX
var a = "1";
var b = a;     // b = "1": a string
var c = +a;    // c = 1: a number
var d = -a;    // d = -1: a number
```

+ is also used as the string concatenation operator: If any of its arguments is a string or is otherwise not a number, any non-string arguments are converted to strings, and the 2 strings are concatenated. For example, 5 + [1, 2, 3] evaluates to the string "51, 2, 3". More usefully, str1 + " " + str2 returns str1 concatenated with str2, with a space between.

All other arithmetic operators will attempt to convert their arguments into numbers before evaluating. Note that unlike C or Java, the numbers and their operation results are not guaranteed to be integers.

### Bitwise operators
There are seven bitwise operators: &, |, ^, ~, >>, <<, and >>>.

These operators convert their operands to integers (truncating any floating point towards 0), and perform the specified bitwise operation on them. The logical bitwise operators, &, |, and ^, perform the and, or, and xor on each individual bit and provides the return value. The ~ (not operator) inverts all bits within an integer, and usually appears in combination with the logical bitwise operators.

Two bit shift operators, >>, <<, move the bits in one direction that has a similar effect to multiplying or dividing by a power of two. The final bit-shift operator, >>>, operates the same way, but does not preserve the sign bit when shifting.

These operators are kept for parity with the related programming languages, but are unlikely to be used in most JavaScript programs.

### Assignment operators
The assignment operator = assigns a value to a variable. Primitive types, such as strings and numbers are assigned directly, however function and object names are just pointers to the respective function or object. In this case, the assignment operator only changes the reference to the object rather than the object itself. For example, after the following code is executed, "0, 1, 0" will be alerted, even though setA was passed to the alert, and setB was changed. This is, because they are two references to the same object.

```JSX
setA = [ 0, 1, 2 ];
setB = setA;
setB[2] = 0;
alert(setA);
```

Similarly, after the next bit of code is executed, x is a pointer to an empty array.

```JSX
z = [5];
x = z;
z.pop();
```

All the above operators have corresponding assignment operators of the form operator=. For all of them, x operator= y is just a convenient abbreviation for x = x operator y.

| Arithmetic | Logical | Shift |
|------------|---------|-------|
| +=         | &=      | >>=   |
| -=         | \|=     | <<=   |
| *=         | ^=      | >>>=  |
| /=         |         |       |
| %=         |         |       |

For example, a common usage for += is in a for loop

```JSX
var element = document.getElementsByTagName('h2');
var i;
for (i = 0; i < element .length; i += 1) {
  // do something with element [i]
}
```

### Increment operators
Increment operators
There are also the increment and decrement operators, ++ and --. a++ increments a and returns the old value of a. ++a increments a and returns the new value of a. The decrement operator functions similarly, but reduces the variable instead.

As an example, the last four lines all perform the same task:

```JSX
var a = 1;
a = a + 1;
a += 1;
a++;
++a;
```

**Pre and post-increment operators**  
Increment operators may be applied before or after a variable. When they are applied before or after a variable, they are pre-increment or post-increment operators, respectively. The choice of which to use changes how they affect operations.

```JSX
// increment occurs before a is assigned to b
var a = 1;
var b = ++a; // a = 2, b = 2;

// increment occurs to c after c is assigned to d
var c = 1;
var d = c++; // c = 2, d = 1;
Due to the possibly confusing nature of pre and post-increment behaviour, code can be easier to read, if the increment operators are avoided.

// increment occurs before a is assigned to b
var a = 1;
a += 1;
var b = a; // a = 2, b = 2;

// increment occurs to c after c is assigned to d
var c = 1;
var d = c;
c += 1; // c = 2, d = 1;
```

### Comparison operators
The comparison operators determine, if the two operands meet the given condition.

| Operator | Returns                                                               | Notes                                                                                           |
|----------|-----------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| ==       | true, if the two operands are equal                                   | May ignore operand's type  (e.g. a string as an integer)                                        |
| ===      | true, if the two operands are identical                               | Does not ignore operands' types, and only  returns true if they are the same type and value     |
| !=       | true, if the two operands are not equal                               | May ignore an operand's type  (e.g. a string as an integer)                                     |
| !==      | true, if the two operands are not identical                           | Does not ignore the operands' types, and only returns false if they are the same type and value |
| >        | true, if the first operand is greater than the second one             |                                                                                                 |
| >=       | true, if the first operand is greater than or equal to the second one |                                                                                                 |
| <        | true, if the first operand is less than the second one                |                                                                                                 |
| <=       | true, if the first operand is less than or equal to the second one    |                                                                                                 |


Be careful when using == and !=, as they may ignore the type of one of the terms being compared. This can lead to strange and non-intuitive situations, such as:

```JSX
0 == '' // true
0 == '0' // true
false == 'false' // false; (''Boolean to string'')
false == '0' // true (''Boolean to string'')
false == undefined // false
false == null // false (''Boolean to null'')
null == undefined // true
```

For stricter compares use === and !==

```JSX
0 === '' // false
0 === '0' // false
false === 'false' // false
false === '0' // false
false === undefined // false
false === null // false
null === undefined // false
```

### Logical operators
* && - and
* || - or
* ! - not
The logical operators are and, or, and not. The && and || operators accept two operands and provides their associated logical result, while the third accepts one, and returns its logical negation. && and || are short circuit operators. If the result is guaranteed after evaluation of the first operand, it skips evaluation of the second operand.

Technically, the exact return value of these two operators is also equal to the final operand that it evaluated. Due to this, the && operator is also known as the guard operator, and the || operator is also known as the default operator.

```JSX
function handleEvent(event) {
    event = event || window.event;
    var target = event.target || event.srcElement;
    if (target && target.nodeType === 1 && target.nodeName === 'A') {
        // ...
    }
}
```

The ! operator determines the inverse of the given value, and returns the boolean: true values become false, or false values become true.

**Note:** JavaScript represents false by either a Boolean false, the number 0, NaN, an empty string, or the built in undefined or null type. Any other value is treated as true.

### Other operators
**? :**
The ? : operator (also called the "ternary" operator).

```JSX
var target = (a == b) ? c : d;
```

Be cautious though in its use. Even though you can replace verbose and complex if/then/else chains with ternary operators, it may not be a good idea to do so. You can replace

```JSX
if (p && q) {
    return a;
} else {
    if (r != s) {
        return b;
    } else {
        if (t || !v) {
            return c;
        } else {
            return d;
        }
    }
}
```
with

```JSX
return (p && q) ? a
  : (r != s) ? b
  : (t || !v) ? c
  : d
```
The above example is a poor coding style/practice. When other people edit or maintain your code, (which could very possibly be you,) it becomes much more difficult to understand and work with the code.

Instead, it is better to make the code more understandable. Some of the excessive conditional nesting can be removed from the above example.

```JSX
if (p && q) {
    return a;
}
if (r != s) {
    return b;
}
if (t || !v) {
    return c;
} else {
    return d;
}
```

**delete**
delete x unbinds x.

The delete keyword deletes a property from an object. The delete keyword deletes both the value of the property and the property itself. After deletion, the property cannot be used before it is added back again. The delete operator is designed to be used on object properties. It has no effect on variables or functions.

**new**
new cl creates a new object of type cl. The cl operand must be a constructor function.

**instanceof**
o instanceof c tests whether o is an object created by the constructor c.

**typeof**
typeof x returns a string describing the type of x. Following values may be returned:

| Type       | returns     |
|------------|------------ |
| boolean    | "boolean"   |
| number     | "number"    |
| string     | "string"    |
| function   | "function"  |
| undefined  | "undefined" |
| null       | "object"    |
| others     | "object"    |
