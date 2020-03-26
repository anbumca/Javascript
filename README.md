# ES6 for Humans

<br>

### Table of Contents

* [`let`, `const` and block scoping](#1-let-const-and-block-scoping)
* [Arrow Functions](#2-arrow-functions)
* [Default Function Parameters](#3-default-function-parameters)
* [Spread/Rest Operator](#4-spread--rest-operator)
* [Object Literal Extensions](#5-object-literal-extensions)
* [Octal and Binary Literals](#6-octal-and-binary-literals)
* [Array and Object Destructuring](#7-array-and-object-destructuring)
* [super in Objects](#8-super-in-objects)
* [Template Literal and Delimiters](#9-template-literal-and-delimiters)
* [for...of vs for...in](#10-forof-vs-forin)
* [Map and WeakMap](#11-map-and-weakmap)
* [Set and WeakSet](#12-set-and-weakset)
* [Classes in ES6](#13-classes-in-es6)
* [Symbol](#14-symbol)
* [Iterators](#15-iterators)
* [Generators](#16-generators)
* [Promises](#17-promises)

<br>

### 1. let, const and block scoping

`let` allows you to create declarations which are bound to any block, called block scoping. Instead of using `var`, which provides function scope, it is recommended to use block scoped variables (`let` or `const`) in ES6.

```javascript
var a = 2;
{
    let a = 3;
    console.log(a); // 3
    let a = 5; // TypeError: Identifier 'a' has already been declared
}
console.log(a); // 2
```

Another form of block-scoped declaration is the `const`, which creates constants. In ES6, a `const` represents a constant reference to a value. In other words, `Object`'s and `Array`'s contents may change, only the re-assignment of the variable is prevented. Here's a simple example:

```javascript
{
    const b = 5;
    b = 10; // TypeError: Assignment to constant variable

    const arr = [5, 6];
    arr.push(7);
    console.log(arr); // [5,6,7]
    arr = 10; // TypeError: Assignment to constant variable
    arr[0] = 3; // value is mutable
    console.log(arr); // [3,6,7]
}
```

A few things to keep in mind:

* Hoisting of `let` and `const` vary from the traditional hoisting of variables and functions. Both `let` and `const` are hoisted, but cannot be accessed before their declaration, because of [Temporal Dead Zone](http://jsrocks.org/2015/01/temporal-dead-zone-tdz-demystified)
* `let` and `const` are scoped to the nearest enclosing block.
* When using const with fixed strings or values, CAPITAL_CASING might be appropriate (ex: `const PI = 3.14`)
* `const` has to be defined with its declaration.
* Always use `const` over `let`, unless you plan on re-assigning the variable.

<br>

### 2. Arrow Functions

Arrow functions are a short-hand notation for writing functions in ES6. The arrow function definition consists of a parameter list `( ... )`, followed by the `=>` marker and a function body. For single-argument functions, the parentheses may be omitted.

```javascript
// Classical Function Expression
function addition(a, b) {
    return a + b;
};

// Implementation with arrow function
const addition = (a, b) => a + b;

// With single argument, no parentheses required
const add5 = a => 5 + a;
```

Note that in the above example, the `addition` arrow function is implemented with "concise body" which does not need an explicit return statement. Note the omitted `{ }` after the `=>`.

Here is an example with the usual "block body." Including the curly brace wrappers.

```javascript
const arr = ['apple', 'banana', 'orange'];

const breakfast = arr.map(fruit => {
    return fruit + 's';
});

console.log(breakfast); // ['apples', 'bananas', 'oranges']
```

**Behold! There is more...**

Arrow functions don't just make the code shorter. They are closely related to `this` binding behavior.

Arrow functions behavior with `this` keyword varies from that of normal functions. Each function in JavaScript defines its own `this` context but arrow functions capture the `this` value of the nearest enclosing context. Check out the following code:

```javascript
function Person() {
    // The Person() constructor defines `this` as an instance of itself.
    this.age = 0;

    setInterval(function growUp() {
        // In non-strict mode, the growUp() function defines `this`
        // as the global object, which is different from the `this`
        // defined by the Person() constructor.
        this.age++;
    }, 1000);
}
var p = new Person();
```

In ECMAScript 3/5, this issue was fixed by assigning the value in `this` to a variable that could be closed over.

```javascript
function Person() {
    const self = this;
    self.age = 0;

    setInterval(function growUp() {
        // The callback refers to the `self` variable of which
        // the value is the expected object.
        self.age++;
    }, 1000);
}
```

As mentioned above, arrow functions capture the this value of the nearest enclosing context, so the following code works as expected, even with nested arrow functions.

```javascript
function Person() {
    this.age = 0;

    setInterval(() => {
        setTimeout(() => {
            this.age++; // `this` properly refers to the person object
        }, 1000);
    }, 1000);
}

let p = new Person();
```

<br>

### 3. Default Function Parameters

ES6 allows you to set default parameters in function definitions. Here is a simple illustration.

```javascript
const getFinalPrice = (price, tax = 0.7) => price + price * tax;
getFinalPrice(500); // 850
```

<br>

### 4. Spread / Rest Operator

`...` operator is referred to as spread or rest operator, depending on how and where it is used.

When used with any iterable, it acts as to "spread" it into individual elements:

```javascript
const makeToast = (breadType, topping1, topping2) => {
  return `I had ${breadType} toast with ${topping1} and ${topping2}`;
};
```

```javascript
const ingredients = ['wheat', 'butter', 'jam'];
makeToast(...ingredients);
// "I had wheat toast with butter and jam"

makeToast(...['sourdough', 'avocado', 'kale']);
// "I had sourdough toast with avocado and kale"
```

Spread is also great for shaping a new object from other object(s):

```javascript
const defaults = {avatar: 'placeholder.jpg', active: false}
const userData = {username: 'foo', avatar: 'bar.jpg'}

console.log({created: '2017-12-31', ...defaults, ...userData})
// {created: "2017-12-31", avatar: "bar.jpg", active: false, username: "foo"}
```

New arrays can also be shaped expressively:

```javascript
const arr1 = [1, 2, 3];
const arr2 = [7, 8, 9];
console.log([...arr1, 4, 5, 6, ...arr2]) // [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

The other common usage of `...` is gathering all arguments together into an array. This is referred as "rest" operator.

```javascript
function foo(...args) {
    console.log(args);
}
foo(1, 2, 3, 4, 5); // [1, 2, 3, 4, 5]
```

<br>

### 5. Object Literal Extensions

ES6 allows declaring object literals by providing shorthand syntax for initializing properties from variables and defining function methods. It also enables the ability to have computed property keys in an object literal definition.

```javascript
function getCar(make, model, value) {
    return {
        // with property value shorthand
        // syntax, you can omit the property
        // value if key matches variable
        // name
        make,  // same as make: make
        model, // same as model: model
        value, // same as value: value

        // computed values now work with
        // object literals
        ['make' + make]: true,

        // Method definition shorthand syntax
        // omits `function` keyword & colon
        depreciate() {
            this.value -= 2500;
        }
    };
}

let car = getCar('Kia', 'Sorento', 40000);
console.log(car);
// {
//     make: 'Kia',
//     model:'Sorento',
//     value: 40000,
//     makeKia: true,
//     depreciate: function()
// }
```

<br>

### 6. Octal and Binary Literals

ES6 has new support for octal and binary literals.
Prependending a number with `0o` or `0O` would convert it into octal value. Have a look at the following code:

```javascript
let oValue = 0o10;
console.log(oValue); // 8

let bValue = 0b10; // 0b or 0B for binary
console.log(bValue); // 2
```

<br>

### 7. Array and Object Destructuring

Destructuring helps in avoiding the need for temp variables when dealing with object and arrays.

```javascript
function foo() {
    return [1, 2, 3];
}
let arr = foo(); // [1,2,3]

let [a, b, c] = foo();
console.log(a, b, c); // 1 2 3

```

```javascript

function getCar() {
  return {
    make: 'Tesla',
    model: 'g95',
    metadata: {
      vin: '123abc',
      miles: '12000'
    }
  };
}

const {make, model} = getCar();
console.log(make, model); // Tesla g95

const {make, metadata: {miles}} = getCar();
console.log(make, miles); // Tesla 12000


```

<br>

### 8. super in Objects

ES6 allows to use `super` method in (classless) objects with prototypes. Following is a simple example:

```javascript
const parent = {
    foo() {
        console.log("Hello from the Parent");
    }
}

const child = {
    foo() {
        super.foo();
        console.log("Hello from the Child");
    }
}

Object.setPrototypeOf(child, parent);
child.foo(); // Hello from the Parent
             // Hello from the Child
```

<br>

### 9. Template Literal and Delimiters

ES6 introduces an easier way to add interpolations which are evaluated automatically.

* <code>\`${ ... }\`</code> is used for rendering the variables.
* <code>\`</code> Backtick is used as delimiter.

```javascript
let user = 'Kevin';
console.log(`Hi ${user}!`); // Hi Kevin!
```

<br>

### 10. for...of vs for...in
* `for...of` iterates over iterable objects, such as array.

```javascript
const nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname of nicknames) {
    console.log(nickname);
}
// di
// boo
// punkeye
```

* `for...in` iterates over all enumerable properties of an object.

```javascript
const nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname in nicknames) {
    console.log(nickname);
}
// 0
// 1
// 2
// size
```

<br>

### 11. Map and WeakMap

ES6 introduces new set of data structures called `Map` and `WeakMap`. Now, we actually use maps in JavaScript all the time. In fact every object can be considered as a `Map`.

An object is made of keys (always strings) and values, whereas in `Map`, any value (both objects and primitive values) may be used as either a key or a value. Have a look at this piece of code:

```javascript
const myMap = new Map();

const keyString = "a string",
    keyObj = {},
    keyFunc = () => {};

// setting the values
myMap.set(keyString, "value associated with 'a string'");
myMap.set(keyObj, "value associated with keyObj");
myMap.set(keyFunc, "value associated with keyFunc");

myMap.size; // 3

// getting the values
myMap.get(keyString);    // "value associated with 'a string'"
myMap.get(keyObj);       // "value associated with keyObj"
myMap.get(keyFunc);      // "value associated with keyFunc"
```

**WeakMap**

A `WeakMap` is a Map in which the keys are weakly referenced, that doesn’t prevent its keys from being garbage-collected. That means you don't have to worry about memory leaks.

Another thing to note here- in `WeakMap` as opposed to `Map` *every key must be an object*.

A `WeakMap` only has four methods `delete(key)`, `has(key)`, `get(key)` and `set(key, value)`.

```javascript
const w = new WeakMap();
w.set('a', 'b');
// Uncaught TypeError: Invalid value used as weak map key

const o1 = {},
    o2 = () => {},
    o3 = window;

w.set(o1, 37);
w.set(o2, "azerty");
w.set(o3, undefined);

w.get(o3); // undefined, because that is the set value

w.has(o1); // true
w.delete(o1);
w.has(o1); // false
```

<br>

### 12. Set and WeakSet

*Set* objects are collections of unique values. Duplicate values are ignored, as the collection must have all unique values. The values can be primitive types or object references.

```javascript
const mySet = new Set([1, 1, 2, 2, 3, 3]);
mySet.size; // 3
mySet.has(1); // true
mySet.add('strings');
mySet.add({ a: 1, b:2 });
```

You can iterate over a set by insertion order using either the `forEach` method or the `for...of` loop.

```javascript
mySet.forEach((item) => {
    console.log(item);
    // 1
    // 2
    // 3
    // 'strings'
    // Object { a: 1, b: 2 }
});

for (let value of mySet) {
    console.log(value);
    // 1
    // 2
    // 3
    // 'strings'
    // Object { a: 1, b: 2 }
}
```
Sets also have the `delete()` and `clear()` methods.

**WeakSet**

Similar to `WeakMap`, the `WeakSet` object lets you store weakly held *objects* in a collection. An object in the `WeakSet` occurs only once; it is unique in the WeakSet's collection.

```javascript
const ws = new WeakSet();
const obj = {};
const foo = {};

ws.add(window);
ws.add(obj);

ws.has(window); // true
ws.has(foo);    // false, foo has not been added to the set

ws.delete(window); // removes window from the set
ws.has(window);    // false, window has been removed
```

<br>

### 13. Classes in ES6

ES6 introduces new class syntax. One thing to note here is that ES6 class is not a new object-oriented inheritance model. They just serve as a syntactical sugar over JavaScript's existing prototype-based inheritance.

One way to look at a class in ES6 is just a new syntax to work with prototypes and constructor functions that we'd use in ES5.

Functions defined using the `static` keyword implement static/class functions on the class.

```javascript
class Task {
    constructor() {
        console.log("task instantiated!");
    }

    showId() {
        console.log(23);
    }

    static loadAll() {
        console.log("Loading all tasks..");
    }
}

console.log(typeof Task); // function
const task = new Task(); // "task instantiated!"
task.showId(); // 23
Task.loadAll(); // "Loading all tasks.."
```

**extends and super in classes**

Consider the following code:

```javascript
class Car {
    constructor() {
        console.log("Creating a new car");
    }
}

class Porsche extends Car {
    constructor() {
        super();
        console.log("Creating Porsche");
    }
}

let c = new Porsche();
// Creating a new car
// Creating Porsche
```

`extends` allow child class to inherit from parent class in ES6. It is important to note that the derived constructor must call `super()`.

Also, you can call parent class's method in child class's methods using `super.parentMethodName()`

[Read more about classes here](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

A few things to keep in mind:

* Class declarations are not hoisted. You first need to declare your class and then access it, otherwise ReferenceError will be thrown.
* There is no need to use `function` keyword when defining functions inside a class definition.

<br>

### 14. Symbol

A `Symbol` is a unique and immutable data type introduced in ES6. The purpose of a symbol is to generate a unique identifier but you can never get any access to that identifier.

Here’s how you create a symbol:

```javascript
const sym = Symbol("some optional description");
console.log(typeof sym); // symbol
```

Note that you cannot use `new` with `Symbol(…)`.

If a symbol is used as a property/key of an object, it’s stored in a special way that the property will not show up in a normal enumeration of the object’s properties.

```javascript
const o = {
    val: 10,
    [Symbol("random")]: "I'm a symbol",
};

console.log(Object.getOwnPropertyNames(o)); // val
```

To retrieve an object’s symbol properties, use `Object.getOwnPropertySymbols(o)`


<br>

### 15. Iterators

An iterator accesses the items from a collection one at a time, while keeping track of its current position within that sequence. It provides a `next()` method which returns the next item in the sequence. This method returns an object with two properties: done and value.

ES6 has `Symbol.iterator` which specifies the default iterator for an object. Whenever an object needs to be iterated (such as at the beginning of a for..of loop), its *@@iterator* method is called with no arguments, and the returned iterator is used to obtain the values to be iterated.

Let’s look at an array, which is an iterable, and the iterator it can produce to consume its values:

```javascript
const arr = [11,12,13];
const itr = arr[Symbol.iterator]();

itr.next(); // { value: 11, done: false }
itr.next(); // { value: 12, done: false }
itr.next(); // { value: 13, done: false }

itr.next(); // { value: undefined, done: true }
```

Note that you can write custom iterators by defining `obj[Symbol.iterator]()` with the object definition.

<br>

### 16. Generators

Generator functions are a new feature in ES6 that allow a function to generate many values over time by returning an object which can be iterated over to pull values from the function one value at a time.

A generator function returns an **iterable object** when it's called.
It is written using the new `*` syntax as well as the new `yield` keyword introduced in ES6.

```javascript
function *infiniteNumbers() {
    let n = 1;
    while (true) {
        yield n++;
    }
}

const numbers = infiniteNumbers(); // returns an iterable object

numbers.next(); // { value: 1, done: false }
numbers.next(); // { value: 2, done: false }
numbers.next(); // { value: 3, done: false }
```

Each time *yield* is called, the yielded value becomes the next value in the sequence.

Also, note that generators compute their yielded values on demand, which allows them to efficiently represent sequences that are expensive to compute, or even infinite sequences.

<br>

### 17. Promises

ES6 has native support for promises. A *promise* is an object that is waiting for an asynchronous operation to complete, and when that operation completes, the promise is either fulfilled(resolved) or rejected.

The standard way to create a Promise is by using the `new Promise()` constructor which accepts a handler that is given two functions as parameters. The first handler (typically named `resolve`) is a function to call with the future value when it's ready; and the second handler (typically named `reject`) is a function to call to reject the Promise if it can't resolve the future value.

```javascript
const p = new Promise((resolve, reject) => {
    if (/* condition */) {
        resolve(/* value */);  // fulfilled successfully
    } else {
        reject(/* reason */);  // error, rejected
    }
});
```

Every Promise has a method named `then` which takes a pair of callbacks. The first callback is called if the promise is resolved, while the second is called if the promise is rejected.

```javascript
p.then((val) => console.log("Promise Resolved", val),
       (err) => console.log("Promise Rejected", err));
```

Returning a value from `then` callbacks will pass the value to the next `then` callback.

```javascript
const hello = new Promise((resolve, reject) => { resolve("Hello") });

hello.then((str) => `${str} World`)
     .then((str) => `${str}!`)
     .then((str) => console.log(str)) // Hello World!
```

When returning a promise, the resolved value of the promise will get passed to the next callback to effectively chain them together.
This is a simple technique to avoid "callback hell".

```javascript
const p = new Promise((resolve, reject) => { resolve(1) });

const eventuallyAdd1 = (val) => new Promise((resolve, reject) => { resolve(val + 1) });

p.then(eventuallyAdd1)
 .then(eventuallyAdd1)
 .then((val) => console.log(val)); // 3
```

# Future JavaScript

> ES6, ES7, JS2015 and beyond feature documentation and examples

## Features

<!-- toc -->

- [Arrow Functions](#arrow-functions)
  * [Single Expression](#single-expression)
  * [Single Argument](#single-argument)
  * [No Arguments](#no-arguments)
  * [Multiple Expressions](#multiple-expressions)
  * [Lexical this](#lexical-this)
  * [Concise Functional Iteration](#concise-functional-iteration)
- [let and const](#let-and-const)
  * [Block Scope](#block-scope)
  * [Hoisting](#hoisting)
  * [Loop Scope](#loop-scope)
  * [Implicit Scope Creation](#implicit-scope-creation)
  * [Read Only const](#read-only-const)
- [Modules](#modules)
  * [Named Exports](#named-exports)
  * [Default Exports](#default-exports)
  * [Re-Exporting](#re-exporting)
  * [System Module Loader](#system-module-loader)
- [Template Strings](#template-strings)
  * [Multiline](#multiline)
  * [Expression Substitution](#expression-substitution)
  * [Tagged Templates](#tagged-templates)
- [Class](#class)
- [Object Literal Enhancements](#object-literal-enhancements)
  * [Property Initialiser Shorthand](#property-initialiser-shorthand)
  * [Method Initialiser Shorthand](#method-initialiser-shorthand)
  * [Computed Property Names](#computed-property-names)
  * [Native object merging](#native-object-merging)
- [Destructuring](#destructuring)
  * [Object Destructuring](#object-destructuring)
  * [Array Destructuring](#array-destructuring)
  * [Iterator Destructuring](#iterator-destructuring)
  * [Loop Destructuring](#loop-destructuring)
  * [Parameter Destructuring](#parameter-destructuring)
- [Spread Operator](#spread-operator)
  * [Spread Destructuring](#spread-destructuring)
  * [Spread Parameters](#spread-parameters)
  * [Rest Spread Parameters](#rest-spread-parameters)
  * [Spread Property Initialisers](#spread-property-initialisers)
- [Default parameters](#default-parameters)
- [Symbols](#symbols)
  * [Generating Symbols](#generating-symbols)
  * [Pseudo Private Properties](#pseudo-private-properties)
  * [Symbol Constants](#symbol-constants)
- [Iterators](#iterators)
  * [Iterable Interface](#iterable-interface)
  * [for of loop](#for-of-loop)
- [Generators](#generators)
  * [Creating Iterators](#creating-iterators)
  * [Pausing and Resuming](#pausing-and-resuming)
  * [Error Handling](#error-handling)
  * [Generator I/O](#generator-i-o)
    + [Nested Iterators](#nested-iterators)
- [Map](#map)
  * [Map API](#map-api)
  * [Map Keys](#map-keys)
  * [Bulk Initialisation](#bulk-initialisation)
  * [Size](#size)
  * [Deleting Entries](#deleting-entries)
  * [Iteration](#iteration)
  * [Conversion to Array](#conversion-to-array)
  * [WeakMap](#weakmap)
- [Set](#set)
  * [Set API](#set-api)
  * [Adding Values](#adding-values)
  * [Bulk Initialisation](#bulk-initialisation)
  * [Iteration](#iteration)
  * [Conversion to Array](#conversion-to-array)
- [Promise](#promise)
  * [Consuming Promises](#consuming-promises)
  * [Producing promises](#producing-promises)
  * [Chaining](#chaining)
  * [Error Handling](#error-handling)
  * [Promise composition](#promise-composition)
  * [Promises and Generators](#promises-and-generators)
- [Proxy](#proxy)
  * [Simple Proxy](#simple-proxy)
  * [Logger](#logger)
  * [Web Service Resource](#web-service-resource)
  * [More traps](#more-traps)

<!-- tocstop -->

## Arrow Functions

Arrow functions are a concise anonymous function syntax, and their `this` value is lexically bound to their enclosing scope.

### Single Expression

When the arrow function body is a single expression it is implicitly returned.

```js
var add = (a,b) => a+b;
```

### Single Argument

When the arrow function has only one argument the parens around the parameters can be omitted.

```js
var odd = n => n % 2;
```

### No Arguments

When the arrow function has no arguments you need an empty parens `()`.

```js
var random = () => Math.random();
```

### Multiple Expressions

When the arrow function body has multiple expressions then they must be wrapped in `{}` and the `return` statement may not be omitted.

```js
var shout = s => {
  s = s.toUpperCase();
  s = s + '!';
  return s;
}
```

### Lexical this

The arrow function's `this` value is bound to the enclosing scope, so no more need for `var self = this`.

```js
function Counter () {
  this.count = 0;
  setInterval(() => this.count++, 1000);
}

var counter = new Counter();
```

### Concise Functional Iteration

Arrow functions can be used to write iteration, map, reduce and filter operations quite concisely.

```js
var data = ['one', 'two', 'three'];
var processed = data
  .map(s => s.length)
  .filter(length => length < 5);
```

## let and const

The `let` keyword declares a variable that is strictly scoped to the current block, statement or expression where it is defined. This is in contrast to `var` declarations which are scoped to the current function. An addition difference with `var` is that `let` variables are not hoisted to the top of the scope and they can only be used at a point in the code after they have been defined.

`const` variables share all these characteristics with the additional restriction that redeclaring them will generate an error, and changing their value once declared will fail silently.

As a rule of thumb `const` provides the strictest usage contract and clearly signals a narrow intent that the variable will not be redeclared or subsequently have its value reassigned, therefore `const` should be used in preference to `let` and `var` wherever appropriate.

Examples of "blocks" that `let` and `const` variables are scoped to include `if` statements, `for` loop headers and bodies and naked `{}` blocks.

### Block Scope

Attempting to access a `let` or `const` variable outside the block it's declared in will throw an error. Note the use of a naked `{}` block in this example to arbitrarily create a new scope.

```js
var a = 1;

{
  let b = 2;
}

console.log(a); // 1
console.log(b); // ReferenceError, b is undefined
```

### Hoisting

Unlike `var` declarations which are hoisted to the top of their enclosing scope `let` and `const` declarations may only be accessed after they've been declared. `let` and `const` variables are said to be in the scope's TDZ (temporal dead zone) before they've been declared, and any attempt to read or write them beforehand will generate an error.

:warning: Most transpilers currently don't handle this behaviour fully to-spec, so the above example will probably only error in a native ES6 environment.

```js
{
  console.log(foo); // 'foo'
  console.log(bar); // ReferenceError: bar is in the 'TDZ'
  var foo = 'foo';
  let bar = 'bar';
}
```

### Loop Scope

When `let` is used in a `for` loop header a new `i` is scoped for each iteration of the loop. This makes writing async code in loops more intuitive since the closure doesn't need to be created manually. This can also help with traditionally counter-intuituve tasks such as applying click event handlers in a loop.

```js
for (var i=1; i<=5; i++) {
    setTimeout(function(){
        console.log(i);
    }, i*100);
}
// 6,6,6,6,6

for (let i=1; i<=5; i++) {
    setTimeout(function(){
        console.log(i);
    }, i*100);
}
// 1,2,3,4,5
```

### Implicit Scope Creation

Using `let` within an `if` block implicitly creates a new scope. This is a hazard of using `let`. The new scope is easily spotted in the simple example above, but when code becomes more complicated hunting for new scopes created by `let` could become a cognitive burden. A rule of thumb is to place `let` declarations at the top of their enclosing block to clearly signpost their use and also avoid being bitten by the TDZ.

```js
if ( foo ) {
    // We're in the same scope as outside the 'if'
}

if ( foo ) {
    // We're in a new scope
    let a = 1;
}
```

### Read Only const

As mentioned, reassign a value to a constant will silently fail while redeclaring the constant will throw an error.

```js
const foo = 'foo';
foo = 'bar' // Silently fails, foo is still equal to 'foo'
const foo = 'bar' // TypeError, foo has already been defined
```

However constants are not immutable, therefore the properties of non-primitive values defined as a constant may be manipulated freely.

```js
const foo = {a: 1};
foo.a = 2;
foo.a; // 2
```

## Modules

ES6 modules will standardise module loading in JavaScript with a concise syntax that removes the need for the competing CommonJS, AMD and UMD module standards and libraries.

Since ES6 modules are part of the language their structure can be statically analysed (at compile time), which is in contrast to the current library-based module loaders which can only reveal their structure by running the code. This paves the way for better static analysis tools, static types and macros in future JavaScript.

ES6 modules use a declarative syntax for importing and exporting, support cyclic dependencies and support an async programmatic loading API for dynamically/conditionally loading modules at runtime.

Future browser APIs will be exposed as ES6 modules instead of global variables, properties of `navigator` or object namespaces such as `Math` and `JSON`.

### Named Exports

Named exports can be individually imported by name and will populate the scope of the importing module.

```js
// math.js
export const PI = Math.PI;
export function add(a,b) => a+b;
export function subtract(a,b) => a-b;
```

```js
// app.js
import {PI,add,subtract} from 'math';
PI; // 3.141592653589793
add(1,1); // 2
```

All named exports can be imported via the wildcard `*` character, in which case they will populate the supplied object namespace.

```js
// app.js
import * as math from 'math';
math.add(1,1); // 2
math.subtract(2,1); // 1
```

Named exports can alternatively be declared separately to the object they're exporting.

```js
// math.js
const PI = Math.PI;
function add(a,b) => a+b;
function subtract(a,b) => a-b;

export {PI,add,subtract};
```

Exported names can be different from the one used internally in the exporting module.

```js
// math.js
const PI = Math.PI;
function add(a,b) => a+b;
function subtract(a,b) => a-b;

export {subtract as minus};
```

And imported names can also be changed.

```js
// app.js
import {subtract as foo} from 'math';
foo(2,1); // 1
```

### Default Exports

Modules can indicate a default object to always be imported when no name is supplied. Default exports are to be favored over named exports since they simplify module APIs by making it unambiguous what to the primary export is. The default export/import syntax is effectively just sugar around a normal named export called `default`.

When importing a default export directly the `{}` named export syntax is not used. Additionally, unlike named exports the imported default export can be named anything you like in the importing module.

```js
// module.js
export default function () {}
```

```js
// app.js
import foo from './module';
foo();
```

Default and named imports can be mixed together on one line.

```js
// module.js
export default function () {}
export function bar () {}
```

```js
// app.js
import foo, {bar} from 'underscore';
```

### Re-Exporting

You can re-export from modules, which can be useful for aggregating many exports from sub modules in a single file. This is often used when exposing a group of exports in a folder's `index.js` file.

```js
// ./stuff/foo.js
export default function () {};
```

```js
// ./stuff/index.js
export {default as foo} from './foo';
export {default as bar} from './bar';
```

```js
// app.js
import {foo} from './stuff';
import * as stuff from 'stuff';
foo();
stuff.foo();
```

### System Module Loader

Modules can be loaded dynamically and conditionally via the loader API exposed by `System`. A common use of `System.import` could be to bootstrap a transpiled ES6 app in a `<script>` tag in a ES5-based browser environment.

```js
System
  .import('some-module')
  .then(someModule => {})
  .catch(error => {});
```

## Template Strings

ES6 template strings, denoted by backticks rather than single or double quotes, enable multiline strings, expression substitution and tagged templates.

### Multiline

All whitespace characters in backtick template strings are preserved, so no extra formatting is required for multiline strings.

```js
var address = `29 Acacia Road,
Nuttytown,
England`;
```

### Expression Substitution

Template strings are able to evaluate any expression against values in their current scope using `${}` syntax.

:warning: Be careful when building strings in this way from user input since you may introduce injection vulnerabilities, see tagged templates below for an alternative approach.

```js
var name = 'Billy';
var born = 1992;
var now = () => new Date().getFullYear();

var message = `${name} is ${now()-born} years old`;
// 'Billy is 23 years old'
```

### Tagged Templates

Tagged templates provide an abstracted and safer approach to string concatenation with dynamic values. They work by providing a layer of syntactic sugar over the process of implementing template generating functions. These functions are referred to as "template tags".

By using the ``func`some string`;`` syntax we invoke a template tag, that is a function to be run in order to process a template string. The template tag function receives the plain string parts and evaluated expressions in separate ordered arrays to be combined in a controlled manner, rather than blindly concatenated.

Template tags could be used by libraries to variously escape user input, strip tags, perform internationalisation or provide any other string-based functionality via an arbitrary DSL.

```js
function foo (literals,...values) {
    console.log(literals); // ['',' is ',' years old']
    console.log(values); // ['Billy',23]
    return 'foo';
}

var name = 'Billy';
var born = 1992;
var now = () => new Date().getFullYear();

var message = foo`${name} is ${now()-born} years old`;
// 'foo'
```

## Class

ES6 classes provide a standardised approach to prototypical inheritance supported by a layer of syntactic sugar built into the language. The syntax supports inheritance, a concise method syntax, constructor functions, getters and setters, super functions and static properties.

```js
class Shape {

  constructor (numSides) {
    this.numSides = numSides;
  }

  get sides () {
     return this.numSides;
  }

  describe () {
    return Shape.label() + ' with ' + this.sides + ' sides';
  }

  static label () {
    return 'A shape';
  }
}

class Triangle extends Shape {

  constructor () {
    super(3);
  }

  describe () {
    return super.describe() + ', A.K.A. a triangle';
  }
}

var triangle = new Triangle();
console.log(triangle.describe()); // 'A shape with 3 sides, A.K.A. a triangle'
```

An ES7 syntax supports static and instance property initialisers too. This potentially alleviates the need to initialise properties in the constructor.

```js
class Shape {
  foo = 'foo';
  static bar = 'bar';
}
```

## Object Literal Enhancements

Object literal syntax has received a number of enhancements.

### Property Initialiser Shorthand

When a new object literal is created and populated with properties of the same name that exist in the current scope then a shorthand syntax can be used.

```js
var foo = 'foo';
var bar = 'bar';

var o = {foo, bar};

console.log(o); // {foo: 'foo', bar: 'bar'}
```

### Method Initialiser Shorthand

Methods can now be declared in object literals in a similar way to the new `class` construct.

```js
var o = {
  foo () {
  }
};

console.log(o); // {foo: [Function]}
```

### Computed Property Names

Dynamic property names can be used while creating object literals.

```js
var foo = 'foo';

var o = {
  [foo]: 'bar',
  [new Date().getTime()]: 'baz'
};

console.log(o); // {foo: 'bar', 1428942731913: 'baz'};
```

### Native object merging

The new `Object.assign` function copies properties and methods from source objects into the leftmost target object and returns it.

```js
var o1 = {foo: 'foo'};
var o2 = {bar: 'bar'};
var o3 = {baz: 'baz', foo: 'qux'};

Object.assign(o1, o2, o3); // {foo: 'qux', bar: 'bar', baz: 'baz'}
console.log(o1); // {foo: 'qux', bar: 'bar', baz: 'baz'}
```

## Destructuring

Destructuring, or "destructuring assignment syntax" describes the syntactically efficient extraction and declaration of new variables in the local scope from arbitrarily complex data structures.

### Object Destructuring

When destructuring, object literal notation syntax is used on the left side of the assignment operation to both describe the target data structure and name the new local variables to be declared.

```js
var data = {
  foo: 'foo',
  bar: 'bar',
  baz: {
    qux: 'qux'
  }
};

var {foo:newFoo,bar:newBar,baz:{qux:newQux}} = data;

newFoo; // 'foo'
newBar; // 'bar'
newQux; // 'qux'
```
A shorthand syntax can be used when the desired local variable names are the same as the object keys in the data. This has the benefit of not having to name target keys twice as in `var foo = bar.foo` and so minimises typing and errors.

```js
var data = {
  foo: 'foo',
  bar: 'bar'
};

var {foo,bar} = data;

foo; // 'foo'
bar; // 'bar'
```

### Array Destructuring

Array literal notation syntax can be freely mixed with object literal notation syntax while destructuring.

```js
var data = ['foo','bar',['baz']];

var [foo,bar,[baz]] = data;

foo; // 'foo'
bar; // 'bar'
baz; // 'baz'
```

### Iterator Destructuring

Array literals can be used to destructure any iterable object.

```js
var set = new Set().add('a').add('b').add('c');

var [x,y,z] = set;
console.log(x,y,z); // a b c
```

### Loop Destructuring

We can destructure in a loop header to efficiently declare loop variables.

```js
let map = new Map();
map.set('a',1);
map.set('b',2);

for (let [k,v] of map) {
  console.log(`key = ${k}, value = ${v}`);
}
```

### Parameter Destructuring

Destructuring can also be used inline in function parameters.

```js
function f ({foo,bar}) {
  console.log(foo);
  console.log(bar);
}

f({foo:'foo',bar:'bar'}); // 'foo' 'bar'
```

## Spread Operator

### Spread Destructuring

The spread operator `...` can be used to destructure arrays, other iterables and objects. It is used to catch remaining properties not already picked off by the destructuring pattern.

```js
const array = [1, 2, 3, 4];
const [first, ...theRest] = array;
console.log(first, theRest); // 1 [2,3,4]
```

```js
const object = {a: 1, b: 2, c:3};
const {a, ...theRest} = object;
console.log(a, theRest); // 1 {b: 2, c: 3}
```

### Spread Parameters

In addition to spread destructuring, the spread operator `...` can be used to unpack an array into its elements in order to be passed into a function as individual arguments.

```js
function f (a,b,c) {
  console.log(a,b,c);
};

var values = [1,2,3];
f(...values); // 1 2 3
```

Spread parameters can also be mixed and matched with normal function parameters.

```js
function f (a,b,c,d) {
  console.log(a,b,c,d);
};

var values = [2,3];
f(1,...values,4); // 1 2 3 4
```

### Rest Spread Parameters

Rest spread parameters are used to capture all remaining parameters passed to a function and expose them as an array. No other named arguments may follow the rest parameters in the function signature.

```js
function f (first,second,...numbers) {
  console.log(first,second,numbers);
}

f(1,2,3,4,5); // 1 2 [3,4,5]
```

### Spread Property Initialisers

Spread properties in object initializers copies the enumerable properties from a provided object onto the newly created object.

```js
const foo = {a: 2, b: 3};
const bar = {a: 1, ...foo};
bar; // {a: 2, b: 3}
```

## Default parameters

Default function parameters provide default values for parameters which are not passed in. In this way function parameters with defined defaults can be thought of as optional parameters.

```js
function f (a,b=2) {
  console.log(a,b);
}

f(1); // 1 2
```

Passing in `undefined` will cause the default value to be used. Passing in `null` however will not cause the default value to be used.

```js
function f (a=1,b=2) {
  console.log(a,b);
}

f(undefined,null); // 1 null
```

The value of the default parameter need not be a primitive, it can for example be a function.

```js
function f (callback=() => {}) {
  console.log(callback);
}
```

The default value may even be the result of calling a function.

```js
function getCallback () {
  return () => {};
}

function f (callback=getCallback()) {
  console.log(callback);
}
```

## Symbols

Symbols are a new primitive type in ES6. Originally drafted as a mechanism to create strictly private object members, they have since lost that trait and now simply define non-string computed property identifiers that are non-enumerable but discoverable.

When referring to specific symbols outside of code the "@@" notation is used. For example, we might refer to the symbol in the first example below as the @@firstName symbol.

### Generating Symbols

Symbols are created with the `Symbol` function. Each newly created symbol value is guaranteed unique. The argument passed to `Symbol()` is the symbol's description. It is good practice to always give a symbol a description to aid with debugging.

```js
var firstName = Symbol('firstName');
console.log(firstName); // 'Symbol(firstName)'
```

### Pseudo Private Properties

Symbols can be used as computed property identifiers in objects and classes. The related value is therefore somewhat private to code that does not have a reference to the symbol itself, for example code in other modules. The value is not strictly private however since the symbol and its value are still enumerable via reflection APIs.

```js
const PRIVATE_VALUE = Symbol('privateValue');
const PRIVATE_METHOD = Symbol('privateMethod');

class Foo {

  constructor () {
    this.publicValue = 'bar';
    this[PRIVATE_VALUE] = 'baz';
  }

  [PRIVATE_METHOD] () {
    // Can't see or call me without jumping through hoops
  }
}
```

### Symbol Constants

Symbols can be a better choice than strings for the values of constants, since they are guaranteed unique.

```js
const COLOR_RED = Symbol('colorRed');
const COLOR_GREEN = Symbol('colorGreen');
const COLOR_BLUE = Symbol('colorBlue');
```

## Iterators

In ES6 objects are said to be iterable when they implement the *iterable* interface. Many built-in objects such as arrays, sets and maps implement this interface. User defined objects and classes can also implement the interface.

Iterable objects can also be iterated using the new `for of` loop, and used with the `...` spread operator.

### Iterable Interface

An object is said to conform to the iterable interface when the value of its property identified with the shared @@iterator Symbol is a function which returns an iterator.

```js
var iterator = [1,2,3][Symbol.iterator]();
```

An iterator is any object that implements a `next` function.

```js
var iterator = [1,2,3][Symbol.iterator]();

console.log(iterator.next); // '[Function]'
```

The `next` function can be called repeatedly to step through the iteration. Each time it returns an object that contains two keys, `value` and `done`. The values of these keys indicate the current value of the iteration and its completion status respectively.

```js
var iterator = [1,2,3][Symbol.iterator]();

console.log(iterator.next()); // {value:1, done:false}
console.log(iterator.next()); // {value:2, done:false}
console.log(iterator.next()); // {value:3, done:false}
console.log(iterator.next()); // {value:undefined, done:true}
```

Custom objects and classes can be made iterable by implementing the iterator interface manually.

```js
var iterable = {
  [Symbol.iterator] () {
    return {
      next () {
        // Implement iterator
      }
    }
  }
}
```

### for of loop

The new `for of` loop is designed to work exclusively with iterable objects. The loop calls the `next` function behind the scenes and exits when the `done` property is `true`.

```js
for ( let n of [1,2,3] ) {
  console.log(n);
}
// 1
// 2
// 3
```

As long as the object implements the iterable interface it can be looped with the `for of` loop. This includes arrays, maps and sets.

```js
var map = new Map();
map.set('a', 1);
map.set('b', 2);

for (let pair of map) {
  console.log(pair);
}
// [a,1]
// [b,2]
```

Arrays, sets and maps also expose the `entries`, `keys` and `values` functions for returning specialised iterators. The `keys` iterator loops over only the keys, the `values` iterator only the values, and the `entries` iterator the key/value pairs.

```js
var map = new Map();
map.set('a', 1);
map.set('b', 2);

for (let key of map.keys()) {
  console.log(key);
}
// a
// b
```

In ES6 strings also implement the iterable interface.

```js
for (let char of 'foo') {
  console.log(char);
}
// 'f'
// 'o'
// 'o'
```

## Generators

Generators are a type of function that can be paused and resumed, and are primarily used for lazy evaluation of sequences. They can also be used to clean up the syntax of sequences of async tasks.

When a generator is created using the `function *` syntax a specialised iterator instance is returned that can be used to control the generator, consume its output or feed it new input.

Each `yield` expression encountered inside the generator function body passes a `value` out into the iteration. Generators are initially paused at the very beginning of their function body, and can only be advanced through each successive internal `yield` expression by calling `next` on the generator's iterator each time.

The iterator's `next()` method can also be used to pass values into the generator which manifest inside the function body as the value of the `yield` expression at which the runtime is currently paused.

### Creating Iterators

At their simplest, generators provide a shorthand for easily creating custom iterable objects. Generator derived iterables can be used in all the same ways as standard iterables, i.e. with the `for of` loop and the `...` spread operator.

```js
function * makeGenerator () {
  yield 1;
  yield 2;
  yield 3;
}

var it = makeGenerator();

for (let i of it) {
    console.log(i);
}
// 1, 2, 3

var array = [...makeGenerator()];
console.log(array); // [1,2,3]
```

`yield` doesn't need to be called explicitly line by line, it can be called repeatedly in a loop to generate a dynamic sequence of values. The generator pauses execution at each `yield` while waiting for `next` to be called, so its in this way that the sequence is lazily generated.

```js
function * makeGenerator (items) {
  for (let i=0; i < items.length; i++) {
    yield items[i];
  }
}

var it = makeGenerator([1,2,3]);

for (let i of it) {
    console.log(i);
}
// 1
// 2
// 3
```

A `return` statement can be used in a generator function body to terminate the iteration and indicate a final `value` emitted together with `done:true`.

:warning: Be careful when using `return` in a generator though; a `for in` loop will discard the final returned value and not loop over it since at that point `done:true`.

```js
function * makeGenerator () {
  yield 1;
  return 2;
  yield 3; // Unreachable
}

var it = makeGenerator();

console.log(it.next()); // {value:1, done:false}
console.log(it.next()); // {value:2, done:true}
console.log(it.next()); // {value:undefined, done:true}

```

Generators can be defined in objects and classes using the shorthand method syntax.

```js
var o = {
  * makeGenerator () {}
}

var it = o.makeGenerator();

console.log(it.next); // 'Function'
```

A `yield` expression can only be used directly inside the body of a generator function, if you try to use it within a non-generator function body the code will explode.

```js
function * makeGenerator () {
  [1,2].forEach(x => yield x); // Explodes!
}
```

### Pausing and Resuming

An key feature of generator derived iterables is that they are initially paused at the very start of their function body. Calling `next` will cause the runtime to advance to the next `yield` expression and pause again.

```js
function * makeGenerator () {
  console.log('Hello');
  yield 1;
  console.log('World');
}

var it = makeGenerator();
var result = it.next(); // 'Hello'
console.log(result); // {value:1, done:false}
result = it.next(); // 'World'
console.log(result); // {value:undefined, done:true}
```

### Error Handling

Error handling in generators is simple since it can be handled normally via `throw` and `try ... catch`.

Generator iterators expose a method `throw` which can be used to raise an error inside the generator at the `yield` expression at which it is currently paused.

The string "Hi mum!" in the example below will never be logged, since we manually throw an error into the iteration.

```js
function * makeGenerator () {
  yield 1;
  console.log('Hi mum!');
}

var it = makeGenerator();

it.next(); // {value:1, done:false}
it.throw('Poop!') // Uncaught Error
```

### Generator I/O

`yield` passes data out of the generator into the iterator, but the iterator's `next` method can be used to pass data in the other direction, that is, into the generator. The value passed in via `next` becomes the value of the `yield` expression at which the generator is currently paused. Note that no value is passed in with the very first call to `next`, this is because although the generator is paused, it is not paused at a `yield` so therefore any value passed via `next` here is discarded.

Try to follow the arithmetic has values pass in and out of the generator below. It's quite mind bending to understand at first, and may take some effort!

```js
function * makeGenerator (x) {
    var y = 2 * (yield (x + 3));
    var z = yield (y / 2);
    return (x + y + z);
}

var it = makeGenerator(2);

console.log(it.next()); // {value:5, done:false}
console.log(it.next(4)); // {value:4, done:false}
console.log(it.next(3)); // {value:13, done:true}
```

#### Nested Iterators

The `yield *` syntax can be used to nest iterables arbitrarily deeply. By using `yield *` we are delegating the iteration to a child iterable which will output all its values into the main loop until they are exhausted at which point control is handed back to the parent generator. In this way iterators can be composed together and reused.

```js
function * makeGenerator () {
  yield 1;
  yield * [2,3];
  yield 4;
}

var array = [...makeGenerator()];
console.log(array); // [1,2,3,4]
```

Here we're giving the `Foo` class a default iterator which loops over its items, and implementing it using a generator function which delegates to a nested iterator, the built-in `values` iterator of `Array`.

```js
class Foo {

  items: [],

  * [Symbol.iterator] () {
    yield * this.items.values();
  }
}
```

`yeild *` can be used to nest any sort of iterable, including those derived from generators.

```js
function * generatorB(i) {
  yield i + 1;
  yield i + 2;
  yield i + 3;
}

function * generatorA(i) {
  yield i;
  yield * generatorB(i);
  yield i + 10;
}

var it = generatorA(10);

console.log(gen.next().value); // 10
console.log(gen.next().value); // 11
console.log(gen.next().value); // 12
console.log(gen.next().value); // 13
console.log(gen.next().value); // 20
```

## Map

Map is a new iterable data structure in ES6. It can be used to map values to values. Previously in ES5 this was usually achieved by (ab)using native objects and mapping strings to values.

### Map API

| Method           | Description                                                |
| ---------------- | ---------------------------------------------------------- |
| `get(key)`       | Returns the value for the specified `key`                  |
| `set(key,value)` | Set or update the `value` at `key`                         |
| `has(key)`       | Whether `key` exists in the map                            |
| `delete(key)`    | Remove entry for `key`                                     |
| `clear()`        | Remove all entries                                         |
| `entries()`      | Returns an iterable which iterates `[key,value]` (default) |
| `keys()`         | Returns an iterable which iterates `key`                   |
| `values()`       | Returns an iterable which iterates `value`                 |
| `forEach(f)`     | Functional iteration                                       |

### Map Keys

Any value can be a Map key, even an object or a function.

```js
var key1 = {};
var key2 = 'a';
var key3 = true;
var key4 = () => {};

var map = new Map();
map.set(key1, 'foo');
map.set(key2, 'bar');
map.set(key3, 'baz');
map.set(key4, 'qux');

map.get(key1); // 'foo'
map.get(key2); // 'bar'
map.get(key3); // 'baz'
map.get(key4); // 'qux'
```

>

### Bulk Initialisation

Map instances can be initialised with an array of key/value pairs.

```js
var map = new Map([
  [1,'foo'],
  [2,'bar'],
  [3,'baz']
]);
```
### Size

The `size` getter returns the number of entries in the map.

```js
var map = new Map([
  [1,'foo'],
  [2,'bar'],
  [3,'baz']
]);

console.log(map.size); // 3
```

### Deleting Entries

Entries can be removed from the map by calling the `delete()` method. The return value indicates whether the specified key was present in the map to begin with.

```js
var map = new Map([
  [1,'foo'],
  [2,'bar'],
  [3,'baz']
]);

console.log(map.delete(1)); // true
console.log(map.delete(4)); // false
```

### Iteration

Map can be iterated using the new `for of` loop. Its default iterator is same as that returned by `entries()`.

```js
for ( let entry of map ) {
  console.log(entry); // [key,value]
}

for ( let key of map.entries() ) {
  console.log(entry); // [key,value]
}

for ( let key of map.keys() ) {
  console.log(key); // key
}
```

You can destructure during the iteration to gain concise access to the `key` and `value` variables.

```js
for ( let [key,value] of map) {
  console.log(key,value);
}
```

### Conversion to Array

The iterators of Map can be used to convert the data structure into an array using the `...` spread operator.

```js
var map = new Map([
  [1,'foo'],
  [2,'bar'],
  [3,'baz']
]);

var values = [...map.values()];
console.log(values); // 'foo', 'bar', 'baz'
```

The concise conversion between Map and `Array` can be used to leverage array's `filter()` and `map()` functions to process the underlying data structure.

```js
var map1 = new Map([
  [1,'a'],
  [2,'b'],
  [3,'c'],
  [4,'d']
]);

var map2 = new Map(
  [...map1]
  .filter(([key,value]) => key > 2)
  .map(([key,value]) => [key * 2, value])
);

console.log(map2); // {6:'c', 8:'d'}
```

### WeakMap

WeakMap does not prevent its keys from being garbage collected. This makes WeakMap suitable for storing key/value pairs where the key may disappear sometime in the future. In the above example, once the `.Foo` element has been removed from the DOM, as long as there are no other references anywhere, it is free to be garbage collected and at which point it was also disappear from the WeakMap. Because of the way the weak references are stored in WeakMap it is not iterable, does not have a `clear()` method and cannot contain primitive values only object references, but aside from these differences its API is the same as Map.

```js
var map = new WeakMap();

map.set(document.querySelector('.Foo'), 'foo');
```

## Set

Set is a new iterable data structure in ES6 designed to store unique values. Unlike arrays where elements are stored at specific indices one typically simply tests values for membership in a set.

### Set API

| Method          | Description                                          |
| --------------- | ---------------------------------------------------- |
| `add(value)`    | Adds a value to the set                              |
| `has(value)`    | Whether a value exists in the set                    |
| `delete(value)` | Remove value from the set                            |
| `clear()`       | Remove all values                                    |
| `values()`      | Returns an iterable which iterates `value` (default) |
| `forEach(f)`    | Functional iteration                                 |

### Adding Values

A Set can contain primitive values and object references. Each new, unique value or reference counts towards the size of the set.

```js
var set = new Set();

set.add({});
set.add({});
set.add(1);
set.add('foo');

console.log(set.size); // 4
```

Adding duplicate values has no effect.

```js
var set = new Set();

set.add(1);
set.add(1);

console.log(set.size); // 1
```

### Bulk Initialisation

Set can be initialised using an array as a data source, which also has the effect of removing duplicate values.

```js
var set = new Set(['red','green','blue','blue']);

console.log(set.size); // 3
```

### Iteration

Set is iterable so can therefore be iterated using the new `for of` loop. The order of iteration is the same as the order of insertion into the set.

```js
var set = new Set(['red','green','blue']);

for (let value of set) {
  console.log(value);
}
```

### Conversion to Array

Since Set is both iterable and can be initialised by an array, conversion to and from an array is straightforward using the `...` spread operator. Duplicate values will be pruned from the array during its conversion to a set.

```js
var set = new Set([1,2,3]);
var array = [...set];

array.push(3);

console.log(array); // 1,2,3,3
console.log([...new Set(array)]); // 1,2,3
```

Array conversion and initialisation can be used to access array's `filter()` and `map()` methods.

```js
var set1 = new Set([1,2,3,4,5]);
var set2 = new Set([...set].filter(x => x > 2));

console.log(set2); // {3,4,5}
```

## Promise

A promise is an object that represents the result of an async operation. ES6 has a built-in promise implementation that conforms to the Promise/A+ spec.

A promise may be in one of the following three mutually exclusive states, `pending|fulfilled|rejected`.

Resolving a promise means a state change from `pending -> fulfilled`. The promise may resolve with a value, that is, the result of the async operation.

Rejecting a promise means a state change from `pending -> rejected`. The promise may reject with an error, which is often an instance of `Error`.

When a promise is either `fulfilled` or `rejected` it is said to be "settled".

Code can react to a promise's state change by registering callbacks via its `then()` and `catch()` methods.

> The ES6 promise implementation lacks some of the API provided by other promise libraries, such as `done()` and `finally()` methods, amoungst others. Consider using a mature promise library such as [Bluebird](https://github.com/petkaantonov/bluebird) if these additional features are important, although the benefits of using the standardised ES6 implementation shouldn't be overlooked.

### Consuming Promises

A promise's `then()` method can register two callbacks to handle fulfillment or rejection.

```js
promise.then(
  value => {},
  error => {}
);
```

Alternatively use the `then()` and `catch()` methods to handle each callback explicitly. This is the preferred form since it signals intent more clearly.

```js
promise
  .then(value => {})
  .catch(error => {});
```

### Producing promises

A promise can be instantiated and then resolved or rejected within a callback passed to its constructor.

```js
var promise = new Promise((resolve,reject) => {
  resolve('yay!');
  // Or,
  // reject(new Error('Poop!'));
});

promise
  .then(value => console.log('success',value))
  .catch(error => console.log('fail',error));

// 'success yay!'
```

A more concise and efficient approach is to leverage the static methods `Promise.resolve()` and `Promise.reject()` which return an immediately resolved or rejected promise directly.

```js
var promise = Promise.resolve().then(function () {
  // return Promise.resolve('yay!');
  // Or,
  return Promise.reject(new Error('Poop!'));
});

promise
  .then(value => console.log('success',value))
  .catch(error => console.log('fail',error));

// 'fail [Error: Poop!]'
```

### Chaining

Any value returned in a fulfillment callback is appended onto the promise chain. When returning a primitive value it is wrapped in a promise and immediately resolved.

```js
Promise.resolve()
  .then(() => 'foo')
  .then(value => console.log(value));

// 'foo'
```

So-called *thenable* objects, i.e. other promises, can also be returned from a fulfillment callback. In which case the chain will wait for the promise to settle and then propagate its fulfilled or rejected state into the chain.

```js
Promise.resolve()
  .then(() => Promise.resolve('foo'))
  .then(value => console.log(value));

// 'foo'
```

When chaining async methods that take a single argument and return a promise they can be directly and concisely inserted into the `then` chain by reference without the need to wrap them in an anonymous callback function.

```js
function asyncThing (value) {
  return Promise.resolve(value);
}

Promise.resolve('foo')
  .then(asyncThing)
  .then(value => console.log(value));

// 'foo'
```

### Error Handling

Any errors thrown by code in the promise setup function passed to its constructor will propagate into the promise chain and surface in the first rejection callback.

```js
var promise = new Promise((resolve,reject) => {
  throw(new Error('Poop!'));
});

promise
  .then(value => console.log(value))
  .catch(error => console.log(error));

// '[Error: Poop!]'
```

Any errors thrown in a fulfillment callback will also raise an error in the chain. Errors drop through additional chained fulfillment callbacks until they reach a rejection callback. Be careful since if there are no rejection callbacks registered on the promise the error will be silently swallowed. Exceptions will not bubble out of the promise chain.

```js
var promise = lib.getPromise();

promise
  .then(() => {
    throw(new Error('Poop!'));
  })
  .then(lib.getAnotherPromise)
  .then(lib.getYetAnotherPromise)
  .catch(error => console.log(error));
```

If an error is caught in a rejection callback but that callback doesn't raise a new error then that error is deemed recoverable and the chain continues in a non-error state.

```js
var promise = lib.getPromise();

promise
  .then(lib.getAnotherPromise)
  .then(lib.getYetAnotherPromise)
  .catch(error => error)
  .then(value => console.log('Everything is OK, but there was an error ',value));
```

### Promise composition

The `Promise.all` method takes an array of promises and returns a single promise that fulfills with an array containing the values of each the supplied promises.

```js
var promises = ['foo','bar','baz'].map(value => {
  return Promise.resolve(value);
});

Promise.all(promises)
  .then(values => console.log(values))
  .catch(error => console.log(error));

// ['foo','bar','baz']
```

The `Promise.race` method takes an array of promises and returns a single promise that fulfills with the value of the first promise of the supplied promises to fulfill.

```js
function delay (delay) {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      resolve(delay);
    },delay)
  });
}

Promise
  .race([delay(100),delay(200)])
  .then(value => console.log(value));

// 100
```

### Promises and Generators

Flow control libraries like [tj/co](https://github.com/tj/co) combine the power of promises with generators to create easily comprehensible async code. Error handling also becomes simpler than the callback-based equivalent code since `try catch` and `throw` can be used.

These libraries anticipate the ES7 `async function` syntax which will essentially allow the above style code natively.

```js
function asyncThing (value) {
  return Promise.resolve(value);
}

var promise = co(function * () {
  var a = yield asyncThing('foo');
  var b = yield asyncThing('bar');
  return a + b;
});

promise
  .then(value => console.log(value))
  .catch(error => console.log(error));

// 'foobar'
```

## Proxy

Proxies are a meta-programming feature in ES6. They allow for the creation of handler objects that transparently intercept and customise operations performed on target objects.

For each type of operation that can be performed on a target object (get, set etc.) there is a corresponding method that can be defined on the handler to intercept the operation and customise it. These operation interceptors are termed "traps".

If the handler does not have a trap defined for a particular operation it transparently forwards the operation back to the target object. In this way a proxy can stand in for a target object while augmenting it with an outer layer of additional functionality. There is no way to tell if any particular object is being proxied or not.

> :warning: Most transpilers can't create a true Proxy implementation, so a real ES6 environment may be needed to work with these objects.

### Simple Proxy

The 'get' trap intercepts every attempt to access any property on the target object. It works for property access and method invocation.

```js
var target = {
  foo: 'foo'
};

var handler = {
  get (target, key) {
    return 'bar';
  }
};

var proxy = new Proxy(target, handler);

console.log(proxy.foo); // 'bar'
console.log(proxy.baz()); // Error: String 'bar' is not a function
```

### Logger

A simple logger could be created using a proxy. Proxies could also be a good choice for implementing profilers, mocks and spies since they can dynamically respond to arbitrary property access and method invocation.

```js
var target = {
  foo: 'foo'
};

var handler = {
  get (target, key) {
    console.log('GET', key);
    return target[key];
  }
};

var proxy = new Proxy(target, handler);

var foo = proxy.foo; // "GET foo"
```

### Web Service Resource

Another example use for a proxy could be a REST web service utility. REST resource names could be intercepted from arbitrary method calls and used to build the request url.

```js
function makeResouce (baseUrl) {
  return new Proxy({}, {
    get (target,key) {
      return () => http.get(baseUrl + key);
    }
  });
}

var api = makeResource('http://api.domain.com/');

api.users().then(value => {
  console.log(value);// api.domain.com/users JSON
});
```

>

### More traps

An incomplete list of additional traps that may be defined on the proxy handler.

```js
var handler = {
  get (target,key) {
    // proxy.foo
    // proxy.foo()
  },
  set (target,key,value) {
    // proxy.foo = 'bar';
  },
  ownKeys (target) {
    // Object.keys(proxy)
  },
  has (target,key) {
    // key in proxy
  },
  enumerate (target) {
    // for ( x in proxy ) { }
  },
  deleteProperty (target,key) {
    // delete foo
  },
  construct (target,args) {
    // new proxy(..args)
  }
};
```