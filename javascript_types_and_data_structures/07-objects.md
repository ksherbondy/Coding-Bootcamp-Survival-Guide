# JavaScript Objects: From Object Literals to Prototypes

> A beginner-friendly guide to creating objects, accessing data, using `this`, constructor functions, prototypes, classes, built-in object methods, and choosing the right object pattern.

---

## Learning Objectives

By the end of this lesson, you should be able to:

- explain what a JavaScript object is
- create objects in several different ways
- add, read, update, and delete properties
- use dot notation and bracket notation
- explain when bracket notation is required
- write methods inside objects
- explain what `this` refers to in common object patterns
- create multiple objects with constructor functions
- explain the difference between instance properties and prototype methods
- explain why shared methods are often placed on a prototype
- inspect an object's prototype
- distinguish plain objects, arrays, functions, Maps, Sets, and class instances
- use common static methods from `Object`
- use inherited methods from `Object.prototype`
- recognize when a plain object is acting as a lookup table
- avoid common beginner mistakes involving objects

---

# 1. What Is an Object?

An object is a collection of properties.

Each property is essentially:

```text
key → value
```

Example:

```js
const dog = {
  name: "Snoopy",
  breed: "Beagle",
  age: 4
};
```

Conceptually:

```text
dog
├── name  → "Snoopy"
├── breed → "Beagle"
└── age   → 4
```

An object's values can be almost anything:

```text
string
number
boolean
array
another object
function
null
other JavaScript values
```

Example:

```js
const user = {
  name: "Ada",
  active: true,
  scores: [98, 91, 100],
  address: {
    city: "London"
  },
  greet: function() {
    return "Hello";
  }
};
```

Objects are useful when several values belong together as one conceptual thing.

---

# 2. Object Literal Syntax

The most common way to create an object is an **object literal**:

```js
const dog = {
  name: "Snoopy",
  breed: "Beagle"
};
```

The braces:

```js
{
}
```

create the object.

Inside the object:

```js
name: "Snoopy"
```

means:

```text
property key: name
property value: "Snoopy"
```

Multiple properties are separated with commas.

---

# 3. Reading Object Properties

There are two main forms.

## Dot Notation

```js
console.log(dog.name);
```

Output:

```text
Snoopy
```

Use dot notation when you know the property name while writing the code.

## Bracket Notation

```js
console.log(dog["name"]);
```

This also returns:

```text
Snoopy
```

Bracket notation becomes especially useful when the key comes from a variable.

```js
const key = "breed";

console.log(dog[key]);
```

Output:

```text
Beagle
```

This:

```js
dog[key]
```

means:

> Evaluate `key`, then use its value as the property name.

If:

```js
key = "breed";
```

then:

```js
dog[key]
```

is effectively:

```js
dog["breed"]
```

---

# 4. Dot Notation vs Bracket Notation

A useful rule:

```text
I know the property name while writing the code
→ object.property

the property name is dynamic or stored in a variable
→ object[key]
```

Example:

```js
const student = {
  name: "Sam",
  grade: 92
};

const requestedField = "grade";

console.log(student[requestedField]);
```

This cannot be replaced with:

```js
student.requestedField
```

because that would look for a property literally named:

```text
requestedField
```

rather than:

```text
grade
```

---

# 5. Bracket Notation Enables Lookup Tables

Consider:

```js
function getPlanetName(id) {
  return {
    1: "Mercury",
    2: "Venus",
    3: "Earth",
    4: "Mars",
    5: "Jupiter",
    6: "Saturn",
    7: "Uranus",
    8: "Neptune"
  }[id];
}
```

The object is created and immediately accessed using:

```js
[id]
```

If:

```js
id = 3;
```

JavaScript effectively performs:

```js
{
  1: "Mercury",
  2: "Venus",
  3: "Earth"
}[3]
```

and returns:

```text
Earth
```

This is **computed property access**.

It is especially useful when the problem is really:

```text
key → associated value
```

rather than:

```text
input → completely different behavior
```

---

# 6. Missing Properties Return `undefined`

If a key does not exist:

```js
const planets = {
  1: "Mercury",
  2: "Venus"
};

console.log(planets[10]);
```

Output:

```js
undefined
```

You can provide a fallback:

```js
return planets[id] ?? "Invalid planet ID";
```

The nullish coalescing operator:

```js
??
```

uses the right-hand value when the left-hand side is:

```text
null
undefined
```

---

# 7. Adding Properties

```js
const dog = {
  name: "Snoopy"
};

dog.breed = "Beagle";
```

Now the object contains:

```js
{
  name: "Snoopy",
  breed: "Beagle"
}
```

You can also use bracket notation:

```js
dog["age"] = 4;
```

Or a dynamic key:

```js
const property = "favoriteFood";

dog[property] = "pizza";
```

---

# 8. Updating Properties

```js
dog.age = 5;
```

If the property already exists, assignment replaces its value.

---

# 9. Deleting Properties

```js
delete dog.age;
```

Afterward:

```js
dog.age
```

returns:

```js
undefined
```

Use `delete` deliberately. Frequently changing object shapes can also be less friendly to JavaScript engine optimization than keeping relatively stable shapes.

---

# 10. `const` Objects Can Still Change

This often confuses beginners:

```js
const dog = {
  name: "Snoopy"
};

dog.name = "Scooby";
```

This is valid.

`const` prevents rebinding the variable:

```js
dog = {};
```

That is not allowed.

But `const` does not automatically make the object immutable.

Think:

```text
const protects the binding

not necessarily the object behind the binding
```

---

# 11. Methods: Functions Stored on Objects

A function stored as an object property is commonly called a **method**.

```js
const dog = {
  name: "Snoopy",

  bark: function() {
    return "Woof";
  }
};
```

Call it:

```js
dog.bark();
```

Modern shorthand syntax:

```js
const dog = {
  name: "Snoopy",

  bark() {
    return "Woof";
  }
};
```

These express the same basic idea.

---

# 12. Using `this`

Suppose every dog should introduce itself.

```js
const dog = {
  name: "Snoopy",

  introduce() {
    return `My name is ${this.name}`;
  }
};
```

When called as:

```js
dog.introduce();
```

`this` refers to the object through which the method was called.

So:

```js
this.name
```

finds:

```js
dog.name
```

---

# 13. `this` Is Determined by How a Function Is Called

A useful beginner rule is:

> For ordinary functions used as methods, look to the left of the dot at call time.

```js
dog.bark();
```

Here:

```text
this → dog
```

However, `this` has more rules than this one shortcut covers.

Important contexts include:

```text
method calls
constructor calls with new
call/apply/bind
standalone function calls
arrow functions
```

For beginners, understand the method and constructor cases first.

---

# 14. Arrow Functions and `this`

Arrow functions do **not** create their own `this`.

This can make them a poor choice for object methods that expect `this` to refer to the object.

Example:

```js
const dog = {
  name: "Snoopy",

  bark: () => {
    return this.name;
  }
};
```

Do not assume:

```js
this === dog
```

inside that arrow function.

For methods that rely on object-style `this`, prefer:

```js
bark() {
  return this.name;
}
```

or:

```js
bark: function() {
  return this.name;
}
```

---

# 15. Creating Objects With `new Object()`

You can also write:

```js
const dog = new Object();

dog.name = "Snoopy";
dog.breed = "Beagle";
```

This works, but object literal syntax is usually clearer:

```js
const dog = {
  name: "Snoopy",
  breed: "Beagle"
};
```

So:

```text
{}
```

is generally preferred for ordinary plain objects.

---

# 16. Why We Need Repeatable Object-Creation Patterns

Writing one dog manually is easy:

```js
const snoopy = {
  breed: "Beagle"
};
```

But what if we need:

```text
100 dogs
1,000 users
10,000 game entities
```

We need a repeatable creation pattern.

One traditional JavaScript solution is a **constructor function**.

---

# 17. Constructor Functions

Constructor functions are ordinary functions intended to be called with:

```js
new
```

Example:

```js
function Dog(breed) {
  this.breed = breed;
}
```

Create instances:

```js
const snoopy = new Dog("Beagle");
const scooby = new Dog("Great Dane");
```

Now:

```js
console.log(snoopy.breed);
console.log(scooby.breed);
```

returns:

```text
Beagle
Great Dane
```

By convention, constructor function names begin with a capital letter:

```js
Dog
Person
Car
User
```

This is a convention, not special syntax.

---

# 18. What `new` Conceptually Does

When you write:

```js
const snoopy = new Dog("Beagle");
```

a useful mental model is that JavaScript approximately does this:

```text
1. Create a new empty object.

2. Connect that object's prototype to Dog.prototype.

3. Call Dog with `this` referring to the new object.

4. Execute:
   this.breed = "Beagle";

5. Return the newly created object
   unless the constructor explicitly returns another object.
```

You do not manually perform these steps; `new` does them for you.

---

# 19. Putting a Method Inside the Constructor

You can write:

```js
function Dog(breed) {
  this.breed = breed;

  this.bark = function() {
    return "Woof";
  };
}
```

Then:

```js
const snoopy = new Dog("Beagle");
const scooby = new Dog("Great Dane");

snoopy.bark();
scooby.bark();
```

Both dogs can bark.

But each constructor call created a new function object.

Conceptually:

```text
snoopy
├── breed → "Beagle"
└── bark  → function A

scooby
├── breed → "Great Dane"
└── bark  → function B
```

You can prove it:

```js
console.log(snoopy.bark === scooby.bark);
```

Output:

```js
false
```

---

# 20. Prototype Methods

Instead, define the shared method once:

```js
function Dog(breed) {
  this.breed = breed;
}

Dog.prototype.bark = function() {
  return "Woof";
};
```

Create dogs:

```js
const snoopy = new Dog("Beagle");
const scooby = new Dog("Great Dane");
```

Both still work:

```js
snoopy.bark();
scooby.bark();
```

But now:

```js
console.log(snoopy.bark === scooby.bark);
```

returns:

```js
true
```

They access the same shared function.

---

# 21. Access vs Ownership

This is the key distinction.

Constructor version:

```js
this.bark = function() {};
```

means:

```text
every instance OWNS its own bark function
```

Prototype version:

```js
Dog.prototype.bark = function() {};
```

means:

```text
every instance can ACCESS one shared bark function
```

Conceptually:

```text
snoopy
├── breed
└── [[Prototype]] ──┐
                    │
                    ├── bark → one shared function
                    │
scooby              │
├── breed            │
└── [[Prototype]] ──┘
```

For two dogs, the memory difference is trivial.

For a very large number of instances, repeatedly creating identical functions is unnecessary.

---

# 22. Why Put Data on the Instance but Methods on the Prototype?

Instance-specific data:

```js
this.breed = breed;
this.name = name;
this.age = age;
```

Each dog may have different values.

Shared behavior:

```js
Dog.prototype.bark = function() {};
Dog.prototype.run = function() {};
Dog.prototype.sleep = function() {};
```

The behavior is conceptually the same for every dog.

A useful traditional pattern:

```text
instance
→ unique state/data

prototype
→ shared behavior
```

This is not an absolute rule, but it is a strong design pattern.

---

# 23. How Prototype Lookup Works

Suppose:

```js
snoopy.bark();
```

JavaScript first looks for:

```text
bark
```

directly on `snoopy`.

If it does not find it, JavaScript follows the prototype link.

Conceptually:

```text
snoopy.bark
    ↓
not found on snoopy
    ↓
look at Dog.prototype
    ↓
found bark
```

This is **prototype-chain lookup**.

---

# 24. Shadowing a Prototype Property

Suppose:

```js
Dog.prototype.sound = "Woof";
```

Then:

```js
console.log(snoopy.sound);
```

finds the prototype property.

But if we do:

```js
snoopy.sound = "Quiet woof";
```

then:

```js
console.log(snoopy.sound);
```

returns:

```text
Quiet woof
```

The instance property is found before JavaScript reaches the prototype.

The prototype value still exists; it is just shadowed for that instance.

---

# 25. Checking Whether a Property Belongs Directly to an Object

Use:

```js
Object.hasOwn(object, property)
```

Example:

```js
Object.hasOwn(snoopy, "breed");
```

returns:

```js
true
```

because `breed` belongs directly to `snoopy`.

But:

```js
Object.hasOwn(snoopy, "bark");
```

returns:

```js
false
```

when `bark` lives on `Dog.prototype`.

---

# 26. Inspecting the Prototype

Use:

```js
Object.getPrototypeOf(snoopy);
```

For an instance made with:

```js
new Dog()
```

this should be:

```js
Dog.prototype
```

Test:

```js
console.log(
  Object.getPrototypeOf(snoopy) === Dog.prototype
);
```

Output:

```js
true
```

---

# 27. Creating an Object With a Specific Prototype

You can create an object directly from another object:

```js
const dogMethods = {
  bark() {
    return "Woof";
  }
};

const snoopy = Object.create(dogMethods);

snoopy.breed = "Beagle";
```

Now:

```js
snoopy.bark();
```

works through the prototype chain.

---

# 28. `Object.create(null)`

You can create an object with **no prototype**:

```js
const dictionary = Object.create(null);
```

This object does not inherit from:

```js
Object.prototype
```

That means:

```js
dictionary.toString
```

does not automatically exist.

This can be useful for specialized dictionary-like objects, though `Map` is often a better modern choice when the primary purpose is dynamic key/value storage.

---

# 29. ES6 Classes

Modern JavaScript also provides `class` syntax:

```js
class Dog {
  constructor(breed) {
    this.breed = breed;
  }

  bark() {
    return "Woof";
  }
}
```

Create instances:

```js
const snoopy = new Dog("Beagle");
const scooby = new Dog("Great Dane");
```

Classes may look like a different object system, but JavaScript classes are built on the prototype system.

The method:

```js
bark() {
  return "Woof";
}
```

is placed on:

```js
Dog.prototype
```

not copied separately into every instance.

Test:

```js
console.log(snoopy.bark === scooby.bark);
```

Result:

```js
true
```

---

# 30. Constructor Function vs Class

Traditional:

```js
function Dog(breed) {
  this.breed = breed;
}

Dog.prototype.bark = function() {
  return "Woof";
};
```

Class syntax:

```js
class Dog {
  constructor(breed) {
    this.breed = breed;
  }

  bark() {
    return "Woof";
  }
}
```

They are not identical in every semantic detail, but both rely on JavaScript's prototype-based inheritance model.

Understanding prototypes still matters because prototypes are the underlying mechanism.

---

# 31. Factory Functions

You do not need constructors or classes to create multiple objects.

A factory function simply returns an object.

```js
function createDog(name, breed) {
  return {
    name,
    breed,

    bark() {
      return "Woof";
    }
  };
}
```

Usage:

```js
const snoopy = createDog("Snoopy", "Beagle");
```

This is straightforward and avoids `new`.

In this exact form, however, each call creates another `bark` function.

Factories can also be designed to share behavior or use closures depending on the application.

---

# 32. Object Property Shorthand

Instead of:

```js
const name = "Snoopy";
const breed = "Beagle";

const dog = {
  name: name,
  breed: breed
};
```

you can write:

```js
const dog = {
  name,
  breed
};
```

When the variable name and desired property name match, JavaScript provides shorthand syntax.

---

# 33. Computed Property Names During Object Creation

Bracket syntax can also be used while defining an object.

```js
const key = "favoriteFood";

const dog = {
  name: "Snoopy",
  [key]: "Pizza"
};
```

Result:

```js
{
  name: "Snoopy",
  favoriteFood: "Pizza"
}
```

This is different from:

```js
key: "Pizza"
```

which would literally create a property named:

```text
key
```

---

# 34. Object Destructuring

Given:

```js
const dog = {
  name: "Snoopy",
  breed: "Beagle",
  age: 4
};
```

instead of:

```js
const name = dog.name;
const breed = dog.breed;
```

you can write:

```js
const { name, breed } = dog;
```

---

# 35. Renaming During Destructuring

```js
const { name: dogName } = dog;
```

Now:

```js
dogName
```

contains:

```text
Snoopy
```

---

# 36. Default Values During Destructuring

```js
const { age = 0 } = dog;
```

If `age` is `undefined`, the default is used.

---

# 37. Spread Syntax With Objects

Copy properties into a new object:

```js
const dog = {
  name: "Snoopy",
  breed: "Beagle"
};

const copy = {
  ...dog
};
```

Add or override:

```js
const olderDog = {
  ...dog,
  age: 5
};
```

Override existing property:

```js
const renamed = {
  ...dog,
  name: "Scooby"
};
```

Later properties win.

---

# 38. Spread Creates a Shallow Copy

Suppose:

```js
const user = {
  name: "Ada",
  address: {
    city: "London"
  }
};

const copy = { ...user };
```

`copy` is a new outer object.

But:

```js
copy.address === user.address
```

is:

```js
true
```

The nested object reference is still shared.

This is a **shallow copy**.

---

# 39. `Object.assign()`

Another way to copy or merge enumerable own properties:

```js
const result = Object.assign({}, dog);
```

Modern code often prefers spread syntax for simple cases because it is easier to read.

---

# 40. Objects as Frequency Counters

Objects are often useful in coding challenges.

```js
const colors = ["red", "blue", "red", "green", "red"];

const counts = {};

for (const color of colors) {
  counts[color] = (counts[color] || 0) + 1;
}
```

Result:

```js
{
  red: 3,
  blue: 1,
  green: 1
}
```

Notice:

```js
counts[color]
```

is bracket notation with a dynamic key.

If:

```js
color === "red"
```

then:

```js
counts[color]
```

means:

```js
counts["red"]
```

---

# 41. Objects vs `Map`

A plain object can act like a key/value table:

```js
const users = {
  alice: 10,
  bob: 20
};
```

JavaScript also provides:

```js
Map
```

Example:

```js
const users = new Map();

users.set("alice", 10);
users.set("bob", 20);

console.log(users.get("alice"));
```

Use a plain object when modeling a record or structured entity:

```js
const user = {
  name: "Ada",
  age: 36
};
```

Consider `Map` when the primary purpose is dynamic key/value storage and lookup.

`Map` also allows keys of any value type.

---

# 42. Objects vs Arrays

Arrays are objects too:

```js
typeof [];
```

returns:

```text
"object"
```

But arrays are specialized for ordered indexed collections.

Use:

```js
[]
```

when order/index position is central.

Use:

```js
{}
```

when named properties describe the data.

Compare:

```js
const point = [10, 20];
```

with:

```js
const point = {
  x: 10,
  y: 20
};
```

The object carries more semantic meaning.

---

# 43. Functions Are Objects Too

Functions can have properties:

```js
function greet() {
  return "Hello";
}

greet.language = "English";

console.log(greet.language);
```

JavaScript functions are callable objects.

This is part of why constructor functions can have:

```js
Dog.prototype
```

as a property.

---

# 44. Other Important Built-In Object Types

JavaScript has many specialized objects, including:

```text
Object
Array
Function
Date
RegExp
Map
Set
WeakMap
WeakSet
Promise
Error
ArrayBuffer
typed arrays
```

Not every object should be represented with `{}`.

Choose a structure based on what behavior the data needs.

---

# 45. Static Methods on `Object`

These are called like:

```js
Object.keys(dog);
```

not:

```js
dog.keys();
```

Important standard methods include:

```text
Object.assign()
Object.create()
Object.defineProperties()
Object.defineProperty()
Object.entries()
Object.freeze()
Object.fromEntries()
Object.getOwnPropertyDescriptor()
Object.getOwnPropertyDescriptors()
Object.getOwnPropertyNames()
Object.getOwnPropertySymbols()
Object.getPrototypeOf()
Object.groupBy()
Object.hasOwn()
Object.is()
Object.isExtensible()
Object.isFrozen()
Object.isSealed()
Object.keys()
Object.preventExtensions()
Object.seal()
Object.setPrototypeOf()
Object.values()
```

Some environments may not support the newest additions if they use an older JavaScript runtime.

---

# 46. `Object.keys()`

Returns an array of an object's own enumerable string keys.

```js
const dog = {
  name: "Snoopy",
  breed: "Beagle"
};

console.log(Object.keys(dog));
```

Output:

```js
["name", "breed"]
```

---

# 47. `Object.values()`

```js
Object.values(dog);
```

Result:

```js
["Snoopy", "Beagle"]
```

---

# 48. `Object.entries()`

Returns key/value pairs.

```js
Object.entries(dog);
```

Result:

```js
[
  ["name", "Snoopy"],
  ["breed", "Beagle"]
]
```

This pairs beautifully with destructuring:

```js
for (const [key, value] of Object.entries(dog)) {
  console.log(key, value);
}
```

---

# 49. `Object.fromEntries()`

Converts key/value pairs into an object.

```js
const entries = [
  ["name", "Snoopy"],
  ["breed", "Beagle"]
];

const dog = Object.fromEntries(entries);
```

---

# 50. `Object.hasOwn()`

Checks whether an object directly owns a property.

```js
Object.hasOwn(dog, "name");
```

Result:

```js
true
```

It does not count inherited prototype properties.

---

# 51. `Object.create()`

Creates a new object with a specified prototype.

```js
const animal = {
  eat() {
    return "Eating";
  }
};

const dog = Object.create(animal);
```

Now:

```js
dog.eat();
```

works through the prototype chain.

---

# 52. `Object.getPrototypeOf()`

Returns an object's prototype.

```js
Object.getPrototypeOf(snoopy);
```

---

# 53. `Object.setPrototypeOf()`

Changes an object's prototype.

```js
Object.setPrototypeOf(object, prototype);
```

This exists, but changing prototypes after object creation is generally discouraged for performance and design reasons.

Prefer creating the object with the intended prototype from the beginning.

---

# 54. `Object.freeze()`

Prevents ordinary addition, deletion, and reassignment of own properties.

```js
const dog = Object.freeze({
  name: "Snoopy"
});
```

Then:

```js
dog.name = "Scooby";
```

does not successfully replace the frozen property.

Important:

> `Object.freeze()` is shallow.

Nested objects are not automatically deeply frozen.

---

# 55. `Object.seal()`

Prevents adding or deleting properties, while existing writable properties may still be changed.

```js
const dog = Object.seal({
  name: "Snoopy"
});

dog.name = "Scooby";
```

That update is allowed.

But:

```js
delete dog.name;
```

is not.

---

# 56. `Object.preventExtensions()`

Prevents adding new properties.

```js
Object.preventExtensions(dog);
```

Existing properties may still be modified or deleted depending on their descriptors.

---

# 57. Checking Object Restrictions

```js
Object.isFrozen(object);
Object.isSealed(object);
Object.isExtensible(object);
```

---

# 58. `Object.is()`

Compares two values with semantics slightly different from `===`.

```js
Object.is(NaN, NaN);
```

returns:

```js
true
```

while:

```js
NaN === NaN
```

returns:

```js
false
```

Another difference:

```js
Object.is(0, -0);
```

returns:

```js
false
```

while:

```js
0 === -0
```

returns:

```js
true
```

---

# 59. Property Descriptors

Properties have metadata beyond key and value.

Common descriptor fields include:

```text
value
writable
enumerable
configurable
get
set
```

Inspect one:

```js
Object.getOwnPropertyDescriptor(dog, "name");
```

---

# 60. `Object.defineProperty()`

Allows precise control over a property.

```js
const dog = {};

Object.defineProperty(dog, "name", {
  value: "Snoopy",
  writable: false,
  enumerable: true,
  configurable: false
});
```

This is more advanced than ordinary assignment, but it explains how JavaScript property behavior can be controlled.

---

# 61. `Object.defineProperties()`

Defines multiple properties with descriptors.

```js
Object.defineProperties(object, {
  name: {
    value: "Snoopy"
  },
  breed: {
    value: "Beagle"
  }
});
```

---

# 62. Getting Property Names and Descriptors

Useful reflection methods:

```js
Object.getOwnPropertyNames(object);
Object.getOwnPropertySymbols(object);
Object.getOwnPropertyDescriptors(object);
```

`Object.keys()` only returns enumerable own string-keyed properties.

These methods can reveal more.

---

# 63. `Object.groupBy()`

Modern JavaScript includes:

```js
Object.groupBy(items, callback);
```

Example:

```js
const numbers = [1, 2, 3, 4, 5];

const grouped = Object.groupBy(numbers, number => {
  return number % 2 === 0 ? "even" : "odd";
});
```

Possible result:

```js
{
  odd: [1, 3, 5],
  even: [2, 4]
}
```

Runtime support depends on the JavaScript version/environment.

---

# 64. Methods Ordinary Objects Inherit From `Object.prototype`

Most ordinary objects ultimately inherit behavior from:

```js
Object.prototype
```

Important inherited methods include:

```text
hasOwnProperty()
isPrototypeOf()
propertyIsEnumerable()
toLocaleString()
toString()
valueOf()
```

The `constructor` property is also inherited.

Legacy prototype helpers/accessors also exist in many runtimes:

```text
__proto__
__defineGetter__()
__defineSetter__()
__lookupGetter__()
__lookupSetter__()
```

Prefer modern APIs for new code.

---

# 65. `hasOwnProperty()`

Historically common:

```js
dog.hasOwnProperty("name");
```

Modern code should generally prefer:

```js
Object.hasOwn(dog, "name");
```

Why?

An object can:

- override `hasOwnProperty`
- have no `Object.prototype`
- come from unusual prototype construction

`Object.hasOwn()` avoids those problems.

---

# 66. `isPrototypeOf()`

Checks whether an object exists somewhere in another object's prototype chain.

```js
Dog.prototype.isPrototypeOf(snoopy);
```

Result:

```js
true
```

---

# 67. `propertyIsEnumerable()`

Checks whether an own property is enumerable.

```js
dog.propertyIsEnumerable("name");
```

---

# 68. `toString()`

Most ordinary objects inherit:

```js
toString()
```

For a normal plain object:

```js
dog.toString();
```

typically produces:

```text
[object Object]
```

Specialized built-in types may provide more useful versions.

---

# 69. `valueOf()`

Returns the primitive value associated with an object when applicable.

Plain objects generally return themselves.

Most beginner code rarely calls this directly.

---

# 70. `for...in`

You can iterate enumerable string properties:

```js
for (const key in dog) {
  console.log(key, dog[key]);
}
```

Important:

> `for...in` can include enumerable inherited properties.

If you specifically want own properties, common choices are:

```js
for (const key of Object.keys(dog)) {
  console.log(key, dog[key]);
}
```

or:

```js
for (const [key, value] of Object.entries(dog)) {
  console.log(key, value);
}
```

---

# 71. The `in` Operator

Checks whether a property exists directly or through the prototype chain.

```js
"name" in dog;
```

If `bark` lives on `Dog.prototype`:

```js
"bark" in snoopy;
```

returns:

```js
true
```

Compare:

```js
Object.hasOwn(snoopy, "bark");
```

which returns:

```js
false
```

So:

```text
property anywhere in lookup chain
→ "property" in object

property directly owned
→ Object.hasOwn(object, "property")
```

---

# 72. Optional Chaining

If nested data may not exist:

```js
const city = user.address.city;
```

can throw if:

```js
user.address
```

is `undefined`.

Optional chaining:

```js
const city = user.address?.city;
```

returns:

```js
undefined
```

instead of throwing at that missing step.

You can chain:

```js
user.profile?.address?.city;
```

---

# 73. Optional Method Calls

```js
dog.bark?.();
```

This calls `bark` only if it exists and is not `null` or `undefined`.

---

# 74. Objects Are Compared by Identity

```js
{} === {};
```

returns:

```js
false
```

because these are two separate object instances.

But:

```js
const a = {};
const b = a;

console.log(a === b);
```

returns:

```js
true
```

because both variables refer to the same object.

---

# 75. Object Assignment Does Not Automatically Copy

```js
const original = {
  score: 10
};

const copy = original;

copy.score = 20;

console.log(original.score);
```

Output:

```text
20
```

Both variables point to the same object.

For a new shallow object:

```js
const copy = {
  ...original
};
```

---

# 76. JSON Is Not the Same as a JavaScript Object

JavaScript object:

```js
const dog = {
  name: "Snoopy",

  bark() {
    return "Woof";
  }
};
```

JSON:

```json
{
  "name": "Snoopy"
}
```

JSON:

- is a data format
- uses stricter syntax
- requires quoted string keys
- cannot represent functions

Convert to JSON text:

```js
JSON.stringify(dog);
```

Convert JSON text to JavaScript:

```js
JSON.parse(text);
```

---

# 77. Symbols as Property Keys

Properties can also use `Symbol` keys.

```js
const id = Symbol("id");

const user = {
  [id]: 123
};
```

Symbols are useful when you want keys unlikely to collide with ordinary string keys.

---

# 78. Getters

A getter looks like a property but runs a function when read.

```js
const rectangle = {
  width: 10,
  height: 5,

  get area() {
    return this.width * this.height;
  }
};
```

Use:

```js
rectangle.area
```

not:

```js
rectangle.area()
```

---

# 79. Setters

A setter runs when a property is assigned.

```js
const person = {
  firstName: "",

  set name(value) {
    this.firstName = value.trim();
  }
};

person.name = "  Ada  ";
```

Now:

```js
person.firstName
```

is:

```text
Ada
```

---

# 80. Private Class Fields

Modern classes can define private fields:

```js
class BankAccount {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}
```

Outside code cannot directly access:

```js
account.#balance
```

---

# 81. Inheritance With Classes

```js
class Animal {
  eat() {
    return "Eating";
  }
}

class Dog extends Animal {
  bark() {
    return "Woof";
  }
}
```

Then:

```js
const dog = new Dog();

dog.eat();
dog.bark();
```

`Dog` instances inherit through the prototype system.

---

# 82. `super`

Inside a subclass:

```js
class Dog extends Animal {
  constructor(name) {
    super();
    this.name = name;
  }
}
```

`super()` calls the parent constructor.

You can also call parent methods:

```js
super.someMethod();
```

---

# 83. Composition vs Inheritance

Inheritance says:

```text
Dog IS AN Animal
```

Composition says:

```text
Dog HAS behavior supplied by another piece
```

Do not automatically use inheritance whenever objects share code.

Sometimes composing smaller behaviors is simpler than building deep inheritance trees.

---

# 84. Choosing an Object-Creation Pattern

## Plain Object Literal

```js
const user = {
  name: "Ada"
};
```

Good for:

```text
one record
configuration
structured data
lookup tables
simple state
```

## Factory Function

```js
function createUser(name) {
  return {
    name
  };
}
```

Good when:

```text
you want repeatable creation
without requiring new
```

## Constructor Function + Prototype

```js
function User(name) {
  this.name = name;
}

User.prototype.greet = function() {
  return `Hello ${this.name}`;
};
```

Good for:

```text
learning traditional JavaScript
legacy code
prototype-based instance patterns
```

## Class

```js
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hello ${this.name}`;
  }
}
```

Good when:

```text
instance-oriented modeling
shared methods
inheritance
encapsulation
```

## `Map`

```js
const users = new Map();
```

Good when:

```text
the main purpose is dynamic key/value storage
keys may not be strings
you want Map-specific methods
```

---

# 85. Quick Reference: Static `Object` Methods

These belong to:

```js
Object
```

and are called like:

```js
Object.keys(obj);
```

Practical reference:

```text
Object.assign()
Object.create()
Object.defineProperties()
Object.defineProperty()
Object.entries()
Object.freeze()
Object.fromEntries()
Object.getOwnPropertyDescriptor()
Object.getOwnPropertyDescriptors()
Object.getOwnPropertyNames()
Object.getOwnPropertySymbols()
Object.getPrototypeOf()
Object.groupBy()
Object.hasOwn()
Object.is()
Object.isExtensible()
Object.isFrozen()
Object.isSealed()
Object.keys()
Object.preventExtensions()
Object.seal()
Object.setPrototypeOf()
Object.values()
```

---

# 86. Quick Reference: Methods Inherited From `Object.prototype`

Ordinary objects normally inherit these:

```text
hasOwnProperty()
isPrototypeOf()
propertyIsEnumerable()
toLocaleString()
toString()
valueOf()
```

And the inherited property:

```text
constructor
```

Legacy helpers/accessors you may still encounter:

```text
__proto__
__defineGetter__()
__defineSetter__()
__lookupGetter__()
__lookupSetter__()
```

Prefer modern alternatives in new code.

---

# 87. Important Distinction: Static vs Instance Methods

This does **not** work:

```js
dog.keys();
```

Instead:

```js
Object.keys(dog);
```

Likewise:

```js
Object.values(dog);
Object.entries(dog);
```

These are static methods on:

```js
Object
```

not methods every object instance inherits.

By contrast, arrays have instance methods:

```js
array.map();
array.filter();
array.reduce();
```

Knowing where a method lives helps you read APIs correctly.

---

# 88. Common Beginner Mistakes

## Mistake 1: Using Object-Literal Syntax Inside a Constructor Body

Incorrect:

```js
function Dog(breed) {
  this.breed = breed;

  bark: function() {
    return "Woof";
  }
}
```

Inside a function body, this is not how you define an object property.

Correct instance method:

```js
function Dog(breed) {
  this.breed = breed;

  this.bark = function() {
    return "Woof";
  };
}
```

Or shared method:

```js
Dog.prototype.bark = function() {
  return "Woof";
};
```

## Mistake 2: Confusing Access With Ownership

Both let dogs bark:

```js
this.bark = function() {};
```

and:

```js
Dog.prototype.bark = function() {};
```

The difference:

```text
instance method
→ each instance owns one

prototype method
→ instances share one
```

## Mistake 3: Using Dot Notation With a Dynamic Key

Wrong intent:

```js
const key = "name";

dog.key;
```

Correct:

```js
dog[key];
```

## Mistake 4: Assuming `const` Makes an Object Immutable

```js
const dog = {};
dog.name = "Snoopy";
```

is valid.

## Mistake 5: Comparing Separate Objects With `===`

```js
{ name: "Ada" } === { name: "Ada" };
```

is:

```js
false
```

because they are different instances.

## Mistake 6: Forgetting That Spread Copies Are Shallow

```js
const copy = { ...original };
```

does not recursively clone every nested object.

## Mistake 7: Using `for...in` Without Thinking About Inheritance

Use:

```js
Object.keys()
Object.values()
Object.entries()
```

when you specifically want own enumerable properties.

---

# 89. Choosing Between Data Structures

Ask:

```text
Do I need one structured record?
→ object literal

Do I need key/value lookup?
→ object or Map

Do I need many similar objects?
→ factory, constructor, or class

Do instances have unique data?
→ put it on the instance

Do instances share identical behavior?
→ prototype/class method is a natural fit

Do I need ordered indexed values?
→ Array

Do I need unique values?
→ Set

Do I need arbitrary key types?
→ Map
```

---

# 90. Final Mental Model

A JavaScript object is:

```text
a collection of properties
+
a link to another object called its prototype
```

When you ask for:

```js
object.property
```

JavaScript conceptually searches:

```text
object itself
    ↓
its prototype
    ↓
that prototype's prototype
    ↓
...
    ↓
null
```

Instance state commonly lives here:

```text
object itself
```

Shared behavior commonly lives here:

```text
prototype
```

That one model explains a huge amount of JavaScript object behavior.

---

# 91. Quick Syntax Reference

## Create

```js
const obj = {};

const obj2 = new Object();

const child = Object.create(parent);

function Thing(value) {
  this.value = value;
}

const instance = new Thing(10);

class Thing2 {
  constructor(value) {
    this.value = value;
  }
}
```

## Read

```js
obj.name;
obj["name"];
obj[key];
```

## Add / Update

```js
obj.name = "Ada";
obj[key] = value;
```

## Delete

```js
delete obj.name;
```

## Check

```js
"name" in obj;

Object.hasOwn(obj, "name");
```

## Enumerate

```js
Object.keys(obj);
Object.values(obj);
Object.entries(obj);
```

## Copy / Merge

```js
const copy = { ...obj };

Object.assign({}, obj);
```

## Prototype

```js
Object.getPrototypeOf(obj);

Object.create(proto);

Constructor.prototype.method = function() {};
```

## Restrict Mutation

```js
Object.freeze(obj);
Object.seal(obj);
Object.preventExtensions(obj);
```

## Safe Nested Access

```js
obj.profile?.address?.city;
```

---

# 92. Practice Questions

1. What is the difference between:

```js
dog.name
```

and:

```js
dog[key]
```

?

2. Why does:

```js
this.bark = function() {};
```

create more function objects than:

```js
Dog.prototype.bark = function() {};
```

when many dogs are constructed?

3. Where does JavaScript look when a property does not exist directly on an object?

4. Why does:

```js
const copy = original;
```

not create an independent object?

5. What is the difference between:

```js
"name" in dog
```

and:

```js
Object.hasOwn(dog, "name")
```

?

6. Why might `Map` be a better choice than `{}` for some lookup-heavy problems?

7. Why should an arrow function usually not be your default choice for a method that relies on `this`?

8. What does `Object.keys()` return?

9. What is the difference between `Object.freeze()` and `Object.seal()`?

10. Why are JavaScript classes still related to prototypes?

---

# 93. Practice Lab

Create a constructor function:

```js
Dog
```

Each dog should have unique:

```text
name
breed
age
```

All dogs should share:

```text
bark()
describe()
```

Then create three dogs and test:

```js
dog1.bark === dog2.bark
```

Explain why the result has the value it does.

Next, rewrite the same model using:

```js
class Dog
```

Finally, create a lookup object that maps:

```text
breed code → breed name
```

and access it using a dynamic bracket key.

The goal is not just to make the code run.

The goal is to understand:

```text
what belongs to each object
what is shared
where JavaScript finds each property
```
