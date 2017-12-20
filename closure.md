# Deep Javascript Foundations

***

### Closure

> Closure is when a function "remembers" its lexical scope even when the function is executed outside of that lexical scope

#### Simple code example for closure

```javascript
function foo() {
  var bar = "bar";
  
  function baz() {
    console.log(bar);
  }
  
  bam(baz);
}

function bam(baz) {
  baz(); // "bar"
}

foo();
```

* In line **12** `baz` is executed outside of its scope, but it still has access to the `bar` variable from the `foo` scope. "function `baz` closes over variable `bar`"

`setTimeout` and click handlers often use closure.

#### Closure: loops

```javascript
for (var i=1; i<=5; i++) {
  setTimeout(function(){
    console.log("i: ", + i);
  },i*1000);
}
```

* This code logs *6* 5 times.
* This is because the function in `setTimeout` "closes" over the `i` (which is a var in the function scope the for lope is in). In each loop the same `i` is referenced 


#### Module Patterns

#####Not a module pattern

````javascript
var foo = {
  o: {bar: "bar"},
  bar() {
    console.log(this.o.bar);
  }
};

foo.bar();
````

* This is not a module pattern
* Encapsulation is what's necessary to realise the module pattern
* Encapsulation means hiding of information that's not necessary to be seen by the outer world
* If a method or a variable isn't needed, no need to expose it to those parts of the code

##### Classic Module Pattern

```javascript
var foo = (function(){
  var o = {bar: "bar"};
  return {
    bar: function(){
      console.log(o.bar);
    }
  };
  
})();

foo.bar();
```

* Only stuff in the returned object is exposed to the outer code
* `foo.o` wouldn't work but `foo.bar()` does

Module pattern is not just about looking good, it's layering code and separating concerns

#####Modification of Classic Module Pattern

```javascript
var foo = (function() {
  var publicAPI = {
    bar: function(){
      pulicAPI.baz();
    },
    baz: function(){
      console.log("baz");
    }
  };
  return publicAPI;
})();

foo.bar();
```

* Kind of like a singleton module 
* If it wasn't an IIFE, it would be like a module factory function

##### ES6+ Module Pattern

**foo.js**

```javascript
var o = { bar: "bar" };

export function bar() {
  return o.bar;
}
```

```javascript
import { bar } from "foo.js";

bar();		// "bar"

import * as foo from "foo.js";

foo.bar();	//"bar"
```

> Don't adopt ES6 modules without adopting HTTP2 (for HTTP1 loading everything in one file was quicker, for 2 many files is better)

* ES6 modules are singletons by default
* Incompatible with node modules (npm packages) as of the video
* Recommendation to hold off on module syntax for now

> Me: What is the difference between a "module factory" and a "class" (as in OOP)?

There is a difference but I'm not really getting it right now...