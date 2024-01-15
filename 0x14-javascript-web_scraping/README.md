Introduction
Motivation
This document is a cheatsheet for JavaScript you will frequently encounter in modern projects and most contemporary sample code.

This guide is not intended to teach you JavaScript from the ground up, but to help developers with basic knowledge who may struggle to get familiar with modern codebases (or let's say to learn React for instance) because of the JavaScript concepts used.

Besides, I will sometimes provide personal tips that may be debatable but will take care to mention that it's a personal recommendation when I do so.

Note: Most of the concepts introduced here are coming from a JavaScript language update (ES2015, often called ES6). You can find new features added by this update here; it's very well done.

Complementary Resources
When you struggle to understand a notion, I suggest you look for answers on the following resources:

MDN (Mozilla Developer Network)
You don't know JS (book)
Eloquent JavaScript (book)
Douglas Crockford's blog
ES6 Features with examples
Wes Bos blog (ES6)
Javascript Basics for Beginners - a free Udacity course
Reddit (JavaScript)
Google to find specific blog and resources
StackOverflow
Table of Contents
Modern JavaScript cheatsheet
Introduction
Motivation
Complementary resources
Table of contents
Notions
Variable declaration: var, const, let
Short explanation
Sample code
Detailed explanation
External resource
Arrow function
Sample code
Detailed explanation
Concision
this reference
Useful resources
Function default parameter value
External resource
Destructuring objects and arrays
Explanation with sample code
Useful resources
Array methods - map / filter / reduce
Sample code
Explanation
Array.prototype.map()
Array.prototype.filter()
Array.prototype.reduce()
Array.prototype.find()
External Resource
Spread operator "..."
Sample code
Explanation
In iterables (like arrays)
Function rest parameter
Object properties spreading
External resources
Object property shorthand
Explanation
External resources
Promises
Sample code
Explanation
Create the promise
Promise handlers usage
External Resources
Template literals
Sample code
External resources
Tagged Template Literals
External resources
Imports / Exports
Explanation with sample code
Named exports
Default import / export
External resources
JavaScript this
External resources
Class
Samples
External resources
Extends and super keywords
Sample Code
External Resources
Async Await
Sample code
Explanation with sample code
Error handling
External resources
Truthy / Falsy
External resources
Anamorphisms / Catamporphisms
Anamorphisms
Catamorphisms
External resources
Generators
External resources
Static Methods
Short Explanation
Sample Code
Detailed Explanation
Calling other static methods from a static method
Calling static methods from non-static methods
External resources
Glossary
Scope
Variable mutation
Notions
Variable declaration: var, const, let
In JavaScript, there are three keywords available to declare a variable, and each has its differences. Those are var, let and const.

Short explanation
Variables declared with const keyword can't be reassigned, while let and var can.

I recommend always declaring your variables with const by default, but with let if it is a variable that you need to mutate or reassign later.

Scope	Reassignable	Mutable	Temporal Dead Zone
const	Block	No	Yes	Yes
let	Block	Yes	Yes	Yes
var	Function	Yes	Yes	No
Sample code
const person = "Nick";
person = "John" // Will raise an error, person can't be reassigned
let person = "Nick";
person = "John";
console.log(person) // "John", reassignment is allowed with let
Detailed explanation
The scope of a variable roughly means "where is this variable available in the code".

var
var declared variables are function scoped, meaning that when a variable is created in a function, everything in that function can access that variable. Besides, a function scoped variable created in a function can't be accessed outside this function.

I recommend you to picture it as if an X scoped variable meant that this variable was a property of X.

function myFunction() {
  var myVar = "Nick";
  console.log(myVar); // "Nick" - myVar is accessible inside the function
}
console.log(myVar); // Throws a ReferenceError, myVar is not accessible outside the function.
Still focusing on the variable scope, here is a more subtle example:

function myFunction() {
  var myVar = "Nick";
  if (true) {
    var myVar = "John";
    console.log(myVar); // "John"
    // actually, myVar being function scoped, we just erased the previous myVar value "Nick" for "John"
  }
  console.log(myVar); // "John" - see how the instructions in the if block affected this value
}
console.log(myVar); // Throws a ReferenceError, myVar is not accessible outside the function.
Besides, var declared variables are moved to the top of the scope at execution. This is what we call var hoisting.

This portion of code:

console.log(myVar) // undefined -- no error raised
var myVar = 2;
is understood at execution like:

var myVar;
console.log(myVar) // undefined -- no error raised
myVar = 2;
let
var and let  are about the same, but let declared variables

are block scoped
are not accessible before they are assigned
can't be re-declared in the same scope
Let's see the impact of block-scoping taking our previous example:

function myFunction() {
  let myVar = "Nick";
  if (true) {
    let myVar = "John";
    console.log(myVar); // "John"
    // actually, myVar being block scoped, we just created a new variable myVar.
    // this variable is not accessible outside this block and totally independent
    // from the first myVar created !
  }
  console.log(myVar); // "Nick", see how the instructions in the if block DID NOT affect this value
}
console.log(myVar); // Throws a ReferenceError, myVar is not accessible outside the function.
Now, what it means for let (and const) variables for not being accessible before being assigned:

console.log(myVar) // raises a ReferenceError !
let myVar = 2;
By contrast with var variables, if you try to read or write on a let or const variable before they are assigned an error will be raised. This phenomenon is often called Temporal dead zone or TDZ.

Note: Technically, let and const variables declarations are being hoisted too, but not their assignation. Since they're made so that they can't be used before assignation, it intuitively feels like there is no hoisting, but there is. Find out more on this very detailed explanation here if you want to know more.

In addition, you can't re-declare a let variable:

let myVar = 2;
let myVar = 3; // Raises a SyntaxError
const
const declared variables behave like let variables, but also they can't be reassigned.

To sum it up, const variables:

are block scoped
are not accessible before being assigned
can't be re-declared in the same scope
can't be reassigned
const myVar = "Nick";
myVar = "John" // raises an error, reassignment is not allowed
const myVar = "Nick";
const myVar = "John" // raises an error, re-declaration is not allowed
But there is a subtlety : const variables are not immutable ! Concretely, it means that object and array const declared variables can be mutated.

For objects:

const person = {
  name: 'Nick'
};
person.name = 'John' // this will work ! person variable is not completely reassigned, but mutated
console.log(person.name) // "John"
person = "Sandra" // raises an error, because reassignment is not allowed with const declared variables
For arrays:

const person = [];
person.push('John'); // this will work ! person variable is not completely reassigned, but mutated
console.log(person[0]) // "John"
person = ["Nick"] // raises an error, because reassignment is not allowed with const declared variables
External resource
How let and const are scoped in JavaScript - WesBos
Temporal Dead Zone (TDZ) Demystified
Arrow function
The ES6 JavaScript update has introduced arrow functions, which is another way to declare and use functions. Here are the benefits they bring:

More concise
this is picked up from surroundings
implicit return
Sample code
Concision and implicit return
function double(x) { return x * 2; } // Traditional way
console.log(double(2)) // 4
const double = x => x * 2; // Same function written as an arrow function with implicit return
console.log(double(2)) // 4
this reference
In an arrow function, this is equal to the this value of the enclosing execution context. Basically, with arrow functions, you don't have to do the "that = this" trick before calling a function inside a function anymore.

function myFunc() {
  this.myVar = 0;
  setTimeout(() => {
    this.myVar++;
    console.log(this.myVar) // 1
  }, 0);
}
Detailed explanation
Concision
Arrow functions are more concise than traditional functions in many ways. Let's review all the possible cases:

Implicit VS Explicit return
An explicit return is a function where the return keyword is used in its body.

  function double(x) {
    return x * 2; // this function explicitly returns x * 2, *return* keyword is used
  }
In the traditional way of writing functions, the return was always explicit. But with arrow functions, you can do implicit return which means that you don't need to use the keyword return to return a value.

  const double = (x) => {
    return x * 2; // Explicit return here
  }
Since this function only returns something (no instructions before the return keyword) we can do an implicit return.

  const double = (x) => x * 2; // Correct, returns x*2
To do so, we only need to remove the brackets and the return keyword. That's why it's called an implicit return, the return keyword is not there, but this function will indeed return x * 2.

Note: If your function does not return a value (with side effects), it doesn't do an explicit nor an implicit return.

Besides, if you want to implicitly return an object you must have parentheses around it since it will conflict with the block braces:

const getPerson = () => ({ name: "Nick", age: 24 })
console.log(getPerson()) // { name: "Nick", age: 24 } -- object implicitly returned by arrow function
Only one argument
If your function only takes one parameter, you can omit the parentheses around it. If we take back the above double code:

  const double = (x) => x * 2; // this arrow function only takes one parameter
Parentheses around the parameter can be avoided:

  const double = x => x * 2; // this arrow function only takes one parameter
No arguments
When there is no argument provided to an arrow function, you need to provide parentheses, or it won't be valid syntax.

  () => { // parentheses are provided, everything is fine
    const x = 2;
    return x;
  }
  => { // No parentheses, this won't work!
    const x = 2;
    return x;
  }
this reference
To understand this subtlety introduced with arrow functions, you must know how this behaves in JavaScript.

In an arrow function, this is equal to the this value of the enclosing execution context. What it means is that an arrow function doesn't create a new this, it grabs it from its surrounding instead.

Without arrow function, if you wanted to access a variable from this in a function inside a function, you had to use the that = this or self = this trick.

For instance, using setTimeout function inside myFunc:

function myFunc() {
  this.myVar = 0;
  var that = this; // that = this trick
  setTimeout(
    function() { // A new *this* is created in this function scope
      that.myVar++;
      console.log(that.myVar) // 1

      console.log(this.myVar) // undefined -- see function declaration above
    },
    0
  );
}
But with arrow function, this is taken from its surrounding:

function myFunc() {
  this.myVar = 0;
  setTimeout(
    () => { // this taken from surrounding, meaning myFunc here
      this.myVar++;
      console.log(this.myVar) // 1
    },
    0
  );
}
Useful resources
Arrow functions introduction - WesBos
JavaScript arrow function - MDN
Arrow function and lexical this
Function default parameter value
Starting from ES2015 JavaScript update, you can set default value to your function parameters using the following syntax:

function myFunc(x = 10) {
  return x;
}
console.log(myFunc()) // 10 -- no value is provided so x default value 10 is assigned to x in myFunc
console.log(myFunc(5)) // 5 -- a value is provided so x is equal to 5 in myFunc

console.log(myFunc(undefined)) // 10 -- undefined value is provided so default value is assigned to x
console.log(myFunc(null)) // null -- a value (null) is provided, see below for more details
The default parameter is applied in two and only two situations:

No parameter provided
undefined parameter provided
In other words, if you pass in null the default parameter won't be applied.

Note: Default value assignment can be used with destructured parameters as well (see next notion to see an example)

External resource
Default parameter value - ES6 Features
Default parameters - MDN
Destructuring objects and arrays
Destructuring is a convenient way of creating new variables by extracting some values from data stored in objects or arrays.

To name a few use cases, destructuring can be used to destructure function parameters or this.props in React projects for instance.

Explanation with sample code
Object
Let's consider the following object for all the samples:

const person = {
  firstName: "Nick",
  lastName: "Anderson",
  age: 35,
  sex: "M"
}
Without destructuring

const first = person.firstName;
const age = person.age;
const city = person.city || "Paris";
With destructuring, all in one line:

const { firstName: first, age, city = "Paris" } = person; // That's it !

console.log(age) // 35 -- A new variable age is created and is equal to person.age
console.log(first) // "Nick" -- A new variable first is created and is equal to person.firstName
console.log(firstName) // ReferenceError -- person.firstName exists BUT the new variable created is named first
console.log(city) // "Paris" -- A new variable city is created and since person.city is undefined, city is equal to the default value provided "Paris".
Note : In const { age } = person;, the brackets after const keyword are not used to declare an object nor a block but is the destructuring syntax.

Function parameters
Destructuring is often used to destructure objects parameters in functions.

Without destructuring

function joinFirstLastName(person) {
  const firstName = person.firstName;
  const lastName = person.lastName;
  return firstName + '-' + lastName;
}

joinFirstLastName(person); // "Nick-Anderson"
In destructuring the object parameter person, we get a more concise function:

function joinFirstLastName({ firstName, lastName }) { // we create firstName and lastName variables by destructuring person parameter
  return firstName + '-' + lastName;
}

joinFirstLastName(person); // "Nick-Anderson"
Destructuring is even more pleasant to use with arrow functions:

const joinFirstLastName = ({ firstName, lastName }) => firstName + '-' + lastName;

joinFirstLastName(person); // "Nick-Anderson"
Array
Let's consider the following array:

const myArray = ["a", "b", "c"];
Without destructuring

const x = myArray[0];
const y = myArray[1];
With destructuring

const [x, y] = myArray; // That's it !

console.log(x) // "a"
console.log(y) // "b"
Useful resources
ES6 Features - Destructuring Assignment
Destructuring Objects - WesBos
ExploringJS - Destructuring
Array methods - map / filter / reduce / find
Map, filter, reduce and find are array methods that are coming from a programming paradigm named functional programming.

To sum it up:

Array.prototype.map() takes an array, does something on its elements and returns an array with the transformed elements.
Array.prototype.filter() takes an array, decides element by element if it should keep it or not and returns an array with the kept elements only
Array.prototype.reduce() takes an array and aggregates the elements into a single value (which is returned)
Array.prototype.find() takes an array, and returns the first element that satisfies the provided condition.
I recommend to use them as much as possible in following the principles of functional programming because they are composable, concise and elegant.

With those four methods, you can avoid the use of for and forEach loops in most situations. When you are tempted to do a for loop, try to do it with map, filter, reduce and find composed. You might struggle to do it at first because it requires you to learn a new way of thinking, but once you've got it things get easier.

Sample code
const numbers = [0, 1, 2, 3, 4, 5, 6];
const doubledNumbers = numbers.map(n => n * 2); // [0, 2, 4, 6, 8, 10, 12]
const evenNumbers = numbers.filter(n => n % 2 === 0); // [0, 2, 4, 6]
const sum = numbers.reduce((prev, next) => prev + next, 0); // 21
const greaterThanFour = numbers.find((n) => n>4); // 5
Compute total grade sum for students with grades 10 or above by composing map, filter and reduce:

const students = [
  { name: "Nick", grade: 10 },
  { name: "John", grade: 15 },
  { name: "Julia", grade: 19 },
  { name: "Nathalie", grade: 9 },
];

const aboveTenSum = students
  .map(student => student.grade) // we map the students array to an array of their grades
  .filter(grade => grade >= 10) // we filter the grades array to keep those 10 or above
  .reduce((prev, next) => prev + next, 0); // we sum all the grades 10 or above one by one

console.log(aboveTenSum) // 44 -- 10 (Nick) + 15 (John) + 19 (Julia), Nathalie below 10 is ignored
Explanation
Let's consider the following array of numbers for our examples:

const numbers = [0, 1, 2, 3, 4, 5, 6];
Array.prototype.map()
const doubledNumbers = numbers.map(function(n) {
  return n * 2;
});
console.log(doubledNumbers); // [0, 2, 4, 6, 8, 10, 12]
What's happening here? We are using .map on the numbers array, the map is iterating on each element of the array and passes it to our function. The goal of the function is to produce and return a new value from the one passed so that map can replace it.

Let's extract this function to make it more clear, just for this once:

const doubleN = function(n) { return n * 2; };
const doubledNumbers = numbers.map(doubleN);
console.log(doubledNumbers); // [0, 2, 4, 6, 8, 10, 12]
Note : You will frequently encounter this method used in combination with arrow functions

const doubledNumbers = numbers.map(n => n * 2);
console.log(doubledNumbers); // [0, 2, 4, 6, 8, 10, 12]
numbers.map(doubleN) produces [doubleN(0), doubleN(1), doubleN(2), doubleN(3), doubleN(4), doubleN(5), doubleN(6)] which is equal to [0, 2, 4, 6, 8, 10, 12].

Note: If you do not need to return a new array and just want to do a loop that has side effects, you might just want to use a for / forEach loop instead of a map.

Array.prototype.filter()
const evenNumbers = numbers.filter(function(n) {
  return n % 2 === 0; // true if "n" is par, false if "n" isn't
});
console.log(evenNumbers); // [0, 2, 4, 6]
Note : You will frequently encounter this method used in combination with arrow functions

const evenNumbers = numbers.filter(n => n % 2 === 0);
console.log(evenNumbers); // [0, 2, 4, 6]
We are using .filter on the numbers array, filter is iterating on each element of the array and passes it to our function. The goal of the function is to return a boolean that will determine whether the current value will be kept or not. Filter then returns the array with only the kept values.

Array.prototype.reduce()
The reduce method goal is to reduce all elements of the array it iterates on into a single value. How it aggregates those elements is up to you.

const sum = numbers.reduce(
  function(acc, n) {
    return acc + n;
  },
  0 // accumulator variable value at first iteration step
);

console.log(sum) // 21
Note : You will frequently encounter this method used in combination with arrow functions

const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum) // 21
Just like for .map and .filter methods, .reduce is applied on an array and takes a function as the first parameter.

This time though, there are changes:

.reduce takes two parameters
The first parameter is a function that will be called at each iteration step.

The second parameter is the value of the accumulator variable (acc here) at the first iteration step (read next point to understand).

Function parameters
The function you pass as the first parameter of .reduce takes two parameters. The first one (acc here) is the accumulator variable, whereas the second parameter (n) is the current element.

The accumulator variable is equal to the return value of your function at the previous iteration step. At the first step of the iteration, acc is equal to the value you passed as .reduce second parameter.

At first iteration step
acc = 0 because we passed in 0 as the second parameter for reduce

n = 0 first element of the number array

Function returns acc + n --> 0 + 0 --> 0

At second iteration step
acc = 0 because it's the value the function returned at the previous iteration step

n = 1 second element of the number array

Function returns acc + n --> 0 + 1 --> 1

At third iteration step
acc = 1 because it's the value the function returned at the previous iteration step

n = 2 third element of the number array

Function returns acc + n --> 1 + 2 --> 3

At fourth iteration step
acc = 3 because it's the value the function returned at the previous iteration step

n = 3 fourth element of the number array

Function returns acc + n --> 3 + 3 --> 6

[...] At last iteration step
acc = 15 because it's the value the function returned at the previous iteration step

n = 6 last element of the number array

Function returns acc + n --> 15 + 6 --> 21

As it is the last iteration step, .reduce returns 21.

Array.prototype.find()
const greaterThanZero = numbers.find(function(n) {
  return n > 0; // return number just greater than 0 is present
});
console.log(greaterThanZero); // 1
Note : You will frequently encounter this method used in combination with arrow functions

We are using .find on the numbers array, .find is iterating on each element of the array and passes it to our function, until the condition is met. The goal of the function is to return the element that satisfies the current testing function. The .find method executes the callback function once for each index of the array until the callback returns a truthy value.

Note : It immediately returns the value of that element (that satisfies the condition) if found. Otherwise, returns undefined.

External Resource
Understanding map / filter / reduce in JS
