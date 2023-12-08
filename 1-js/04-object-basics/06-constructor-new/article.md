# Constructor, operator "new"

The regular `{...}` syntax allows us to create one object. But often we need to create many similar objects, like multiple users or menu items and so on.

That can be done using constructor functions and the `"new"` operator.

> ![t]- t: Factory functions
>
> - Before entering this (constructor functions and `new` operator), there
>   is another way of generating objects from a function.
> - This is what is called a "factory function" (or factory method, if it is
>   a method): it creates inside its code a new object and returns it to the
>   caller.
> - The simple example that we will see in this guide for a constructor of a
>   "user" object could be instead with a factory something like this:
>
>   ```js
>   function createUser(name) {
>     return {
>       name: name,
>       // Or more simply in this case:
>       //   name,
>       isAdmin: false
>     }
>   }
>   let user = createUser("Jack");
>   ```
>
> - The internal implementation of the factory can be with a literal
>   notation as in this simple example, or with whatever the implementer
>   decides (e.g. with `new` on some constructor). The key is that
>   **externally** the caller does not use new with this interface for
>   creating objects (and does not need to know anything about `new` operator).
> - Some libraries/API's provide their functionality as factories (e.g.
>   `document.createElement()`). Others, as constructors to be used with `new`.
>   And others, provide both ways.
> - Note that an initial capital is not used on factories. The name may help
>   the user by suggesting that something new is created inside for
>   returning it... but we cannot count on this always.
> - Note: in Java, the equivalent of this is a
>   ["static factory method"](https://www.baeldung.com/java-constructors-vs-static-factory-methods).

## Constructor function

Constructor functions technically are regular functions. There are two conventions though:

1. They are named with capital letter first.
2. They should be executed only with `"new"` operator.

For instance:

```js run
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

*!*
let user = new User("Jack");
*/!*

alert(user.name); // Jack
alert(user.isAdmin); // false
```

When a function is executed with `new`, it does the following steps:

1. A new empty object is created and assigned to `this`.
2. The function body executes. Usually it modifies `this`, adds new properties to it.
3. The value of `this` is returned.

In other words, `new User(...)` does something like:

```js
function User(name) {
*!*
  // this = {};  (implicitly)
*/!*

  // add properties to this
  this.name = name;
  this.isAdmin = false;

*!*
  // return this;  (implicitly)
*/!*
}
```

So `let user = new User("Jack")` gives the same result as:

```js
let user = {
  name: "Jack",
  isAdmin: false
};
```

> ![t]- t: Yet another behaviour of `this` dependent on the way of calling a function.
>
> - To be added to what we already learned about `this`.

Now if we want to create other users, we can call `new User("Ann")`, `new User("Alice")` and so on. Much shorter than using literals every time, and also easy to read.

That's the main purpose of constructors -- to implement reusable object creation code.

> ![t] t: This can also be achieved, as seen above, with factory functions.

Let's note once again -- technically, any function (except arrow functions, as they don't have `this`) can be used as a constructor. It can be run with `new`, and it will execute the algorithm above. The "capital letter first" is a common agreement, to make it clear that a function is to be run with `new`.

> ![t]- t: Classes are not needed in JS.
>
> - Note that, contrary to Java, in JS we can define and use constructor
>   functions to instantiate objects without having to create/define a class.
> - The usage of these constructor functions is with `new`, similar to the
>   usage of a constructor of a class in Java.
> - We'll see later in the course that in ES6 they introduced the possibility
>   to define explicity classes with a syntax quite similar to Java (as an
>   alternative of defining constructor functions like the one we are seeing
>   here).

````smart header="new function() { ... }"
If we have many lines of code all about creation of a single complex object, we can wrap them in an immediately called constructor function, like this:

```js
// create a function and immediately call it with new
let user = new function() { 
  this.name = "John";
  this.isAdmin = false;

  // ...other code for user creation
  // maybe complex logic and statements
  // local variables etc
};
```

This constructor can't be called again, because it is not saved anywhere, just created and called. So this trick aims to encapsulate the code that constructs the single object, without future reuse.
````

## Constructor mode test: new.target

```smart header="Advanced stuff"
The syntax from this section is rarely used, skip it unless you want to know everything.
```

Inside a function, we can check whether it was called with `new` or without it, using a special `new.target` property.

It is undefined for regular calls and equals the function if called with `new`:

```js run
function User() {
  alert(new.target);
}

// without "new":
*!*
User(); // undefined
*/!*

// with "new":
*!*
new User(); // function User { ... }
*/!*
```

That can be used inside the function to know whether it was called with `new`, "in constructor mode", or without it, "in regular mode".

We can also make both `new` and regular calls to do the same, like this:

```js run
function User(name) {
  if (!new.target) { // if you run me without new
    return new User(name); // ...I will add new for you
  }

  this.name = name;
}

let john = User("John"); // redirects call to new User
alert(john.name); // John
```

This approach is sometimes used in libraries to make the syntax more flexible. So that people may call the function with or without `new`, and it still works.

> ![t] t: This function thus can be used as constructor or as factory.

Probably not a good thing to use everywhere though, because omitting `new` makes it a bit less obvious what's going on. With `new` we all know that the new object is being created.

> ![t] t: But many API's only provide factories (ideally, with non capital initial letter).

## Return from constructors

Usually, constructors do not have a `return` statement. Their task is to write all necessary stuff into `this`, and it automatically becomes the result.

But if there is a `return` statement, then the rule is simple:

- If `return` is called with an object, then the object is returned instead of `this`.
- If `return` is called with a primitive, it's ignored.

In other words, `return` with an object returns that object, in all other cases `this` is returned.

For instance, here `return` overrides `this` by returning an object:

```js run
function BigUser() {

  this.name = "John";

  return { name: "Godzilla" };  // <-- returns this object
}

alert( new BigUser().name );  // Godzilla, got that object
```

And here's an example with an empty `return` (or we could place a primitive after it, doesn't matter):

```js run
function SmallUser() {

  this.name = "John";

  return; // <-- returns this
}

alert( new SmallUser().name );  // John
```

Usually constructors don't have a `return` statement. Here we mention the special behavior with returning objects mainly for the sake of completeness.

````smart header="Omitting parentheses"
By the way, we can omit parentheses after `new`:

```js
let user = new User; // <-- no parentheses
// same as
let user = new User();
```

Omitting parentheses here is not considered a "good style", but the syntax is permitted by specification.
````

> ![t] t: Only can omit them when no parameters are used. And even there, it's confusing.

## Methods in constructor

Using constructor functions to create objects gives a great deal of flexibility. The constructor function may have parameters that define how to construct the object, and what to put in it.

Of course, we can add to `this` not only properties, but methods as well.

For instance, `new User(name)` below creates an object with the given `name` and the method `sayHi`:

```js run
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}

*!*
let john = new User("John");

john.sayHi(); // My name is: John
*/!*

/*
john = {
   name: "John",
   sayHi: function() { ... }
}
*/
```

> ![t]- t: The "method shorthand" we saw for litteral notation cannot be used here.

To create complex objects, there's a more advanced syntax, [classes](info:classes), that we'll cover later.

> ![t] t: To create complex objects, but also simple ones.

## Summary

- Constructor functions or, briefly, constructors, are regular functions, but there's a common agreement to name them with capital letter first.
- Constructor functions should only be called using `new`. Such a call implies a creation of empty `this` at the start and returning the populated one at the end.

We can use constructor functions to make multiple similar objects.

> ![t] t: And we can also use factories for that matter.

JavaScript provides constructor functions for many built-in language objects: like `Date` for dates, `Set` for sets and others that we plan to study.

```smart header="Objects, we'll be back!"
In this chapter we only cover the basics about objects and constructors. They are essential for learning more about data types and functions in the next chapters.

After we learn that, we return to objects and cover them in-depth in the chapters <info:prototypes> and <info:classes>.
```
