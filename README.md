# Javascript Preparation Topics

> *Click &#9733; if you like the project. Your contributions are heartily &#9825; welcome.*

<br/>

### Other cheatsheet or notes links

- [Javascript Array learning kit](https://hakeemsalman.github.io/javascript-array-cheatsheet/)
- [Javascript String learning kit](https://hakeemsalman.github.io/javascript-string-cheatsheet/)
- [Javascritp Practice Questions kit](https://github.com/hakeemsalman/javascript-practice-questions)
- [HTML learning kit](https://hakeemsalman.github.io/html-learning-kit/)

--- 

- [Javascript Preparation Topics](#javascript-preparation-topics)
    - [Other cheatsheet or notes links](#other-cheatsheet-or-notes-links)
- [Topics Wise](#topics-wise)
  - [Nested functions](#nested-functions)
  - [Lexical Environment](#lexical-environment)
    - [1. Variables](#1-variables)
    - [2. Functions declarations](#2-functions-declarations)
    - [3. Inner and outer Lexical Environment](#3-inner-and-outer-lexical-environment)
    - [4. Returning a function](#4-returning-a-function)
  - [Closure](#closure)
    - [definition](#definition)
    - [You should know these concepts before moving on Closure.](#you-should-know-these-concepts-before-moving-on-closure)
    - [Real-life optimizations](#real-life-optimizations)
- [Useful links](#useful-links)



# Topics Wise

## Nested functions


> NOTE: The nested function's `[[Enviroment]]` is set the moment outer function is called and remains the same no matter from where you call this returned nested function.

## Lexical Environment

- In JavaScript, every **running function**, **code block** `{...}`, and the **script** as a whole have an *internal (hidden) associated object* known as the **Lexical Environment**.

- The Lexical Environment object consists of **two** parts:
  - *Environment Record*: an object that stores all local variables as its properties (and some other information similar to `this` value).
  - *Reference record*: A reference to the outer lexical environment, the one associated with the outer code.

> <span style="color: orange;">**NOTE:**</span> "**Lexical Environment**" is a specification object: it only exists “theoretically” in the language specification to describe how things work. We can’t get this object in our code and manipulate it directly.


- To understand this, split into smaller topics:

### 1. Variables

- A **variable** is just a property of the special internal object, `Environment Record`.
- *To get or change a variable* means is -> *to get or change a property of that object*.

![lexical scope](/assets/lexical-scope.png)
![alt text](/assets/lexical%20scope%20variable.png)

![lexical execution](/assets/lexical%20execution.png)

- Rectangles on the right-hand side demonstrate how the global Lexical Environment changes during the execution:
  1. When the script starts, the Lexical Environment is pre-populated with all declared variables.
  2. Initially, they are in the `Uninitialized` state. That’s a special internal state, it means that the engine knows about the variable, but it cannot be referenced until it has been declared with `let`. It’s almost the same as if the variable didn’t exist.
  3. Then `let` phrase definition appears. There’s no assignment yet, so its value is `undefined`. We can use the variable from this point forward.
  4. `phrase` is assigned a value.
  5. `phrase` changes the value.

### 2. Functions declarations

- A function is also a value, like a variable.
- The difference is that a Function Declaration is instantly fully initialized.
- When a Lexical Environment is created, a Function Declaration immediately becomes a ready-to-use function (unlike let, that is unusable till the declaration).
- That’s why we can use a function, declared as Function Declaration, even before the declaration itself.
- For example, here’s the initial state of the global Lexical Environment when we add a function:
- ![alt text](/assets/lexical%20function%20scope.png)

> <span style="color: orange;">**NOTE:**</span> It applies to only Function declation `function say(){.....}` and not to Function expression `let say = function(name)...`


### 3. Inner and outer Lexical Environment

- When a function runs, at the beginning of the call, a new Lexical Environment is created automatically to store local variables and parameters of the call.

> When the code wants to access a variable - the inner Lexical Environment is searched first, then the outer one, then the more outer one and so on until the global one.
> If a variable is not found anywhere, that's an error in strict mode (without use strict, an assignment to a non-existing variable creates a new global variable, for compatibility with old code).

![alt text](/assets/innerouterLexicalscope.png)

1. For the `name` variable, the `alert` inside `say` finds it immediately in the inner Lexical Environment.
1. When it wants to access `phrase`, then there is no `phrase` locally, so it follows the reference to the outer Lexical Environment and finds it there.

### 4. Returning a function

```js
function makeCounter() {
  let count = 0;

  return function() {
    return count++;
  };
}

let counter = makeCounter();
```

![counter function](/assets/counter-function-one.png)

- All functions remember the Lexical Environment in which they were made. Technically, there’s no magic here: all functions have the hidden property named `[[Environment]]`, that keeps the reference to the Lexical Environment where the function was created:

![counter function two](/assets/counter-function-two.png)
- So, counter.`[[Environment]]` has the reference to {count: 0} Lexical Environment. That’s how the function remembers where it was created, no matter where it’s called. The `[[Environment]]` reference is set once and forever at function creation time.
- Later, when counter() is called, a new Lexical Environment is created for the call, and its outer Lexical Environment reference is taken from counter.`[[Environment]]`:

![counter-function-three](/assets/counter-function-three.png)

> A variable is updated in the Lexical Environment where it lives.


## Closure

### definition

- A closure is a function that remembers its outer variables and can access them.
- In JavaScript, all functions are naturally closures.
- Lexical Environment:
  - Every function in JavaScript is created with a hidden `[[Environment]]` property referencing the environment where it was created.
  - Functions *remember* the variables from their environment due to `this` property.
- Example of Closure:
    - ```js
      function outer() {
        let name = "JavaScript";
        return function inner() {
          console.log(name); // Accesses the outer variable
        };
      }
      const func = outer();
      func(); // Output: JavaScript
      ```

### You should know these concepts before moving on Closure.

1. [Lexical Environment](#lexical-environment)

<table>
  <tr>
  <td>


  - A [closure](https://en.wikipedia.org/wiki/Closure_(computer_programming)) is a function that remembers its outer variables and can access them. In some languages, that’s not possible, or a function should be written in a special way to make it happen. But as explained above, in JavaScript, all functions are naturally closures (there is only one exception, to be covered in The "new Function" syntax).
  - That is: they automatically remember where they were created using a hidden [[Environment]] property, and then their code can access outer variables.
  - When on an interview, a frontend developer gets a question about “what’s a closure?”, a valid answer would be a definition of the closure and an explanation that all functions in JavaScript are closures, and maybe a few more words about technical details: the [[Environment]] property and how Lexical Environments work.


  </td>
  </tr>
</table>

### Real-life optimizations

- As we've seen, in theory while a function is alive, all outer variables are also retained.
- But in practice, JavaScript engines try to optimize that. They analyze variable usage and if it’s obvious from the code that an outer variable is not used – it is removed.


# Useful links

[requestAnimationFrame test timing](https://event-loop-tests.glitch.me/raf-timing-test.html)
[while true test ](https://event-loop-tests.glitch.me/while-true-test.html)
[setTimeout loop test](https://event-loop-tests.glitch.me/setTimeout-loop.html)