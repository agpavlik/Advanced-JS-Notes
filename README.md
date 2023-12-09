## JavaScript: The Advanced Concepts

- [Javascript Engine](#1)
- [Writing Optimized Code](#2)
- [Call Stack and Memory Heap](#3)

### 📒 Javascript Engine <a name="1"></a>

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

### 📒 Writing Optimized Code <a name="2"></a>

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

> CheatSheetv - https://zerotomastery.io/cheatsheets/javascript-cheatsheet-the-advanced-concepts/#writing-optimized-code

> Javascript Hidden Classes and Inline Caching - https://richardartoul.github.io/jekyll/update/2015/04/26/hidden-classes.html

> Managing arguments - https://github.com/petkaantonov/bluebird/wiki/Optimization-killers#3-managing-arguments

> WebAssembly - https://webassembly.org/

### 📒 Call Stack and Memory Heap <a name="3"></a>

The JavaScript engine does a lot of work for us, but 2 of the biggest jobs are reading and executing it. We need a place to store and write our data and a place to keep track line by line of what's executing.
That's where the call stack and the memory heap come in.

The `memory heap` is a place to store and write information so that we can use our memory appropriately. It is a place to allocate, use, and remove memory as needed. Think of it as a storage room of boxes that are unordered.

The `call stack` keeps track of where we are in the code, so we can run the program in order.

### 📒 Stack Overflow <a name="4"></a>

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

#### 🚩 R <a name="5"></a>
