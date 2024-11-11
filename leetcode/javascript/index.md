---
title: Javascript
parent: Leetcode
nav_order: 3
---

Notes from [LeetCode's 30 Days of JavaScript Challenge](https://leetcode.com/studyplan/30-days-of-javascript/).

# Functions

**Basic Syntax**

```js
function f(a, b) {
    const sum = a + b;
    return sum;
}
console.log(f(3, 4)); // 7
```

**Anonymous Function**

You can optionally exclude the name of the function after the function keyword.

```js
var f = function(a, b) {
    const sum = a + b;
    return sum;
}
console.log(f(3, 4)); // 7
```
**Function Hoisting**

JavaScript has a feature called hoisting where a function can sometimes be used before it is initialized. You can only do this if you declare functions with the function syntax.

```js
function createFunction() {
    return f;
    function f(a, b) {
        const sum = a + b;
        return sum;
    }
}
const f = createFunction();
console.log(f(3, 4)); // 7
```
**Arrow Syntax**

```js
const f = (a, b) => {
    const sum = a + b;
    return sum;
};
console.log(f(3, 4)); // 7
```

If you can write the code in a single line, you can omit the return keyword. This can result in very short code.

```js
const f = (a, b) => a + b;
console.log(f(3, 4)); // 7
```

**Rest Arguments**

(spread operator)

```js
function f(...args) {
    const sum = args[0] + args[1];
    return sum;
}
console.log(f(3, 4)); // 7
```

It may not be immediately obvious why you would use this syntax because you can always just pass an array and get the same result.

The primary use-case is for creating generic factory functions that accept any function as input and return a new version of the function with some specific modification.

By the way, a function that accepts a function and/or returns a function is called a higher-order function, and they are very common in JavaScript.

```js
function log(inputFunction) {
    return function(...args) {
        console.log("Input", args);
        const result = inputFunction(...args);
        console.log("Output", result);
        return result;
    }
}
const f = log((a, b) => a + b);
f(1, 2); // Logs: Input [1, 2] Output 3
```

> When a function returns an object or another function, it's making use of higher-order functions and factory functions. Higher-order functions are functions that operate on other functions, either by taking them as arguments or by returning them. Factory functions, on the other hand, are functions that return object instances. This concept is crucial in functional programming and provides a way to encapsulate and reuse code.

# Closures

An important topic in JavaScript is the concept of closures. When a function is created, it has access to a reference to all the variables declared around it, also known as it's lexical environment. The combination of the function and its enviroment is called a closure. This is a powerful and often used feature of the language.

Closures are useful for creating private variables and functions, implementing partial function application, and preserving state in asynchronous code.
https://leetcode.com/problems/counter/solutions/3491300/day2-o-1-understanding-closure-in-easy-way-and-its-practical-uses


```js
function createAdder(a) {
    function f(b) {
        const sum = a + b;
        return sum;
    }
    return f;
}
const f = createAdder(3);
console.log(f(4)); // 7
```
In this example, `createAdder` passes the first parameter `a` and the inner function has access to it. This way, `createAdder` serves as a factory of new functions, with each returned function having different behavior.

**Closures vs Class**

```js
class Adder {
  constructor(a) {
     this.a = a;
  }

  add(b) {
    const sum = this.a + b;
    return sum;
  }
}
const addTo2 = new Adder(2);
addTo2.add(5); // 7
```
- One key difference is that closures allow for true encapsulation. In the class example, there is nothing stopping you from writing addTo2.a = 3; and breaking it's expected behavior.
- However, in the closure example, it is theoretically impossible to access a. Note that as of 2022, true encapsulation is achievable in classes with # prefix syntax.
- Another difference is how the functions are stored in memory. If you create many instances of a class, each instance stores a single reference to the prototype object where all the methods are stored. Whereas for closures, all the "methods" are generated and a "copy" of each is stored in memory each time the outer function is called. For this reason, classes can be more efficient, particularly in the case where there are many methods.

# Method Chaining

Full method chaining is a common pattern in JavaScript that permits multiple methods to be invoked in a single statement. This pattern is implemented when each method returns an object, which could be the original object (for mutable objects) or a new object (for immutable objects). Full method chaining enhances readability and conciseness of the code and is a preferred pattern in many JavaScript libraries.

```js
let arr = [5, 2, 8, 1];

let result = arr.sort().reverse().join("-");

console.log(result); // "8-5-2-1"
```

In this example, the sort() method sorts the array, the reverse() method subsequently reverses the sorted array, and finally, the join("-") method joins the elements into a string with "-" as a separator. Each of these methods returns an array, allowing the next method to be directly invoked on the result.

**Error Handling**

**Throwing an Error instance**

Errors in JavaScript functions can be handled using `try...catch...finally blocks.

```js
function divide(numerator, denominator) {
  if (denominator === 0) {
    throw new Error("Cannot divide by zero!");
  }
  return numerator / denominator;
}

try {
  console.log(divide(5, 0));
} catch (error) {
  console.error(error.message); // "Cannot divide by zero!"
}
```

**Throwing an Aggregated Error**

There are times when you might want to throw multiple errors at once. This is particularly useful when dealing with Promises. JavaScript has an in-built AggregateError object that can be used in these scenarios. The AggregateError object takes an iterable of error objects and an optional message as parameters.

```js
let error1 = new Error("First Error");
let error2 = new Error("Second Error");

try {
  throw new AggregateError([error1, error2], "Two errors occurred.");
} catch (error) {
  if (error instanceof AggregateError) {
    console.error(error.message); // "Two errors occurred."
    for (let e of error.errors) {
      console.error(e.message); // logs "First Error" then "Second Error"
    }
  }
}
```

**Objects**

At their core, objects are just mappings from strings to other values. The values can be anything: strings, functions, other objects, etc. The string that maps to the value is called the key.

There are three ways to access values on an object:

```js
const val = object.obj.x;

const val = object["obj"]["x"];

const { num, str} = object;
```

**Classes**

You can also define classes in JavaScript. The classes's constructor returns an object which is an instance of that class.

JavaScript implements classes with special objects call `prototypes`. All the methods (in this case greet) are functions stored on the object's prototype.

The reason is that accessing keys on an object is actually slightly more complicated than just looking at the object's keys. There is actually an algorithm that traverse the prototype chain. First, JavaScript looks at the keys on the object. If the requested key wasn't found, it then looks on the keys of the prototype object. If it still wasn't found, it looks at the prototype's prototype, and so on. This is how inheritance is implemented in JavaScript!

Every time a new Person is created, age and name fields are added to the object. However only a single reference to the prototype object is added. So no matter how many instances of Person are created or how many methods are on the class, only a single prototype object is generated.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes