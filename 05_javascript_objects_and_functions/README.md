# Section 5: Objects and Functions

## Primitives and Objects
In JavaScript we have two types of values, primitives and objects.

**Primitives**
* Numbers
* Strings
* Booleans
* undefined
* null

**Objects**
* Arrays
* Functions
* Objects
* Dates
* Wrappers for Numbers, Strings, Booleans

> Almost everything in JavaScript are Objects.

## Inheritance
Everyone knows inheritance from OOPLs but it exists in JavaScript as well.

### Prototypes
JavaScript is a prototyped-based language. Each and every JavaScript object has a prototype property. Inheritance is made possible through this property.

> Properties and functions that we want other objects to inherit are put into the prototype property.

Every object in JavaScript inherits from the Object object which has a prototype property filled with general methods that can be called on objects such as toString() or valueOf().

This whole sequence of having objects inherit things through the prototype properties is called the *Prototype chain*.

![Image not found](https://github.com/jacobhal/javascript/blob/master/05_javascript_objects_and_functions/prototype-chain.png)

The prototype chain works like this:
1. When we try to access a certain method or property on an object, JavaScript will try to find the method/property on that exact object.
2. If it is not available on the object, it looks in its prototype which is the prototype property of its parent.
3. This process repeats until we reach null which is the only one which does not have a prototype.

#### Summary
* Every JavaScript has a **prototype property**, which makes inheritance possible in JavaScript.
* The prototype property of an object is where we put methods and properties that we want **other objects to inherit**.
* The Constructor's prototype property is **NOT** the prototype of the Constructor itself, it's the prototype of **ALL** instances that are created through it.
* When a certain method (or property) is called, the search starts in the object itself, and if it cannot be found, the search moves on to the object's prototype. This continues until the method is found: **prototype chain**.

## Coding with Objects and Functions

### Function Constructors
If we want a "blueprint" for creating objects, for instance Person objects, we can use Function Constructors.

In the code below "Person" is the function constructor, from which we can instantiate new Person objects. The "new" operator makes "this" point to the newly created empty object instead of the global object.

```JSX
var john = {
    name: 'John',
    yearOfBirth: 1990,
    job: 'teacher'
};

var Person = function(name, yearOfBirth, job) {
    this.name = name;
    this.yearOfBirth = yearOfBirth;
    this.job = job;
}

Person.prototype.calculateAge  = function() {
    console.log(2016 - this.yearOfBirth);
};

Person.prototype.lastName = 'Smith';

var john = new Person('John', 1990, 'teacher');
var jane = new Person('Jane', 1969, 'designer');
var mark = new Person('Mark', 1948, 'retired');

// Available through prototype inheritance
john.calculateAge();
jane.calculateAge();
mark.calculateAge();

// Available through prototype inheritance
console.log(john.lastName);
console.log(jane.lastName);
console.log(mark.lastName);
```

> Note that we have access to calculateAge and lastName because we defined them in the prototype property. In this case it is rather "unnecessary" but if we had something like 20 functions on the Person object with 100 lines of code for each, there would be alot of copies of functions attached to each object.

### Prototypes in the console
In order to see what things an object inherits, you can look at the prototype property. Either write <Object-constructor-name>.prototype in the console or console.log the entire instantiated object and expand the "__proto__" tree.

Tips and tricks on instances of objects:
* `obj.hasOwnProperty(<property-name>)` checks if obj has a property with the given name defined on the object directly.
* `obj instanceof Person` checks if obj is an instance of the Person constructor in this case.

> `console.info(<variable-name>)` can be used to check the full object definition for variables. You will notice that data structures like arrays also has prototypes defined in order to give us access to the generic functions/properties that are already available on arrays: length, splice, indexOf etc.

### Creating Objects: Object.create

### Primitives vs Objects

### First Class Functions: Passing Functions as Arguments

### First Class Functions: Functions Returning Functions

### Immediately Invoked Function Expressions (IIFE)

### Closures

### Bind, Call and Apply



