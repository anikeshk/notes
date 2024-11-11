---
title: JavaScipt Notes
layout: page
parent: Leetcode
nav_order: 2
---

Notes from [LeetCode's 30 Days of JavaScript Challenge](https://leetcode.com/studyplan/30-days-of-javascript/).

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