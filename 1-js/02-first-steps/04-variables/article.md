# Variables

Most of the time, a JavaScript application needs to work with information. Here are two examples:
1. An online shop -- the information might include goods being sold and a shopping cart.
2. A chat application -- the information might include users, messages, and much more.

Variables are used to store this information.

## A variable

A [variable](https://en.wikipedia.org/wiki/Variable_(computer_science)) is a "named storage" for data. We can use variables to store goodies, visitors, and other data.

> [!t] t: Stores information", but **not persistently**!
>
> - This is stored in registers/RAM (volatile memory).
> - For persistence, we need quite other more advanced mechanisms:
>   - For browsers these are seen in chapter "ADD 4 Storing data in the
>     browser".
>   - For server side/node, see chapter "ADD 2 Binary data, files", and its
>     article "2.4 File and FileReader".
>   - And for sure, we may store persistently stuff in DB's with a variety
>     of mechanisms completely out of the scope of this FE course.

To create a variable in JavaScript, use the `let` keyword.

The statement below creates (in other words: *declares*) a variable with the name "message":

```js
let message;
```

Now, we can put some data into it by using the assignment operator `=`:

```js
let message;

*!*
message = 'Hello'; // store the string 'Hello' in the variable named message
*/!*
```

The string is now saved into the memory area associated with the variable. We can access it using the variable name:

```js run
let message;
message = 'Hello!';

*!*
alert(message); // shows the variable content
*/!*
```

To be concise, we can combine the variable declaration and assignment into a single line:

```js run
let message = 'Hello!'; // define the variable and assign the value

alert(message); // Hello!
```

> [!t] t: Shared global scope and order test
> - Declare a variable in a .js file, load it in an .html, then use it with
>   alert in the contents of a following `<script>` element and from the
>   console.
> - Note that it is not available if we invert the order.

We can also declare multiple variables in one line:

```js no-beautify
let user = 'John', age = 25, message = 'Hello';
```

That might seem shorter, but we don't recommend it. For the sake of better readability, please use a single line per variable.

The multiline variant is a bit longer, but easier to read:

```js
let user = 'John';
let age = 25;
let message = 'Hello';
```

Some people also define multiple variables in this multiline style:

```js no-beautify
let user = 'John',
  age = 25,
  message = 'Hello';
```

...Or even in the "comma-first" style:

```js no-beautify
let user = 'John'
  , age = 25
  , message = 'Hello';
```

Technically, all these variants do the same thing. So, it's a matter of personal taste and aesthetics.

> [!t] t: ... unless you are collaborating
>
> - When working with others, follow the style guides agreed or in use.
> - If none are used, use SemiStandardJS (with a linter), and forget about it.

````smart header="`var` instead of `let`"
In older scripts, you may also find another keyword: `var` instead of `let`:

```js
*!*var*/!* message = 'Hello';
```

The `var` keyword is *almost* the same as `let`. It also declares a variable, but in a slightly different, "old-school" way.

There are subtle differences between `let` and `var`, but they do not matter for us yet. We'll cover them in detail in the chapter <info:var>.
````

## A real-life analogy

We can easily grasp the concept of a "variable" if we imagine it as a "box" for data, with a uniquely-named sticker on it.

For instance, the variable `message` can be imagined as a box labeled `"message"` with the value `"Hello!"` in it:

![](variable.svg)

We can put any value in the box.

We can also change it as many times as we want:

```js run
let message;

message = 'Hello!';

message = 'World!'; // value changed

alert(message);
```

When the value is changed, the old data is removed from the variable:

![](variable-change.svg)

We can also declare two variables and copy data from one into the other.

> [!t] t: Only "copies data" for primitive data types (number, string, boolean, ...).
>  - For non-primitive (array, object, ...), only the reference is copied when
>    asigning with `=`, then both variables point to the same place.

```js run
let hello = 'Hello world!';

let message;

*!*
// copy 'Hello world' from hello into message
message = hello;
*/!*

// now two variables hold the same data
alert(hello); // Hello world!
alert(message); // Hello world!
```

````warn header="Declaring twice triggers an error"
A variable should be declared only once.

A repeated declaration of the same variable is an error:

```js run
let message = "This";

// repeated 'let' leads to an error
let message = "That"; // SyntaxError: 'message' has already been declared
```
So, we should declare a variable once and then refer to it without `let`.
````

```smart header="Functional languages"
It's interesting to note that there exist so-called [pure functional](https://en.wikipedia.org/wiki/Purely_functional_programming) programming languages, such as [Haskell](https://en.wikipedia.org/wiki/Haskell), that forbid changing variable values.

In such languages, once the value is stored "in the box", it's there forever. If we need to store something else, the language forces us to create a new box (declare a new variable). We can't reuse the old one.

Though it may seem a little odd at first sight, these languages are quite capable of serious development. More than that, there are areas like parallel computations where this limitation confers certain benefits.
```

> [!t] t: Functional programming on non pure functional languages:
>
  - Why does jsinfo talk about strange "functional languages"?
  - It is because there is a huge trend in some "non pure functional
    languages" (like JS and others) to use them increasingly in a
    "functional way": that is restrict oneself to (or favoring) the ways of
    doing things in that way.
  - This has been increasingly adopted since ES6 and in the way that many JS
    frameworks and libraries work and are used.
  - Forbiding changing variable values is only one of the typical aspects of
    functional programming. There are many others, that we will address
    during this course.
  - When comming from "classical" languages, it may get some time to be used
    to this programming style. But it has some benefits. And in any case,
    **many people** program nowadays in this way, so we'd better be able
    *understand their code!

## Variable naming [#variable-naming]

There are two limitations on variable names in JavaScript:

1. The name must contain only letters, digits, or the symbols `$` and `_`.
2. The first character must not be a digit.

Examples of valid names:

```js
let userName;
let test123;
```

When the name contains multiple words, [camelCase](https://en.wikipedia.org/wiki/CamelCase) is commonly used. That is: words go one after another, each word except first starting with a capital letter: `myVeryLongName`.

> [!t] t: use lowerCamelCase, but **not** UpperCamelCase **nor** snake_case

> - Don't use UpperCamelCase (even if you can)
>   - Nor for variables names, nor for normal function names.
>   - Convention reserves this for class names (and its alternative in JS
>     "constructor functions").
> - Don't use snake_case (even if you can):
>   - Convention is lowerCamelCase.

What's interesting -- the dollar sign `'$'` and the underscore `'_'` can also be used in names. They are regular symbols, just like letters, without any special meaning.

These names are valid:

```js run untrusted
let $ = 1; // declared a variable with the name "$"
let _ = 2; // and now a variable with the name "_"

alert($ + _); // 3
```

Examples of incorrect variable names:

```js no-beautify
let 1a; // cannot start with a digit

let my-name; // hyphens '-' aren't allowed in the name
```

```smart header="Case matters"
Variables named `apple` and `APPLE` are two different variables.
```

````smart header="Non-Latin letters are allowed, but not recommended"
It is possible to use any language, including cyrillic letters, Chinese logograms and so on, like this:

```js
let имя = '...';
let 我 = '...';
```

Technically, there is no error here. Such names are allowed, but there is an international convention to use English in variable names. Even if we're writing a small script, it may have a long life ahead. People from other countries may need to read it some time.
````

````warn header="Reserved names"
There is a [list of reserved words](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords), which cannot be used as variable names because they are used by the language itself.

For example: `let`, `class`, `return`, and `function` are reserved.

The code below gives a syntax error:

```js run no-beautify
let let = 5; // can't name a variable "let", error!
let return = 5; // also can't name it "return", error!
```
````

> [!t] t: Memorize all... Or better use a linter
>
> - We may want the help of a linter here to warn us on time.
> - Remember that, otherwhise, until that line of code is stimulated, you
>   will not get any error. Do you have a thorough regression unit test
>   battery?
> - The alternative is to memorize every reserved word (and hope it does not
>   change with new additions to the standard).

````warn header="An assignment without `use strict`"

Normally, we need to define a variable before using it. But in the old times, it was technically possible to create a variable by a mere assignment of the value without using `let`. This still works now if we don't put `use strict` in our scripts to maintain compatibility with old scripts.

```js run no-strict
// note: no "use strict" in this example

num = 5; // the variable "num" is created if it didn't exist

alert(num); // 5
```

This is a bad practice and would cause an error in strict mode:

```js
"use strict";

*!*
num = 5; // error: num is not defined
*/!*
```
````

> [!t]: Once warned...
>
> - We may want to use undeclared variables for quick tests on the console.
> - But don't get used to it in your code, or it will byte you.

## Constants

To declare a constant (unchanging) variable, use `const` instead of `let`:

```js
const myBirthday = '18.04.1982';
```

Variables declared using `const` are called "constants". They cannot be reassigned. An attempt to do so would cause an error:

```js run
const myBirthday = '18.04.1982';

myBirthday = '01.01.2001'; // error, can't reassign the constant!
```

When a programmer is sure that a variable will never change, they can declare it with `const` to guarantee and clearly communicate that fact to everyone.

> [!t] t: To const or not to const?
>
> - Some style guides/linters recommend to use `const` always by default for
>   variables...
>   - ... Then only change to `let` if it happens to be needed (defensive
>     programming). This is an easy to follow rule and systematic, and we will
>     adhere to it
>   - `const` on non-primitive types (arrays, objects, ...):
>     - The variable may be typically constant, even if what is pointed to can
>       be altered in its fields ("not semantically constant").
> - Some other prefer to use const only when semantically constant (which in
>   any case the language is not able to ensure for the contents of
>   non-primitive types).

### Uppercase constants

There is a widespread practice to use constants as aliases for difficult-to-remember values that are known prior to execution.

Such constants are named using capital letters and underscores.

For instance, let's make constants for colors in so-called "web" (hexadecimal) format:

```js run
const COLOR_RED = "#F00";
const COLOR_GREEN = "#0F0";
const COLOR_BLUE = "#00F";
const COLOR_ORANGE = "#FF7F00";

// ...when we need to pick a color
let color = COLOR_ORANGE;
alert(color); // #FF7F00
```

Benefits:

- `COLOR_ORANGE` is much easier to remember than `"#FF7F00"`.
- It is much easier to mistype `"#FF7F00"` than `COLOR_ORANGE`.
- When reading the code, `COLOR_ORANGE` is much more meaningful than `#FF7F00`.

When should we use capitals for a constant and when should we name it normally? Let's make that clear.

Being a "constant" just means that a variable's value never changes. But there are constants that are known prior to execution (like a hexadecimal value for red) and there are constants that are *calculated* in run-time, during the execution, but do not change after their initial assignment.

For instance:

```js
const pageLoadTime = /* time taken by a webpage to load */;
```

The value of `pageLoadTime` is not known prior to the page load, so it's named normally. But it's still a constant because it doesn't change after assignment.

In other words, capital-named constants are only used as aliases for "hard-coded" values.

## Name things right

Talking about variables, there's one more extremely important thing.

A variable name should have a clean, obvious meaning, describing the data that it stores.

Variable naming is one of the most important and complex skills in programming. A quick glance at variable names can reveal which code was written by a beginner versus an experienced developer.

In a real project, most of the time is spent modifying and extending an existing code base rather than writing something completely separate from scratch. When we return to some code after doing something else for a while, it's much easier to find information that is well-labeled. Or, in other words, when the variables have good names.

Please spend time thinking about the right name for a variable before declaring it. Doing so will repay you handsomely.

Some good-to-follow rules are:

- Use human-readable names like `userName` or `shoppingCart`.
- Stay away from abbreviations or short names like `a`, `b`, `c`, unless you really know what you're doing.
- Make names maximally descriptive and concise. Examples of bad names are `data` and `value`. Such names say nothing. It's only okay to use them if the context of the code makes it exceptionally obvious which data or value the variable is referencing.
- Agree on terms within your team and in your own mind. If a site visitor is called a "user" then we should name related variables `currentUser` or `newUser` instead of `currentVisitor` or `newManInTown`.


> [!t] t: "Stay away from abbreviations or short names"
>
> - I will sometimes skip this rule for inmmediateness: on demos and
>   examples, specially orally explained or out of a real program context.

Sounds simple? Indeed it is, but creating descriptive and concise variable names in practice is not. Go for it.

> [!t] t: It's difficult to name things right, but **EXTREMELY IMPORTANT**
>
> - Many things here also applies not only to variable names, but also to
>   functions, classes, modules, files, folders, whole projects, whole
>   companies, web sites, ...
> - The more limited is the scope (the more local variable), the less ambigüity.
>   And also the less risk of name clash. Go for local!
> - Too long names also reduce legibility: use the necessary and sufficient
>   words in this context/scope.
>   - But better too long than too short.
>   - Your editor completion may be your friend here.
> - "Sounds simple?" In practice, it is one of the [two hard things in
>   programming](https://martinfowler.com/bliki/TwoHardThings.html).
>   - But it is very important: at least, pay attention to it to start
>     improving.
> - Not easy to preview all the future. Refactor names when needed!
>   - Modern Agile development practices recommend this (counting on a good
>     regression test battery).

```smart header="Reuse or create?"
And the last note. There are some lazy programmers who, instead of declaring new variables, tend to reuse existing ones.

As a result, their variables are like boxes into which people throw different things without changing their stickers. What's inside the box now? Who knows? We need to come closer and check.

Such programmers save a little bit on variable declaration but lose ten times more on debugging.

> [!t] t: Not only them...
>
> ... but also any collaborator or future mantainer.

An extra variable is good, not evil.

Modern JavaScript minifiers and browsers optimize code well enough, so it won't create performance issues. Using different variables for different values can even help the engine optimize your code.
```

> [!t] t: On task "Create a variable with the name of our planet" (spoiler alert)
>
> - Not sure if a perfect example... but interesting to think about it.
> - Shouldn't this be a constant? Unless we go multilingual.
> - "planet" here is for sure not good enough.
> - But maybe "ourPlanet" (without "Name") could be enough and maybe
>   sufficient (implicit).
> - If more things are added later, they can take "ourPlanetSize", ...
> - And if some day we refactor to an object "ourPlanet", then it will
>   for sure get a "name" attribute in that context.
> - "ourPlanetName" is in any case also good. In doubt, tend to long.

## Summary

We can declare variables to store data by using the `var`, `let`, or `const` keywords.

- `let` -- is a modern variable declaration.
- `var` -- is an old-school variable declaration. Normally we don't use it at all, but we'll cover subtle differences from `let` in the chapter <info:var>, just in case you need them.
- `const` -- is like `let`, but the value of the variable can't be changed.

Variables should be named in a way that allows us to easily understand what's inside them.
