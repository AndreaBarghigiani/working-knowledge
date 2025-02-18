# 3. Values and Variables
## Primitives are immutable
This is a mantra that we commonly tell ourselves, but do we really know what it means?

For example, when working with a string the following is a fine codeblock:
```js
let str = "hello";
console.log(str[0]); // "h"
```

This could let us think that we can do something like that:
```js
str[0] = "p"; 
console.log(str); // It'll print "pello"?
```

Unfortunately **not!**

While we can access a single character of the string with `str[0]` we're **not able to change it**, because strings are primitives!

What about this?
```js
let num = 42;
num.question = "ultimate question"
```

Again, we will get nothing. JavaScript could throw an error depending on our setup, but that's out of the scope for now.

The critical thing to remember is that **primitives in JavaScript are immutable**. They cannot be changed, a variable could be reassigned to a different primitive but we cannot change them.

# Variables point to a value
It does not matter if a variable points to a primitive value, to an object or an array...

A variable is just a name that is wired to a value, and as such we can wire it to a different value.
```js
let str = "hello";
str = "there";
console.log(str); // "there"
```

Here we **haven't changed the value**, we just disconnected `str` from the primitive string `"hello"` and pointed it to the other primitive string `"there"`.

## Rules of assignment
So, we've just discovered that a variable is simply *a name that is wired to a value*, but there are some specifics about how we can assign this connection:
- the left side of an assignment **must be a "wire"**, and not a value
- the right side of an assignment **must be an expression** that returns a value

To demonstrate the first point, let's study the following:
```js
123 = "numbers"; // Wrong! A (primitive) number cannot be a variable
"hello" = "world"; // Again, wrong! 
```

In both examples, we're trying to use a primitive as variable names; this is not possible because **we can only point to primitives** and we cannot use them for something else.

To properly understand the second point, we need a little detour. We know that the right side of an assignment declaration has to be an expression. Expressions in JavaScript are something that **returns a value**. Writing something like `const sum = 2 + 5` is totally fine because, with the `+` operator, we're asking JavaScript the answer to the math question `2 + 5`, which'll return `7`. 

And to play the devils advocate here, we must talk about the difference between function declarations and function expressions (arrow functions are also function expressions).

When we declare a function, something magic happens with the JavaScript interpreter. Thanks to the `function` keyword, it's able to read it through and also *hoist* it and allow us to use it **anywhere** in our program.

But function expressions, since they **are** expressions, just **return** the function as a value of such an expression. 

I get that this can be confusing, but you have to take this leap of faith because it's how JavaScript works internally.
```js
// Wrong!!!
const declaration = function declaration(){ return "Not working. Not expression" }

// Right
const expression = function() { 
	return "Totally fine. Expression returns the function and assigns it to expression name"
}
```

Here I've used a standard *function expression* and not an *arrow function* as we use in modern JavaScript. If you're curious, beside syntax, the main difference between the two is how `this` is assigned internally. 

Inside a *function expression*, `this` is dynamic and determined by **how** the function is called, while in an *arrow function*, `this` is lexical and gets inherited by its surrounding scope.
# 4. Studying from the Inside
We're building the foundation of our JavaScript mental model, where values are all we need. Values belongs to a type, primitive values are immutable and we can point to values using *"wires"* that we call variables.
# 5. Meeting the Primitive Values
## Undefined
This is the first primitive value we want to understand. `undefined` does not mean *"nothing"*, it is a value. A value that often a variable points to when we still have to wire it to a different value.
```js
let var;
console.log(var); // "undefined" 
```
Often, we refer to this piece of code as *"this variable is not defined yet,"* but it is wrong. This variable **is defined**; it just points to the `undefined` value.

That's how JavaScript works. Just try to `console.log` a variable that's not already defined with the `let` keyword (that it does not hoist it). You will get a `ReferenceError` because inside our JavaScript universe such variable name is, for real, **not defined yet**.
```js
console.log(var); // ReferenceError
let var = undefined;
```
## Null
Beside an historical bug that gives us the string *"object"* when we try to `console.log(null)`, `null` is just a primitive value!

Instead of `undefined`, `null` is used to tell that we're intentionally missing a value.

While `undefined` and `null` looks similar, both primitives define a missing value, their use is slightly different. The first usually present a coding mistake, a wire that does not point (yet) to nothing, while the second is better suited to inform **missing data**.
## Boolean
This is the first, and only, primitive value that define the on and off statuses. Like in a transistor that lives inside our CPU.

Besides being a value itself, this is the only primitive value that helps our programs to **define logic**. Does not matter what you write for your `if`, `while`, and even `for` conditions. The computer will always **translate your expressions down to a `boolean`**.

Check this example:
```js
const isSad = true;
const isHappy = !isSad;
const isFeeling = isSad || isHappy; // At least one is true?
const isConfusing = isSad && isHappy; // Are both true?
```
Remember that every wire, every variable, just points to a primitive value, and it does not matter if later on `isSad` points to `false` or a different primitive. At the time of the evaluation, `isHappy` gets pointed to the opposite value of `isSad` thanks to our expression that uses the `!`.
## Numbers
Until now, we have met four primitive values: `undefined`, `null`, `true`, and `false`. But get ready, because with numbers we introduce **18 quintillion** new values that we can use in our programs.

And now you're maybe thinking: *"Wait a sec, in math we have infinite numbers! Is the math broken in JavaScript.*

This is generally a misconception. The math in JavaScript is not broken, it just uses a different math. Something called *floating-point math* and that's implemented because our computers are not able to understand infinity, many humans have troubles understanding it too so why do we expect that computers (built from humans with more limits than its creators) would behave differently.

Jokes aside, generally speaking *floating-point math* is pretty accurate for our day to day calculations and it's a way for the computer to just pick the **closest number** to the one we actually use.

That's why math run with integers is generally more precise that when we use floating numbers.
```js
console.log(10 + 20 === 30); // true
console.log(0.1 + 0.2 === 0.3); // false! It actually is 0.30000000000000004
```
But let's not fall for it. Generally speaking the math we need in our programs is pretty basic and we can present a human readable one with `toFixed` or one of the others utility functions that JavaScript has built-in. If you will ever need more precision, you'll learn how to reach for it 😉

### Special Numbers
I need to open a little chapter on special numbers here, because ocasionally you will encounter one of these: `NaN`, `Infinity`, `-Infinity`, and `-0`.

Yes I know, I just told you that computers does not understand infinity. And in fact they not exists in the sense that JavaScript is able to work with them, these special numbers exists because the JavaScript engine has to represent their values anyway.
```js
let scale = 0;
let a = 1 / scale; // Infinity
let b = 0 / scale; // NaN
let c = -a; // -Infinity
let d = 1 / c; // -0
```
The more interesting one of all these is `NaN`, the result of `0/0`, and you know why? Because even though it means *Not A Number* if you try to `console.log` its type you get `number`!
```js
console.log(typeof(NaN)); // "number"
```
And this is an issue with naming things. In JavaScript the primitive value of `NaN` is indeed a `number`. Is not a `null`, an `undefined`, a `boolean`, a `string` or any other primitive. It is just a number.

But in floating-point math the name for this invalid result is *not a number*.
## BigInts
I want to skip this part because we could write a book about it. The important thing to know is that they, recently, exist and they're pretty useful for financial programs where precision is critical.

But, also because they're new, you'll not see them much around and I let you deep dive in case you need to work with them.

We have another commonly used primitive value that I want to focus on for the next section.
## Strings
The way JavaScript uses to represent text. Generally speaking you can write string in three different ways:
- **single quotes:** `'this is a string'` 
- **double quotes:** `"this is a string"`
- **backticks:** `` `this is a string too` `` sometimes referred to template literals (or strings), quite more powerful than the previous ones 

As we saw in a previous lesson **strings aren't objects**. They do have built-in properties. like the ability to know its length by chaining `.length` or access a specific character with brakets `[1]`. But this does not mean that hey are objects (or arrays).

To keep working on our mental model, the ones that helps us study JavaScript from the Inside, every string that you can think of **already exists** in our JavaScript universe.

Even `"adlfkjsdflkjasdlfk lkdfjlaksdf asdlfkjasdfkl"`. 

I haven't *created it*, my wire just points to it.
## Symbols
This is another primitive value that we will skip, at least for now, because it requires us to understand deeply objects and properties in JavaScript. For now just understand that Symbols allow you to hide away some information inside an object and control which parts of the code can access it.
# 6. Meeting Objects and Functions
## Objects
Generally, we say that everything in JavaScript is an object, and besides primitive values and functions, that's true. 

We have already seen that primitive values exist in the JavaScript universe. Whenever a wire points to them or we display them in the console or inside our applications, we just summon them. We do not create them.

This is different for objects, though.

We have the ability to create our own values with objects.
```js
const one = {};
const two = {};
```

In this example, `one` and `two` are wires that point to **two distinct objects**. This is not like primitive values, where we do not point to the same value. We create two separate values. In this case, both are empty objects, but they are not the same object.

Since we're talking about creation, it makes sense to also talk about destruction.

Can we destroy an object? We, as programmers, are not able to destroy an object directly. We can stop referring to it, but then it is up to the JavaScript engine to chose when it's the case to destroy one with its garbage collection abilities. 
## Functions
It can sound strange, but functions are values too. They're just a special type of values that can return a value if they get called.

This does not happen with any other value.

So remember, functions are the only kind of value that can represent two different values:
- first we have the value that represent the function itself
- second we can call the function and have the value that its able to return
# 7. Equality of Values
In JavaScript equality is commonly something that people jokes about, and for good reason! We have multiple ways to check if a value is equal to another:
- `a === b` called **Strict Equality**
- `a == b` called **Loose Equality**, this is the source of many jokes!
- `Object.is(a, b)` less common and called **Same Value Equality**
## Same Value Equality
Although this method is less commonly used by many programmers, it is as safe as our commonly used **Strict Equality**. Despite having `Object` in the method name, it does not involve objects.

This method compares values!
## Comparing Same Value Equality to Strict Equality
`Object.is` and `===` generally do the same thing, except for two special cases. Funny enough, both cases have to do with [[#Special Numbers]].
### `NaN`
We encounter this special number previously and we understood that it's a value that JavaScript uses when we do invalid math like `0/0`.
```js
console.log(0/0); // NaN
```
But if we can see a difference based on the type of equality we execute in our code:
```js
console.log(NaN === NaN); // false
console.log(Object.is(NaN, NaN)); // true
```
This is for historical reasons, so we just have to deal with it and take it for granted.
### `-0`
In regular math, we do not have the concept of negative `0`; it is just `0`. But because our computers use floating-point math we have that, and generally speaking its used to display a negative value that too little to represent.
```js
console.log(-0 === 0); // true
console.log(Object.is(-0, 0)); // false 
```

The thing is that in both exceptions, `Object.is` is the method that really respects our mental models! Both special numbers are **primitive values**, and as such `NaN` will always be the same if we compare to itself while `-0` and `0` are different values.
## Loose equality
This is probably one of the most joked parts of JavaScript, and it has been recognized to be one of the early bad design decisions of the language.

Since we will not find it commonly, many teams even and code standards prohibit its use altogether, we will not cover it much here.

There's only one, even though confusing, use case that can make our code shorter:
```js
if (x == null){
	// ...
}
```
This is the same as write the following:
```js
if (x === null || x === undefined){
	// ...
}
```
But as I told you, is not commonly used and you could even confuse your team!
# 8. Properties
Just like variables, properties are wires and wires points to **values**. The difference between the two is that properties **always** start from objects.

We can access the properties of an object with the dot notation (`.`) or brackets (`[]`).

Objects cannot have two properties with the same name.

Let's check the following example:
```js
const person = { name: "Andrea" };

console.log(person.name); // ?
console.log(person.age); // ?
console.log(person.age.inDays); // ?
```
Are you able to figure out what will print each `console.log`?

Let's refresh some knowledge here. We saw that every time we write `{}` in JavaScript **we create a new object**. In this case our `person` wire points to a new object `{ name: "Andrea" }`.

This new object has only one property: `name`.

When we ask JavaScript the question *"Which is the value of `name` inside the object wired to `person`?*, and we do this with the expression `person.name`, JavaScript follows the wire that's connection `person` to the object and looks at the property `name`.

If `name` exists, it'll answer with the value that the property points at. In our case, the string value is `"Andrea."`

So, we solved the first case.

What happens when we ask JavaScript for a property that does not exist?

Well, since JavaScript **always returns values from expressions** (or an error), it checks the object that `person` points at and sees that there is no `age` property, so it just returns `undefined`.

Now we know that `person.age` points to a primitive value. It points at `undefined`.

Since `undefined` does not have any properties inside it, it is not an object after all. Therefore, it will give us a `TypeError` when we ask for the property `inDays` that we think will be inside `person.` age`.
# 9. Mutation
Mutations, that's its basically an alias for *change*, could be tricky to understand based on our mental model.

For example, the following code may surprise you:
```js
const sherlock = {
	surname: 'Holmes',
	address: { city: 'London' }
}

const john = {
	surname: 'Watson',
	address: sherlock.address
}

john.surname = 'Lennon';
john.address.city = 'Maliby';

console.log(john.address.city); // 'Malibu'
console.log(sherlock.address.city); // 'Malibu' 😮
```
What's happening here? Why are my changes (*mutations *) to `john.address.city` also reflected in `sherlock.address.city`?

Remember what we talked a bit earlier about `{}`?

Every time the JavaScript engine sees a pair of curly brackets, it **creates a new object**. In our case, the new object for `sherlock.address` that has a property `city` that points to the string value of *`London`* is **the same object** that `john.address` points at.

We're not nesting the `address` object inside `sherlock` or `john`. In JavaScript **, there are no nested objects.** An object only has properties that **point at** a value. In our example, the value that `address` points at is **the same object** of `{city: 'London'}`.

Now it should be simpler to understand that when we change the value that `city` is pointing at, we change it for all the variables that point to the same object. 

With `john.address.city` we're not only changing the value pointed from `address.city` that lives inside the `john` wire. Noting *lives* inside it, at least `john` object has properties that point to different values.

`address` points to the same object that `shelock.address` and `john.address` point at. So when we change the wired value for `city`, we **mutate it for both**.

I know that this part can look confusing, but I promise that the more you practice this mental model, the more you'll just *get it*.

### `const` vs `let`
I wanted to have a little discussion about these keywords. I purposely used `const` instead of `let` to simplify this.

As you can see, `const` does not make impossible to mutate the object that points at.

That's because `const` makes it impossible to reassign the variable to something else. It does not allow our wires to change the primitive value it points at.
```js
const john = {surname: 'Watson'};
john = 'Pippo'; // TypeError
john.surname = 'Lennon'; // Totally fine!
```
# 10. Prototypes
Prototypes in JavaScript are pretty powerful yet confusing. They've been created to give *hineritance* to our code. 

Check this snippet:
```js
const obj = {};
console.log(obj.name); // ??
```
What will print our `console.log`? The correct answer is *it depends*.

Yeah, I know, programming tutorials are full of *depends* and in part this is why being a developer is not so easy after all. But it is also what makes programming fun and challenging!

Each object in JavaScript has a built-in `__proto__` property that points to the prototype of such object. When you create a blank object like our `obj`, JavaScript populates its `__proto__` with the Object Prototype by default.

This is the reason why each object can access to methods like `hasOwnProperties`, `toString`,  `valueOf` and so on.

All of these live inside the [Object Prototype](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes).

Now that we are aware of the presence of a prototype for our object, prototype that you can inspect via the `__proto__` keyword, the answer *depends* should be easier to understand.

While we know that the property of an object that does not point to a value should return `undefined`, we cannot be 100% sure that it'll be the result of our `console.log`.

That's because we, or another developer working on the same project, could've written something like the following in our program (in **any** part of our program), and the result of our `console.log` would've been different.
```js
Object.prototype.name = 'Sally';

console.log(obj.name); // 'Sally'
```
With the like `Object.prototype.name` we (or one of our colleagues) added the `name` property pointing at the `'Sally'` string value to **every single object**.

I can understand that this feels confusing, but it is how JavaScript works and why it is so powerful.

Basically speaking, every time we ask JavaScript the value that a property points at with our expressions, JavaScript does not only check at the properties defined in our object. It also checks **all the properties** present in the prototype chain.

I've just talked about chain here because an object can have a `__proto__` that points to another object, that has `__proto__` as well that points to something else.

Confusing right? Let's check with this example.
```js
const animal = {
	hasBrain: true,
}

const human = {
	__proto__: animal,
	legs: 2,
}

const student = {
	__proto__: human,
	name: 'Sally',
}

console.log(student.hasBrain); // true
```
I hope this example clarifies the chain concept.

Since our `student` has a `__proto__` property but has no `hasBrain` one. JavaScript will check inside the value that `student.__proto__` points at, looking for `hasBrain`.

In our example, `human` has no property `hasBrain`, but has another `__proto__`.

So JavaScript does not stop at `human`, and follows the `__proto__` wire looking for a `hasBrain` property. Luckily, once it gets to `animal`, it finds a `hasBrain` property that points at `true`. Then it'll return such value to our `console.log`. 

### What's the difference between `__proto__` and `prototype`?
First and foremost, `__proto__` is just an accepted way to check which properties came from our prototype chain. The standard way to access it in JavaScript is by using the method `getPrototypeOf()`.

But besides that, if you ever read a definition for an array method on MDN, let's take `filter` for example, you'll notice something curious. All of these methods are defined as `Array.prototype.<methodName>`, in our example is `Array.prototype.filter`.

In an attempt to simplify this concept, we can just say that:
- `__proto__` is used to directly **access** the values of the prototype (and also set them if we use object literal syntax)
- `prototype` is the standard way to **extend** the function that returns our object

Few years ago, object literarls weren't used that much, so to create an object we usually wrote something like:
```js
function Animal() {
	return { hasBrain: true }
}

var dog = Animal();
var cat = Animal();
```
But this has its limits.

What if we wanted to add an `eat()` method to all `Animal`s? Only knowing about `__proto__` would force us to write something like:
```js
function Animal() {
	return { hasBrain: true }
}

var animalEating = {
	eat() {
		console.log('Gnam gnam');
	}
}

var dog = Animal();
dog.__proto__ = animalEating; 
var cat = Animal();
cat.__proto__ = animalEating;

dog.eat();
cat.eat();
```
But this is not DRY at all!

That's why we have the `new` keyword that allow us to:
- create the object automatically making it available with `this`
- the `__proto__` object will be set from whatever we put inside `prototype`
```js
function Animal() {
	this.hasBrain = true
}
Animal.prototype = {
	eat() {
		console.log('Gnam gnam');
	}
}

let dog = new Animal(); // __proto__: Animal.prototype
let cat = new Animal(); // __proto__: Animal.prototype

dog.eat();
cat.eat();
```
On a side note, the `class` syntax that we have in JavaScript since ES6 is just syntactic sugar that hides `prototype` from us.