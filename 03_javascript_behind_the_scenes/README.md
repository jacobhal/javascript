# Section 3: JavaScript Behind the Scenes

## How Our Code is Executed
JavaScript is always executed in an environment, typically in browsers.
There can also be other hosts, such as Node.js.

A JavaScript engine such as Google's V8, Reno or Spidermonkey is used to execute the JavaScript code:
1. The code is parsed in the engine line by line and checks if the syntax is correct.
2. The parser produces an Abstract Syntax Tree.
3. The tree is translated to machine code.

## Execution Contexts and the Execution Stack
### Execution Context
JavaScript is always run in an execution context (scope).

> The default execution context is the global execution context which is associated with the global object. The global object is the window object in a browser.

The execution context object has three properties: 
* Variable Object (VO): contains function arguments, inner variable declarations and function declarations
* Scope Chain: contains the current variable objects and the variable objects of all its parents
* "this" variable

The creation of an execution context is done in two phases:
1. Creation phase
    * Creation of the Variable Object (VO)
        * The *argument* object is created, containing all the arguments that were passed into the function
        * Code is scanned for **function declarations**: for each function, a property is created in the Variable Object, **pointing to the function**
        * Code is scanned for **variable declarations**: for each function, a property is created in the Variable Object, and set to *undefined*
    * Creation of the Scope Chain
        * All the variable objects that an execution context has access to
    * Determine the value of the 'this' variable
2. Execution phase
    * The code in the function that generated the executed context is executed line by line

#### Hoisting
The last two steps of the Varible Object creation is called *Hoisting*. Functions and variables are hoisted in JavaScript, which means that they are available before code execution starts. 

However, functions and variables are hoisted in different ways since functions are already defined before the code execution starts while variables are set to undefined and will only be defined in the execution phase.

> In practice, you can access javascript functions/variables before they are declared in a .js file because the declarations are scanned and hoisted before the execution starts. However, this does not work for function expressions (when you create a variable and set it to a function).

#### Scoping
Scoping in JavaScript means exactly what it does in any other programming language: where we can access certain variables.

JavaScript scoping works as follows:
* In JavaScript, each new function creates a scope in which the variables it defines are accessible.
* Lexical scoping: a function that is lexically within another function gets access to the scope of the outer function.

![Image not found](https://github.com/jacobhal/javascript/blob/master/03_javascript_behind_the_scenes/execution-stack-vs-scope-chain.png)

#### The 'this' keyword
The this variable is a variable that each and every execution context gets and it is stored in the execution context object. 

Where does the 'this' keyword point:
* **Regular function call**: the *this* keyword points at the global object.
* **Method call**: the *this* keyword points to the object that is calling the method.

> The *this* keyword is not assigned a value until the function where it is defined is actually called.

```JSX
calculateAge(1985);

function calculateAge(year) {
    console.log(2016 - year);
    console.log(this); // this refers to the global window object in a regular function call
}

var john = {
    name: 'John',
    yearOfBirth: 1990,
    calculateAge: function() {
        console.log(this); // this refers to the object that is calling the method in a method call
        console.log(2016 - this.yearOfBirth);
        
        function innerFunction() {
            console.log(this); // this refers to the global window object in a regular function call
        }
        innerFunction();
    }
}

john.calculateAge();

var mike = {
    name: 'Mike',
    yearOfBirth: 1984
};


mike.calculateAge = john.calculateAge; // method borrowing, apply john's method to mike
mike.calculateAge(); // this will refer to mike's object in this case
```

### Execution Stack
Contexts are placed on top of each other in an Execution Stack.

The execution stack for the code example below would look like this when we are inside the third function:


| Execution Stack |
------------------
| third()         |
| second()        | 
| first()         |
| Global          |

As functions finish, their contexts gets removed.

```JSX
function first() {
    var a = 'Hello';
    second();
    var x = a + name;
}

function second() {
    var b = 'Hi';
    third();
    var z = b + name;
}

function third() {
    var c = 'Hey';
    var z = c + name;
}

first();
```
