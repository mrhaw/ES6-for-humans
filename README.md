# ES6 for Humans
<br>

### 1. let, const and block spacing

<code>let</code> allows you to create declarations which are bound to any block, called block scoping. Instead of using <code>var</code>, which provides function scope, it is recommended to use <code>let</code> in ES6.

```javascript
var a = 2; 
{	let a = 3;
	console.log(a); // 3}console.log(a); // 2
```

Another form of block-scoped declaration is the <code>const</code>, which creates constants. In ES6, a <code>const</code> represents a constant reference to a value. In other words, the value is not frozen, just the assignment of it. Here's a simple example:

```javascript
{	const ARR = [5,6];
	ARR.push(7);
	console.log(ARR); // [5,6,7]	ARR = 10; // TypeError
}
```

A few things to keep in mind:

* No Hoisting takes place when we use <code>let</code> keyword.
* <code>let</code> and <code>const</code> are scoped to the nearest enclosing block.
* When using <code>const</code>, use CAPITAL_CASING.
* <code>const</code> has to be defined with its declaration.

<br>

### 2. Arrow Functions

Arrow Functions are a short-hand notation for writing functions in ES6. The arrow function definition consists of a parameter list <code>( ... )</code>, followed by the <code>=></code> marker and a function body.

```javascript
var getPrice = function() {
	return 4.55;
};

// Implementation with Arrow Function
var getPrice = () => 4.55;
```
Note that in the above example, the <code>getPrice</code> arrow funtion is implemented with "concise body" which doesnot need an explicit return statement.

Here is an example with the usual "block body"

```javascript
let arr = ['apple', 'banana', 'orange'];

let breakfast = arr.map( fruit => {
	console.log(fruit + 's');
} );  // apples bananas oranges
```

**Behold! There is more...**

Arrow functions don't just make the code shorter. They are closely related to <code>this</code> binding behaviour. 

Arrow functions behaviour with <code>this</code> keyword varies from that of normal functions. Each function in JavaScript defines its own <code>this</code> context but Arrow functions capture the <code>this</code> value of the enclosing context. Check out the following code:

```javascript
function Person() {
	// The Person() constructor defines `this` as an instance of itself.
  	this.age = 0;

  	setInterval(function growUp() {
    	// In non-strict mode, the growUp() function defines `this` 
    	// as the global object, which is different from the `this`
    	// defined by the Person() constructor.
    	this.age++;
  	}, 1000);
}
var p = new Person();
```

In ECMAScript 3/5, this issue was fixed by assigning the value in <code>this</code> to a variable that could be closed over.

```javascript
function Person() {
	var self = this;
	self.age = 0;
	
	setInterval(function growUp() {
    	// The callback refers to the `self` variable of which
    	// the value is the expected object.
    	self.age++;
  	}, 1000);
}
```

As mentioned above, Arrow functions capture the this value of the enclosing context, so the following code works as expected.

```javascript
function Person(){
	this.age = 0;

	setInterval(() => {
    	this.age++; // |this| properly refers to the person object
  	}, 1000);
}

var p = new Person();
```
[Read more about 'Lexical this' in arrow functions here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#Lexical_this)

<br>

### 3. Default Function Parameters

ES6 allows you to set default parameters in function definitions. Here is a simple illustration.

```javascript
let getFinalPrice = (price, tax=0.7) => price + price * tax;
getFinalPrice(500); // 850
```

<br>

### 4. Spread / Rest Operator

<code>...</code> operator is referred to as spread or rest operator, depending on how and where it is used.

When used with any iterable, it acts as to "spread" it into individual elements:

```javascript
function foo(x,y,z) {
	console.log(x,y,z);
}

let arr = [1,2,3];
foo(...arr); // 1 2 3
```

The other common usage of <code>...</code> is gathering a set of values together into an array. This is referred as "rest" operator.

```javascriptfunction foo(...args) {
	console.log(args);}foo( 1, 2, 3, 4, 5); // [1, 2, 3, 4, 5]
```

<br>

### 5. Object Literal Extensions 

ES6 allows declaring object literals by providing shorthand syntax for initializing properties from variables and defining function methods. It also enables the ability to have computed property keys in an object literal definition.

```javascript
function getCar(make, model, value) {
	return {
		// with property value shorthand
		// syntax, you can omit the property
		// value if key matches variable
		// name
		make,  // same as make: make
		model, // same as model: model
		value, // same as value: value

		// computed values now work with
		// object literals
		['make' + make]: true,

		// Method definition shorthand syntax
		// omits `function` keyword & colon
		depreciate() {
			this.value -= 2500;
		}
	};
}

let car = getCar('Kia', 'Sorento', 40000);

// output: {
//     make: 'Kia',
//     model:'Sorento',
//     value: 40000,
//     makeKia: true,
//     depreciate: function()
// }
```

<br>

### 6. Octal and Binary Literals

ES6 has new support for octal and binary literals. 
Prependending a number with <code>0o</code> or <code>0O</code> would convert it into octal value. Have a look at the following code:

```javascript
let oValue = 0o10;
console.log(oValue); // 8

let bValue = 0b10; // 0b or 0B for binary
console.log(bValue); // 2
```

<br>

### 7. Array and Object Destructuring

Desctructuring helps in avoiding the need for temp variables when dealing with object and arrays.

```javascript
function foo() {
	return [1,2,3];}
let arr = foo(); // [1,2,3]

let [a, b, c] = foo();
console.log(a, b, c); // 1 2 3

function bar() {
	return {		x: 4,
		y: 5,
		z: 6	};
}
let {x: x, y: y, z: z} = bar();
console.log(x, y, z); // 4 5 6
```

<br>

### 8. super in Objects

ES6 allows to use <code>super</code> method in (classless) objects with prototypes. Following is a simple examle:

```javascript
var parent = {
	foo() {
		console.log("Hello from the Parent");
	}
}

var child = {
	foo() {
		super.foo();
		console.log("Hello from the Child");
	}
}

Object.setPrototypeOf(child, parent);
child.foo(); // Hello from the Parent
			 // Hello from the Child
```

<br>

### 9. Template Literal and Delimiters

ES6 introduces an easier way to add interpolation which are evaluated automatically. 

* <code>\`${ ... }\`</code> is used for rendering the variables. 
* <code>\`</code> Backtick is used as delimiter.

```javascript
let user = 'Kevin';
console.log(`Hi ${user}!`); // Hi Kevin!
```

<br>

### 10. for...of vs for...in
* <code>for...of</code> iterates over iterable objects, such as array.

```javascript
let nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname of nicknames) {
	console.log(nickname);
} 
Result: di, boo, punkeye
```

* <code>for...in</code> iterates over all enumerable properties of an object.

```javascript
let nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname in nicknames) {
	console.log(nickname);
} 
Result: 0, 1, 2, size
```

<br>

### 11. Classes in ES6

ES6 introduces new class syntax. One thing to note here is that ES6 class is not a new object-oriented inheritance model. They just serve as a syntactical sugar over JavaScript's existing prototype-based inheritance.

One way to look at a class in ES6 is just a new syntax to work with prototypes and contructor functions that we'd use in ES5. 

```javascript
class Task {
	constructor() {
		console.log("task instantiated!");
	}
	showId() {
		console.log(23);
	}
}

console.log(typeof Task); // function
```

**extends and super in classes**

Consider the following code:

```javascript
class Car {
	constructor() {
		console.log("Creating a new car");
	}
}

class Porche extends Car {
	constructor() {
		super();
		console.log("Creating Porche");
	}
}

let c = new Porche(); 
// Creating a new car
// Creating Porche
```

<code>extends</code> allow child class to inherit from parent class in ES6. It is important to note that the derived constructor must call super().

Also, you can call parent class's method in child class's methods using <code>super.parentMethodName()</code>

[Read more about classes here](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

A few things to keep in mind: 

* Class declarations are not hoisted. You first need to declare your class and then access it, otherwise ReferenceError will be thrown.
* There is no need to use <code>function</code> keyword when defining functions inside a class definition.

<br>

### 12. Symbol

A symbol is a unique and immutable data type introduced in ES6. The purpose of a symbol is to generate a unique identifier but you can never get any access to that identifier.

Here’s how you create a symbol:

```javascript
var sym = Symbol( "some optional description" );
console.log(typeof sym); // symbol
```

Note that you cannot use new with Symbol(..).

If a symbol is used as a property/key of an object, it’s stored in a special way that the property will not show up in a normal enumeration of the object’s properties.

```javascript
var o = { 
	val: 10,    [ Symbol("random") ]: "I'm a symbol",
};

console.log(Object.getOwnPropertyNames(o)); // val
```

To retrieve an object’s symbol properties, use <code>Object.getOwnPropertySymbols(o)</code>


<br>

### 13. Iterators

An iterator accesses the items from a collection one at a time, while keeping track of its current position within that sequence. It provides a <code>next()</code> method which returns the next item in the sequence. This method returns an object with two properties: done and value.

ES6 has <code>Symbol.iterator</code> which specifies the default iterator for an object. Whenever an object needs to be iterated (such as at the beginning of a for..of loop), its @@iterator method is called with no arguments, and the returned iterator is used to obtain the values to be iterated.

Let’s look at an array, which is an iterable, and the iterator it can produce to consume its values:

```javascript
var arr = [11,12,13];
var itr = arr[Symbol.iterator]();

itr.next(); // { value: 11, done: false }itr.next(); // { value: 12, done: false }itr.next(); // { value: 13, done: false }
itr.next(); // { value: undefined, done: true }
```

Note that you can write custom iterators by defining <code>\[Symbol.iterator]()</code> with the object definition.

<br>

### 14. Generators

Generator functions are a new feature in ES6 that allow a function to generate many values over time by returning an object which can be iterated over to pull values from the function one value at a time.

A generator function returns an **iterable oject** when it's called.
It is written using the new <code>*</code> syntax as well as the new <code>yield</code> keyword introduced in ES6.

```javascript
function *infiniteNumbers() {
	var n = 1;
  	while (true){
    	yield n++;
  	}
}

var numbers = infiniteNumbers(); // returns an iterable object

numbers.next(); // { value: 1, done: false }
numbers.next(); // { value: 2, done: false }
numbers.next(); // { value: 3, done: false }
```

Each time yield is called, the yielded value becomes the next value in the sequence.

Also, note that generators compute their yielded values on demand, which allows them to efficiently represent sequences that are expensive to compute, or even infinite sequences.

<br>

### 15. Promises

ES6 has native support for promises. A promise is an object that is waiting for an asynchronous operation to complete, and when that operation completes, the promise is either fulfilled(resolved) or rejected.

The standard way to create a Promise is by using the <code>new Promise()</code> constructor which accepts a handler that is given two functions as parameters. The first handler (typically named <code>resolve</code>) is a function to call with the future value when it's ready; and the second handler (typically named <code>reject</code>) is a function to call to reject the Promise if it can't resolve the future value.

```javascript
var p = new Promise(function(resolve, reject) {  
	if (/* condition */) {
    	resolve(/* value */);  // fulfilled successfully
   	}
   	else {
   		reject(/* reason */);  // error, rejected
   	}
});
```

Every Promise has a method named <code>then</code> which takes a pair of callbacks. The first callback is called if the promise is resolved, while the second is called if the promise is rejected.

```javascript
p.then((val) => console.log("Promise Resolved", val),
       (err) => console.log("Promise Rejected", err));
```
