## JavaScript: The Advanced Concepts

CheatSheet: https://zerotomastery.io/cheatsheets/javascript-cheatsheet-the-advanced-concepts/#writing-optimized-code

- [JS Foundation](#1)
  - [JS Engine](#2)
  - [Writing Optimized Code (Memoization, Hidden Classes, Inline Caching, Managing arguments, WebAssembly)](#3)
  - [Call Stack and Memory Heap](#4)
  - [Stack Overflow](#5)
  - [Garbage Collection](#6)
  - [JS Runtime](#7)

---

### JS FOUNDATION <a name="1"></a>

### 📒 Javascript Engine <a name="2"></a>

A `JavaScript engine` is a computer program that you give JavaScript code to and it tells the computer how to execute it. Basically a translator for the computer between JavaScript and a language that the computer understands.

There are many JavaScript Engines out there and typically they are created by web browser vendors. All engines are standardized by `ECMA Script` or `ES`.

![](1.png)

`Parsing` is the process of analyzing the source code, checking it for errors, and breaking it up into parts.

The parser produces a data structure called the `Abstract Syntax Tree` or `AST`. AST is a tree graph of the source code that does not show every detail of the original syntax, but contains structural or content-related details. Certain things are implicit in the tree and do not need to be shown, hence the title abstract.

An `interpreter` directly executes each line of code line by line, without requiring them to be compiled into a machine language program. Interpreters can use different strategies to increase performance. They can parse the source code and execute it immediately, translate it into more efficient machine code, execute precompiled code made by a compiler, or some combination of these.

The `compiler` works ahead of time to convert instructions into a machine-code or lower-level form so that they can be read and executed by a computer. It runs all of the code and tries to figure out what the code does and then compiles it down into another language that is easier for the computer to read.

`Babel` (https://babeljs.io/) is a Javascript compiler that takes your modern JS code and returns browser compatible JS (older JS code). `Typescript` (https://www.typescriptlang.org/) is a superset of Javascript that compiles down to Javascript. Both of these do exactly what compilers do. Take one language and convert into a different one!

In modern engines, the interpreter starts reading the code line by line while the profiler watches for frequently used code and flags then passes is to the compiler to be optimized.

In the end, the JavaScript engine takes the bytecode the interpreter outputs and mixes in the optimized code the compiler outputs and then gives that to the computer. This is called `"Just in Time"` or `JIT` Compiler.

### 📒 Writing Optimized Code <a name="3"></a>

`Memoization` is a way to cache a return value of a function based on its parameters. This makes the function that takes a long time run much faster after one execution. If the parameter changes, it will still have to reevaluate the function.

```javascript
// Bad Way
function addTo80(n) {
  console.log('long time...')
  return n + 80
}

addTo80(5)
addTo80(5)
addTo80(5)

// long time... 85
// long time... 85
// long time... 85

// Memoized Way
functions memoizedAddTo80() {
  let cache = {}
  return function(n) { // closure to access cache obj
    if (n in cache) {
      return cache[n]
    } else {
      console.log('long time...')
      cache[n] = n + 80
      return cache[n]
    }
  }
}
const memoized = memoizedAddTo80()

console.log('1.', memoized(5))
console.log('2.', memoized(5))
console.log('3.', memoized(5))
console.log('4.', memoized(10))

// long time...
// 1. 85
// 2. 85
// 3. 85
// long time...
// 4. 90
```

More information:

> Javascript Hidden Classes and Inline Caching - https://richardartoul.github.io/jekyll/update/2015/04/26/hidden-classes.html

> Managing arguments - https://github.com/petkaantonov/bluebird/wiki/Optimization-killers#3-managing-arguments

> WebAssembly - https://webassembly.org/

### 📒 Call Stack and Memory Heap <a name="4"></a>

The JavaScript engine does a lot of work for us, but 2 of the biggest jobs are reading and executing it. We need a place to store and write our data and a place to keep track line by line of what's executing.
That's where the call stack and the memory heap come in.

The `memory heap` is a place to store and write information so that we can use our memory appropriately. It is a place to allocate, use, and remove memory as needed. Think of it as a storage room of boxes that are unordered.

The `call stack` keeps track of where we are in the code, so we can run the program in order.

### 📒 Stack Overflow <a name="5"></a>

So what happens if you keep calling functions that are nested inside each other? When this happens it's called a **stack overflow**.

```javascript
// When a function calls itself,
// it is called RECURSION
function inception() {
  inception();
}

inception();
// returns Uncaught RangeError:
// Maximum call stack size exceeded
```

### 📒 Garbage Collection <a name="6"></a>

JavaScript is a garbage collected language. If you allocate memory inside of a function, JavaScript will automatically remove it from the memory heap when the function is done being called.
However, that does not mean you can forget about `memory leaks`. No system is perfect, so it is important to always remember memory management. JavaScript completes garbage collection with a `mark` and `sweep` method.

> https://developers.soundcloud.com/blog/garbage-collection-in-redux-applications

### 📒 JavaScript Runtime <a name="7"></a>

When you visit a web page, you run a browser to do so (Chrome, Firefox, Safari, Edge). Each browser has its own version of `JavaScript Runtime` with a set of `Web API's`, methods that developers can access from the window object.

In a synchronous language, only one thing can be done at a time. Imagine an alert on the page, blocking the user from accessing any part of the page until the OK button is clicked. If everything in JavaScript that took a significant amount of time, blocked the browser, then we would have a pretty bad user experience.

This is where `concurrency` and the `event loop` come in. When you run some JavaScript code in a browser, the engine starts to parse the code. Each line is executed and popped on and off the call stack.

Web API's are not something JavaScript recognizes, so the parser knows to pass it off to the browser for it to handle. When the browser has finished running its method, it puts what is needed to be ran by JavaScript into the callback queue. The callback queue cannot be ran until the call stack is completely empty. So, the event loop is constantly checking the call stack to see if it is empty so that it can add anything in the callback queue back into the call stack. And finally, once it is back in the call stack, it is ran and then popped off the stack.

Until 2009, JavaScript was only run inside of the browser. That is when Ryan Dahl decided it would be great if we could use JavaScript to build things outside the browser. He used C and C++ to build an executable (exe) program called `Node JS`. Node JS is a JavaScript runtime environment built on Chrome's V8 engine that uses C++ to provide the event loop and callback queue needed to run asynchronous operations.

#### 🚩 R <a name="5"></a>
