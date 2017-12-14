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
>
> ​	~Kyle Simpson

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

#### IIFE Pattern

> *Question: Is this not obsolete with ES6 block scoping `let` and `const`?* 

```javascript
var foo = "foo";

(function IIFE(global){
  var foo = "foo2";
  console.log(foo); // "foo2"
})(window);

console.log(foo); // "foo"
```

* Using an IIFE allows renaming vars that are in outer scope within the scope of the IIFE (lines **3** and **6**)
  * In the above code block `window` is renamed to `global` within the `IIFE` scope
  * Could pass in *JQuery object* and rename it to `$` to be sure that's what the dollar sign refers to


#### Block Scoping

> `let` is not the new `var`, both are useful
>
> ​	Kyle Simpson

 ```javascript
{
  let temp1, temp2;
  temp1 = someFunc();
  temp2 = someFunc();
  globalVar = temp1+temp2;
}
 ```

* Explicit `let` block: it's very clear `temp1` and `temp2` are scoped to that block

```javascript
function repeat(fn,n) {
  var result;
  for (let i = 0; i<n; i++) {
  	result = fn(result, i);
  }
  return result;
}
```

* `let` should always be used in `for` loops. If you want access to the `i` use an external variable
* Here differentiating between `let` and `var` signals that `result` is scoped to the entire function, while `i` is only scoped to the for loop block. 

##### Places Where var > let

```javascript
function lookupRecord(searchStr) {
  try {
    var id = getRecord(searchStr);
  }
  catch(err) {
    var id = -1;
  }
  return id;
}
```

* Here, if `let` was used, `id` would be scoped to the try/catch blocks (but can't you just declare outside?)
* `var` allows you to redeclare, which he thinks helps with readability.

> You should ideally be able to see the variable declaration, and where it is used, on the same screen. 
>
> ​	Kyle Simpson

* `const` is usually not useful, as it does not make it's value immutable, just means it can't be reassigned, so can confuse people

> So just because some people don't understand how `const` works, we shouldn't use it???

#### Challenge 2 Notes

```javascript
function addProject(description) {
  var projectEntryData;
  
  {
    let projectId;
    	projectId = Math.round(Math.random()*1E4);
    	projectEntryData = {id: projectId, description, work:[]}
    projects.push(projectEntryData);
    
    addProjectToList(projectEntryData);
    addProjectSelection(ProjectEntryData);
  }
}
```

* Concept in programming, principle of least exposure: keep something as private/hidden as necessary
* `projectId` is only used once in the function so it is placed in it's own code block 

#### Hoisting

Metaphor for how JS works (but not really how it works).

Functions and declarations can be thought of as being "hoisted": It doesn't matter where a function literal or var declarations happens inside of a scope, it is as if they were written at the beginning of the scope. 

`var` does two things:

1. variable added at compile time to the enclosing lexical scope
2. at runtime, when scope is entered, any variable added to lexical environment are initialised to undefined

`let` keyword doesn't do *2*, it adds variable to lexical scope but it is only initialised to undefined where it is declared. So `let` does not "hoist". 