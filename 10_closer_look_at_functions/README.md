### First Class Functions: Passing Functions as Arguments
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

### First Class Functions: Functions Returning Functions
Functions can also return functions, look at the code example below:

```JSX
function interviewQuestion(job) {
    if (job === 'designer') {
        return function(name) {
            console.log(name + ', can you please explain what UX design is?');
        }
    } else if (job === 'teacher') {
        return function(name) {
            console.log('What subject do you teach, ' + name + '?');
        }
    } else {
        return function(name) {
            console.log('Hello ' + name + ', what do you do?');
        }
    }
}

var teacherQuestion = interviewQuestion('teacher');
var designerQuestion = interviewQuestion('designer');


teacherQuestion('John');
designerQuestion('John');
designerQuestion('jane');
designerQuestion('Mark');
designerQuestion('Mike');

interviewQuestion('teacher')('Mark'); // works just like the above, except we don't assign the function to a variable first
```

### Immediately Invoked Function Expressions (IIFE)
If we wrap a function in parantheses and omit the function name, we get an IIFE. This lets JavaScript know that it should treat it as an expression and not a function. The last parantheses is how we "invoke" the function in order to be an expression. 

IIFE's can be used for data privacy, there is no way we can access the score variable in the code example below for example. The variables we create in the IIFE scope do not interfere with global variables:

```JSX
(function () {
    var score = Math.random() * 10;
    console.log(score >= 5);
})();

//console.log(score);


(function (goodLuck) {
    var score = Math.random() * 10;
    console.log(score >= 5 - goodLuck);
})(5);
```

### Closures

### Bind, Call and Apply