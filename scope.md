# Deep Javascript Foundation

***

### Scope

Javascript is a compiled language.

Simplified: javascript is function scoped* (block scoped through let and const?)

```javascript
var foo = "bar";

function bar() {
  var foo = "baz";
}

function baz(foo) {
  foo = "bam";
  bam = "yay";
}
```

* If `console.log(foo);` is inserted on line **3.5** it would show `undefined`. 
  * Happens because of the multipass. (simplified) An earlier pass of the compiler creates memory space with the scope manager for formal declarations, so the scope manager "will know", at runtime, that a var with that name exists in its scope before it gets to it.
* If line **4** did not exist `bar` would be the output.
* Once a variable is "shadowed" (var with same name created in scope below it) it is impossible to lexically access the var with same name in parent scope(s)
* Parameters are formal variable declaration (local to the function they are paremeters of)
* Line **9** is not a formal declaration
* Line **9** implicitly creates a `bam` variable in the *global scope* at runtime
* Line **9** is an error in strict mode (`ReferenceError: bam is not defined`)

Should be using strict mode (`"use strict";`) in all my programs.

Think of strict mode as list of suggestions that should be followed so code runs faster 

In the case of function execution, parathesis kind of work like an operator: the name of the function retrieves it, and the paranthesis execute it.

In non-strict mode: unfulfilled LHS references implicitly create a global reference. Unfulfilled RHS references always throw a `ReferenceError`

#### Lexical Scope

* Most of the time happens at compile time. (So lookup process is often metaphorical.)

```javascript
var foo = function bar() {
  // some code
}

bar(); // Error!
```

* Line 5 throws an error because there is no `bar` identifier in the global scope
* `bar` exists in its own scope, as a reference to the function, but not outside of it. **Key difference between function identifier and function expression.**  

```javascript
var foo;

try {
  foo.length;
}
catch (err) {
  console.log(err); // logs TypeError
}

console.log(err); // ReferenceError
```

* The catch clause declares the `err` variable
* `err` is **block scoped** to the catch clause (available only in catch clause)

> You always need to be smarter than your own tools

#### Named Function Expressions

```javascript
var clickHandler = function(){
  // anonymous function expressions
};

var keyHandler = function keyHandler(){
  // named function expression
};
```

> I prefer function declarations [...] if a function is going to have anymore than 2 or 3 lines of code
>
> ​	~Kyle Simpson

* Should always prefer named function expressions over anonymous function expressions
  - Self-reference is useful (for things like recursion, unbinding click-handler, etc)
  - More debuggable stack traces (name is used in stack traces)
  - More self-documenting code

