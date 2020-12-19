# Section 14: OOP With JavaScript

Table of Contents
=================

   * [Section 5: Objects and Functions](#section-5-objects-and-functions)
      * [Primitives and Objects](#primitives-and-objects)
      * [Inheritance](#inheritance)
         * [Prototypes](#prototypes)
            * [Summary](#summary)
      * [Coding with Objects and Functions](#coding-with-objects-and-functions)
         * [Function Constructors](#function-constructors)
         * [Prototypes in the console](#prototypes-in-the-console)
         * [Creating Objects: Object.create](#creating-objects-objectcreate)
         * [Primitives vs Objects](#primitives-vs-objects)

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

![Image not found](https://github.com/jacobhal/javascript/blob/master/14_oop_with_javascript/prototype-chain.png)

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
With Object.create we first define an Object which will act as the prototype and then call the create method from it in order to create a new object.

Object.create is useful for creating more complex inheritance structures.

In the code example below, we see some ways to use Object.create:

```JSX
var personProto = {
    calculateAge: function() {
        console.log(2016 - this.yearOfBirth);
    }
};

var john = Object.create(personProto);
john.name = 'John';
john.yearOfBirth = 1990;
john.job = 'teacher';

var jane = Object.create(personProto, {
    name: { value: 'Jane' },
    yearOfBirth: { value: 1969 },
    job: { value: 'designer' }
});
```

> The difference between Object.create and Function constructors is that Object.create inherits directly from the object passed as the first argument while the newly created object from Function constructors inherits from the prototype property.

### Primitives vs Objects
Variables containing primitives hold the data inside the variable itself.

Variables associated with objects does not contain the object, but a reference to a place in memory where the object is stired.

We can also see that this is true for functions in the code example below:
```JSX
// Primitives
var a = 23;
var b = a;
a = 46;
console.log(a);
console.log(b);

// Objects
var obj1 = {
    name: 'John',
    age: 26
};
var obj2 = obj1;
obj1.age = 30;
console.log(obj1.age);
console.log(obj2.age);

// Functions
var age = 27;
var obj = {
    name: 'Jonas',
    city: 'Lisbon'
};

function change(a, b) {
    a = 30;
    b.city = 'San Francisco';
}

change(age, obj);

console.log(age); // 27
console.log(obj.city); // San Francisco
```



