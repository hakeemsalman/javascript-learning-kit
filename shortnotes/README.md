# Short Notes

- [Short Notes](#short-notes)
  - [Closure:](#closure)
  - [Lexical Environment](#lexical-environment)


## Closure:

- A [closure](https://en.wikipedia.org/wiki/Closure_(computer_programming)) is a function that remembers its outer variables and can access them.
- In some languages, that's not possible, or a function should be written in a special way to make it happen.
- But as explained above, in JavaScript, all functions are naturally closures (there is only one exception, to be covered in The "new Function" syntax).

## Lexical Environment

- Lexical Environment:
  - Every function in JavaScript is created with a hidden `[[Environment]]` property referencing the environment where it was created.
  - Functions *remember* the variables from their environment due to `this` property.