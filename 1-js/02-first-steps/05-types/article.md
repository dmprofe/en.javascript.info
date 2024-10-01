# Data types

A value in JavaScript is always of a certain type. For example, a string or a number.

There are eight basic data types in JavaScript. Here, we'll cover them in general and in the next chapters we'll talk about each of them in detail.

We can put any type in a variable. For example, a variable can at one moment be a string and then store a number:

```js
// no error
let message = "hello";
message = 123456;
```

Programming languages that allow such things, such as JavaScript, are called "dynamically typed", meaning that there exist data types, but variables are not bound to any of them.

## Number

```js
let n = 123;
n = 12.345;
```

The *number* type represents both integer and floating point numbers.

> [!t] t: This is a key difference of JS with other languages like Java
>
> - This "fusion" of integer+float is handy and cool for starting, but
>   has non trivial drawbacks when code becomes complex.

There are many operations for numbers, e.g. multiplication `*`, division `/`, addition `+`, subtraction `-`, and so on.

Besides regular numbers, there are so-called "special numeric values" which also belong to this data type: `Infinity`, `-Infinity` and `NaN`.

- `Infinity` represents the mathematical [Infinity](https://en.wikipedia.org/wiki/Infinity) ∞. It is a special value that's greater than any number.

    We can get it as a result of division by zero:

    ```js run
    alert( 1 / 0 ); // Infinity
    ```

    Or just reference it directly:

    ```js run
    alert( Infinity ); // Infinity
    ```
- `NaN` represents a computational error. It is a result of an incorrect or an undefined mathematical operation, for instance:

    ```js run
    alert( "not a number" / 2 ); // NaN, such division is erroneous
    ```

    `NaN` is sticky. Any further mathematical operation on `NaN` returns `NaN`:

    ```js run
    alert( NaN + 1 ); // NaN
    alert( 3 * NaN ); // NaN
    alert( "not a number" / 2 - 1 ); // NaN
    ```

    So, if there's a `NaN` somewhere in a mathematical expression, it propagates to the whole result (there's only one exception to that: `NaN ** 0` is `1`).

> [!t] t: In JS, "NaN" (**not** a number) **is** indeed a number
>
> - It is a number, with a "special numeric value".
> - This nuance is sometimes important.

```smart header="Mathematical operations are safe"
Doing maths is "safe" in JavaScript. We can do anything: divide by zero, treat non-numeric strings as numbers, etc.

The script will never stop with a fatal error ("die"). At worst, we'll get `NaN` as the result.
```

> [!t] t: It will not crash/die... Good, but...
>
> - It's a short term convenience not to be nagged, but errors may pass more
>   easily hidden and maybe be detected quite late, in production. Not good
>   at all.
> - "will never die": never say never. You can make it die with some
>   Mathematical operations... And for the previous paragraph, in fact you
>   will be lucky: when programming, you normally want to detect
>   problems as early as possible in development stages. And for sure
>   not in production (with 3 billion users and witnesses using your
>   -broken- software to make your company rich, or with a bunch of doctors
>   using your -broken- software to do brain surgeries).

Special numeric values formally belong to the "number" type. Of course they are not numbers in the common sense of this word.

We'll see more about working with numbers in the chapter <info:number>.

## BigInt [#bigint-type]

In JavaScript, the "number" type cannot safely represent integer values larger than <code>(2<sup>53</sup>-1)</code> (that's `9007199254740991`), or less than <code>-(2<sup>53</sup>-1)</code> for negatives.

To be really precise, the "number" type can store larger integers (up to <code>1.7976931348623157 * 10<sup>308</sup></code>), but outside of the safe integer range <code>±(2<sup>53</sup>-1)</code> there'll be a precision error, because not all digits fit into the fixed 64-bit storage. So an "approximate" value may be stored.

For example, these two numbers (right above the safe range) are the same:

```js
console.log(9007199254740991 + 1); // 9007199254740992
console.log(9007199254740991 + 2); // 9007199254740992
```

So to say, all odd integers greater than <code>(2<sup>53</sup>-1)</code> can't be stored at all in the "number" type.

For most purposes <code>±(2<sup>53</sup>-1)</code> range is quite enough, but sometimes we need the entire range of really big integers, e.g. for cryptography or microsecond-precision timestamps.

`BigInt` type was recently added to the language to represent integers of arbitrary length.

A `BigInt` value is created by appending `n` to the end of an integer:

```js
// the "n" at the end means it's a BigInt
const bigInt = 1234567890123456789012345678901234567890n;
```

As `BigInt` numbers are rarely needed, we don't cover them here, but devoted them a separate chapter <info:bigint>. Read it when you need such big numbers.


```smart header="Compatibility issues"
Right now, `BigInt` is supported in Firefox/Chrome/Edge/Safari, but not in IE.
```

You can check [*MDN* BigInt compatibility table](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt#Browser_compatibility) to know which versions of a browser are supported.

## String

A string in JavaScript must be surrounded by quotes.

```js
let str = "Hello";
let str2 = 'Single quotes are ok too';
let phrase = `can embed another ${str}`;
```

In JavaScript, there are 3 types of quotes.

1. Double quotes: `"Hello"`.
2. Single quotes: `'Hello'`.
3. Backticks: <code>&#96;Hello&#96;</code>.

Double and single quotes are "simple" quotes. There's practically no difference between them in JavaScript.

Backticks are "extended functionality" quotes. They allow us to embed variables and expressions into a string by wrapping them in `${…}`, for example:

```js run
let name = "John";

// embed a variable
alert( `Hello, *!*${name}*/!*!` ); // Hello, John!

// embed an expression
alert( `the result is *!*${1 + 2}*/!*` ); // the result is 3
```

The expression inside `${…}` is evaluated and the result becomes a part of the string. We can put anything in there: a variable like `name` or an arithmetical expression like `1 + 2` or something more complex.

> [!t] t: It can be any expression
>
> - Another typicall example is the execution of a function, returning a value.
> - Or it can contain other template literals nested:
>
>   ```js
>   alert(`a ${`b ${`c`} d`} e`); // Output?
>   ```
>
> - Or any combination. Any expression (or series of expressions) can go
>   inside the `${...}`.
> - The same applies e.g. for what we put on the right of a `=`
>   assignment, or as a function parameter, or at an if or loop guard, or
>   at many other places that accept an expression: they all accept an
>   (arbitrarily complex) expression.
> - So it is very good to know what can be an expression in JS (and what
>   cannot be).
> - Specially since functional programming style and many programmers use
>   extensively complex expressions: we need at least this to understand the
>   code.

Please note that this can only be done in backticks. Other quotes don't have this embedding functionality!
```js run
alert( "the result is ${1 + 2}" ); // the result is ${1 + 2} (double quotes do nothing)
```

> [!t] t: Backticks \`...\` are called "template literal".
>
> - Good to know this technical name.
> - ES6+.
> - Also it allows normal newlines inside, contrary to the other two string
>   notations. In the other notations, you can specify a newline with "\n".

> [!t] t: Other JS literals
>
> - There are many JS literals producing primitives (we see them in this
>   chapter, but now I'm showing their **technical names**):
>   - string literals (`'...' ` and `"..."` and templete literals \``...`\`):
>     - Specification of a particular strings with single and double quotes.
>     - We will see details on escaping, inserting more special characters,
>       inserting unicode, ...
>   - numeric literals:
>     - Specification of a particular number or bigint.
>     - We will see that they can be expressed in different ways (e.g. different
>       bases, with scientific notation, ...).
>   - boolean literal: `true` or `false`.
>   - null literal: `null`.
> - Other JS literals, not producing primitives (we will see them later in the
>   course):
>   - object literal (`{...}`) and array literal (`[...]`):
>     - Permit specifying directly an object or an array.
>     - Not many languages provide this for creating objects.
>   - regular expression literal (`/.../`):
>     - Produces directly an object of the class RegExp.
> - All these literals are the powerful JS "literal notation", also the basis
>   of the ubiquous JSON format (JSON stands for JS Object Notation... since
>   almost everything in JS ends up being an Object).

We'll cover strings more thoroughly in the chapter <info:string>.

```smart header="There is no *character* type."
In some languages, there is a special "character" type for a single character. For example, in the C language and in Java it is called "char".

In JavaScript, there is no such type. There's only one type: `string`. A string may consist of zero characters (be empty), one character or many of them.
```

## Boolean (logical type)

The boolean type has only two values: `true` and `false`.

This type is commonly used to store yes/no values: `true` means "yes, correct", and `false` means "no, incorrect".

For instance:

```js
let nameFieldChecked = true; // yes, name field is checked
let ageFieldChecked = false; // no, age field is not checked
```

Boolean values also come as a result of comparisons:

```js run
let isGreater = 4 > 1;

alert( isGreater ); // true (the comparison result is "yes")
```

We'll cover booleans more deeply in the chapter <info:logical-operators>.

> [!t] t: If it's boolean, use a boolean for good
>
> - Please use a boolean when something is semantically a boolean.
> - Use `true` and `false` (reserved words), and **not** `"true"` and
>   `"false"` (strings... and JS will not complain, remember that it is
>   dinamically typed).
> - And for sure don't use for a semantic boolean something like `"yes"` and
>   `"no"`, or `"si"` and `"no"`. The way it is showed later (if needed) to a
>   human end users should not contaminate all your code.

## The "null" value

The special `null` value does not belong to any of the types described above.

It forms a separate type of its own which contains only the `null` value:

```js
let age = null;
```

In JavaScript, `null` is not a "reference to a non-existing object" or a "null pointer" like in some other languages.

It's just a special value which represents "nothing", "empty" or "value unknown".

The code above states that `age` is unknown.

## The "undefined" value

The special value `undefined` also stands apart. It makes a type of its own, just like `null`.

The meaning of `undefined` is "value is not assigned".

If a variable is declared, but not assigned, then its value is `undefined`:

```js run
let age;

alert(age); // shows "undefined"
```

Technically, it is possible to explicitly assign `undefined` to a variable:

```js run
let age = 100;

// change the value to undefined
age = undefined;

alert(age); // "undefined"
```

...But we don't recommend doing that. Normally, one uses `null` to assign an "empty" or "unknown" value to a variable, while `undefined` is reserved as a default initial value for unassigned things.

> [!t] t: null or undefined?
>
> - Nuance between "null" and "undefined" is tricky. Stick to the good advice:
>   - Leave "undefined" for something not put by the programmer.
>   - "null" is something at some point explicitly placed by a programmer.

## Objects and Symbols

The `object` type is special.

All other types are called "primitive" because their values can contain only a single thing (be it a string or a number or whatever). In contrast, objects are used to store collections of data and more complex entities.

Being that important, objects deserve a special treatment. We'll deal with them later in the chapter <info:object>, after we learn more about primitives.

> [!t] t: And the arrays?
>
> - They are no mentioned here since In JS, they are a sub-type of objects.
>   `typeof` (see next section) on them returns `"object"`
> - Some say that in JS everything are objects:
>   - The primitive types in JS (except null and undefined) have also "object
>     wrappers".
>     - E.g.  methods for a string or for a number.
>     - One can test this by using completion in the console.
>   - But be careful: primitives (even with their "object wrapper") behave
>     very differently non primitives. It is critical to distinguish them.

The `symbol` type is used to create unique identifiers for objects. We have to mention it here for the sake of completeness, but also postpone the details till we know objects.

## The typeof operator [#type-typeof]

The `typeof` operator returns the type of the operand. It's useful when we want to process values of different types differently or just want to do a quick check.

A call to `typeof x` returns a string with the type name:

```js
typeof undefined // "undefined"

typeof 0 // "number"

typeof 10n // "bigint"

typeof true // "boolean"

typeof "foo" // "string"

typeof Symbol("id") // "symbol"

*!*
typeof Math // "object"  (1)
*/!*

*!*
typeof null // "object"  (2)
*/!*

*!*
typeof alert // "function"  (3)
*/!*
```

The last three lines may need additional explanation:

1. `Math` is a built-in object that provides mathematical operations. We will learn it in the chapter <info:number>. Here, it serves just as an example of an object.
2. The result of `typeof null` is `"object"`. That's an officially recognized error in `typeof`, coming from very early days of JavaScript and kept for compatibility. Definitely, `null` is not an object. It is a special value with a separate type of its own. The behavior of `typeof` is wrong here.
3. The result of `typeof alert` is `"function"`, because `alert` is a function. We'll study functions in the next chapters where we'll also see that there's no special "function" type in JavaScript. Functions belong to the object type. But `typeof` treats them differently, returning `"function"`. That also comes from the early days of JavaScript. Technically, such behavior isn't correct, but can be convenient in practice.

> [!t] t: Some other interesting typeof examples
>
> ```js
> typeof typeof 0 // "string": typeof returns a string describing the type
> typeof undefined // "undefined"
> typeof NaN // "number"
> typeof '' // "string" (empty string: not undefined, not null, ...)
> typeof "" // The same
> ```
>
> - So when typeof returns "object", it is not conclusive.
> - When it returns a different thing, it is more conclusive.
>   - Except for NaN that is a number: if we want to differentiate it, we
>     will see there is a way.
> - There are (sophisticated) ways to overcome the limitation that too many
>   things return "object" when applying "typeof".

```smart header="The `typeof(x)` syntax"
You may also come across another syntax: `typeof(x)`. It's the same as `typeof x`.

To put it clear: `typeof` is an operator, not a function. The parentheses here aren't a part of `typeof`. It's the kind of parentheses used for mathematical grouping.

Usually, such parentheses contain a mathematical expression, such as `(2 + 2)`, but here they contain only one argument `(x)`. Syntactically, they allow to avoid a space between the `typeof` operator and its argument, and some people like it.

Some people prefer `typeof(x)`, although the `typeof x` syntax is much more common.
```

## Summary

There are 8 basic data types in JavaScript.

- Seven primitive data types:
    - `number` for numbers of any kind: integer or floating-point, integers are limited by <code>±(2<sup>53</sup>-1)</code>.
    - `bigint` for integer numbers of arbitrary length.
    - `string` for strings. A string may have zero or more characters, there's no separate single-character type.
    - `boolean` for `true`/`false`.
    - `null` for unknown values -- a standalone type that has a single value `null`.
    - `undefined` for unassigned values -- a standalone type that has a single value `undefined`.
    - `symbol` for unique identifiers.
- And one non-primitive data type:
    - `object` for more complex data structures.

The `typeof` operator allows us to see which type is stored in a variable.

- Usually used as `typeof x`, but `typeof(x)` is also possible.
- Returns a string with the name of the type, like `"string"`.
- For `null` returns `"object"` -- this is an error in the language, it's not actually an object.

In the next chapters, we'll concentrate on primitive values and once we're familiar with them, we'll move on to objects.
