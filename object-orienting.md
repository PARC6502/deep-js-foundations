# Deep Javascript Foundations

***

### Object Orienting

* `this` keyword
* Prototypes
* `class {}` keyword added in ES6 (not recommended by Kyle Simpson)
* "Inheritance" vs "Behavior Delegation" (Objects Linked to Other Objects)

#### `this` keyword

> Every* function, while executing, has a reference to its current execution context, called `this`.

* Javascript's version of "dynamic scope" is `this` 

```javascript
function foo(){
  console.log(this.bar);
}

var bar = "bar1";
var o2 = { bar: "bar2", foo: foo };
var o3 = { bar: "bar3", foo: foo };
var obj = { bar: "bar4" };
var obj2 = { bar: "bar5" };

foo();			// "bar1"
o2.foo();		// "bar2"
o3.foo();		// "bar3"
foo.call(obj);	// "bar4"

var orig = foo;
foo = function(){ orig.call(obj); };

foo(); 			// "bar4"
foo.call(obj2)	// "bar4"
```

* The only that matters is how the function is called:
  * Default binding rule: `this` keyword defaults to global object if none of the other 3 ways of calling are met. In "strict mode" it defaults to `undefined` so it would be an error. 
  * Implicit binding: set the `this` keyword to the context object 
  * Explicit binding: using the `call` or `apply` methods, the `this` keyword is set to the first arg in those methods (as in line **14**)
  * Hard binding: as in line **17**, the this is permanently set, on line **20** the other context is ignored and `"bar4"` is still printed (can be considered variation of explicit binding)

##### this: hard binding

```javascript
function foo(baz, bam) {
  console.log(this.bar + " " + baz + " " + bam);
}

var obj = { bar: "bar" };
foo = foo.bind(obj);

foo("baz", "bam");		// "bar baz bam"
```

* Using the `.bind` function hard binds to a specific object. Now no matter where `foo` is called its `this` will always refer to `obj` 

##### this: new binding

```javascript
function foo() {
  this.baz = "baz";
  console.log(this.bar + " " + baz);
}

var bar = "bar";
var baz = new foo();	// ???
```

* `new` keyword in JS has nothing to do with instantiating classes

   

Four things happen when `new` keyword placed in front of function call:

1. Creates new, empty object
2. *Newly created object is linked to another object
3. Newly created object from step 1 passed in as the `this` context to the function call
4. If the function does not return it's own object, return that `this`

`new` is AKA "constructor call"

##### this: determining precedence

1. is the function called by `new`?
2. Is the function called by `call()` or `apply()`?
   * `bind()` effectively uses `apply()`
3. Is the function called on a context object? (e.g. `obj.foo()`)
4. DEFAULT: global object (except in strict mode)

Arrow function is said to have "lexical this behaviour", a `this` that behaves by lexical rules rather than the `this` binding rules.