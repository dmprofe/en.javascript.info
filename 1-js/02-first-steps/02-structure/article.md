# Code structure

The first thing we'll study is the building blocks of code.

## Statements

Statements are syntax constructs and commands that perform actions.

We've already seen a statement, `alert('Hello, world!')`, which shows the message "Hello, world!".

We can have as many statements in our code as we want. Statements can be separated with a semicolon.

For example, here we split "Hello World" into two alerts:

```js run no-beautify
alert('Hello'); alert('World');
```

Usually, statements are written on separate lines to make the code more readable:

```js run no-beautify
alert('Hello');
alert('World');
```

> [!t] t: Statement vs expression:
>
> - TODO: maybe place this comment later in the course... but **not too
>   late**. Lots of stuff in JS and modern practices (e.g. tending to
>   functional programming) is facilitated by acquiring this basic
>   knowledge.
> - There is a distinction in programming languages between "statements" and
>   "expressions" (each language may have some nuance on this).
> - You can sort of survive without knowing the details/distinctions, but
>   getting into this at some point will help support a lot of JS knowledge,
>   and better understanding and producing code (that may result too magic
>   otherwhise).
> - You can have a look to this
>   [short and gentle introduction to statements vs. expressions in JS](https://www.joshwcomeau.com/javascript/statements-vs-expressions/).
>   These are the key takeaways:
>   - Expression is a bit of code that produces a value (or directly
>     represents a value), usable in some particular "slots" of an statement
>     (places where they can be).
>   - In the "slot" for an expression, you cannot place a complete
>     statement: only an expression.
>   - But an expression can be the only thing placed in an statement (they
>     are called then "expression statement") in which case they returned
>     value is "lost" (except if executed in the console, where it is
>     printed). But it may make sense doing this when they are doing
>     something colateral besides returning their value.
>   - Statements:
>     - Can be chained (even in the same line) with ";".
>     - Can be (nestedly) grouped in blocks with `{}`.
>       - These are called
>         [block statements in JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block).
>   - Expressions:
>     - Can be chained (even in the same line) with ",". In that case, the
>       last one is the one generating the whole value.
>     - Can be (nestedly) grouped with `()`.

## Semicolons [#semicolon]

A semicolon may be omitted in most cases when a line break exists.

This would also work:

```js run no-beautify
alert('Hello')
alert('World')
```

Here, JavaScript interprets the line break as an "implicit" semicolon. This is called an [automatic semicolon insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion).

**In most cases, a newline implies a semicolon. But "in most cases" does not mean "always"!**

There are cases when a newline does not mean a semicolon. For example:

```js run no-beautify
alert(3 +
1
+ 2);
```

The code outputs `6` because JavaScript does not insert semicolons here. It is intuitively obvious that if the line ends with a plus `"+"`, then it is an "incomplete expression", so a semicolon there would be incorrect. And in this case, that works as intended.

**But there are situations where JavaScript "fails" to assume a semicolon where it is really needed.**

Errors which occur in such cases are quite hard to find and fix.

````smart header="An example of an error"
If you're curious to see a concrete example of such an error, check this code out:

```js run
alert("Hello");

[1, 2].forEach(alert);
```

No need to think about the meaning of the brackets `[]` and `forEach` yet. We'll study them later. For now, just remember the result of running the code: it shows `Hello`, then `1`, then `2`.

Now let's remove the semicolon after the `alert`:

```js run no-beautify
alert("Hello")

[1, 2].forEach(alert);
```

The difference compared to the code above is only one character: the semicolon at the end of the first line is gone.

If we run this code, only the first `Hello` shows (and there's an error, you may need to open the console to see it). There are no numbers any more.

That's because JavaScript does not assume a semicolon before square brackets `[...]`. So, the code in the last example is treated as a single statement.

Here's how the engine sees it:

```js run no-beautify
alert("Hello")[1, 2].forEach(alert);
```

Looks weird, right? Such merging in this case is just wrong. We need to put a semicolon after `alert` for the code to work correctly.

This can happen in other situations also.
````

We recommend putting semicolons between statements even if they are separated by newlines. This rule is widely adopted by the community. Let's note once again -- *it is possible* to leave out semicolons most of the time. But it's safer -- especially for a beginner -- to use them.

> [!t] t: Exceptions to the ";": console, block statements, and StandardJS style.
>
> - "We recommend putting semicolons between statements even if they are
>   separated by newlines.".
> - This is a perfectly valid general advice, and jsinfo justifies with an
>   example the risk of not doing so. Having more ";" than strictly
>   necessary is generally safer than having less (the same happens with
>   parentheses).
> - But some exceptions can be seen.
> - Can be (optionally) avoided in the interactive console:
>   - The risk is less big there: the "Enter" key ends an statement input
>     (contrary to the source code, where, as shown, a statement can be
>     divided with new lines anywhere we could place a space).
>   - So you can afford in the console to ommit the ";" for inmmediateness.
>   - Just don't get used to it for code elsewhere!
> - Block statements do not end with ";":
>   - We will see later that there are statements that typically allow for
>     block statements: e.g. if's, loops, function declarations, ... specify
>     blocks of statements between `{}`. Inside these, each statement has its
>     ";"...
>   - ... But don't put ";" after the `{}` when you use them. In some cases it
>     will raise an error. Not in other cases, but it is still incorrect.
> - A famous JS style guide called
>   ["JS Standard Style" (StandardJS)](https://standardjs.com/) never uses
>   unnecessary ";" (after statements):
>   - StandardJS is a style guide that tries to end the "styling guide"
>     fights, proposing a standard reasonable way of styling JS, not
>     dependent on the project or the tastes of the programmers.
>   - It also provides a JS linter: extremely useful.
>   - But, among other things, it asks to never use the ";" unless in the
>     quite rare cases where it is needed (an example could be the one shown
>     by jsinfo). This was quite polemical.
>   - It arguments quite convincingly the reasons. In fact I completely
>     agree with them... only that they would apply for a fresh new world.
>   - You may encounter code with this style adopted by very clever and
>     competent people.
>   - But it is true that the use of ";" is de facto overwhemly adopted by
>     the JS community: the majority of style guides and JS usage in the
>     wild.
>   - StandarJS finally granted this fact, and they produced a variant
>     called **Semi**standardJS: exactly the same guide, but using ";"
>     (**semi**colons). Benefits of a unique style, without the unpopular
>     drawback of removing the ";".
>   - As with any attempt to standardize, here also may apply
>     ["how standards proliferate" (by XKCD)](https://m.xkcd.com/927/).
>   - Nonetheless, SemistandardJS can be a very sensible default to use in our
>     code when there is not a requirement to follow another specific guide (or
>     the code to which you contribute to does not use a clear and consistent
>     style). The argument for use it on a new contexts is: let's not loose
>     time deciding or arguing on this.
>   - A StandardJS/SemiStanardJS linter helps to follow these style guides
>     and also to catch some programming errors early. We'll see how we can
>     install and use it.
>   - **Warning:** do not confuse "StandardJS" (it's a **styling guide**) with
>     ES standards (the standardization of **the JS language** by standards
>     organizations)

## Comments [#code-comments]

As time goes on, programs become more and more complex. It becomes necessary to add *comments* which describe what the code does and why.

Comments can be put into any place of a script. They don't affect its execution because the engine simply ignores them.

**One-line comments start with two forward slash characters `//`.**

The rest of the line is a comment. It may occupy a full line of its own or follow a statement.

Like here:
```js run
// This comment occupies a line of its own
alert('Hello');

alert('World'); // This comment follows the statement
```

**Multiline comments start with a forward slash and an asterisk <code>/&#42;</code> and end with an asterisk and a forward slash <code>&#42;/</code>.**

Like this:

```js run
/* An example with two messages.
This is a multiline comment.
*/
alert('Hello');
alert('World');
```

The content of comments is ignored, so if we put code inside <code>/&#42; ... &#42;/</code>, it won't execute.

> [!t] t: Alternative names
>
> - "One-line comments" = line comments
> - "Multiline comments" = block comments

Sometimes it can be handy to temporarily disable a part of code:

```js run
/* Commenting out the code
alert('Hello');
*/
alert('World');
```

> [!t] t: Don't leave "deactivated" code in commited real code (professionally)
>
> - Imagine that you have this:
>
>   ```js
>   alert('A');
>
>   alert('B');
>
>   // A perfectly valid comment on the purpose of the following line:
>   alert('C');
>   ```
>
> - Then you "deactivate" some code:
>
>   ```js
>   alert('A');
>
>   // I used this once, I leave it here commented, just in case I may need
>   // it some day again:
>   //alert('B');
>
>   // A perfectly valid comment on the purpose of the following line:
>   alert('C');
>   ```
>
> - Before commiting to version control (production code), it should be
>   better deleted:
>  
>   ```js
>   alert('A');
>
>   // A perfectly valid comment on the purpose of the following line:
>   alert('C');
>   ```
>
> - If you ever need again the deleted code, you have the version control to
>   come back to the past. If you correctly used the commit messages, it
>   should be easy to locate the particular version that deleted it.

```smart header="Use hotkeys!"
In most editors, a line of code can be commented out by pressing the `key:Ctrl+/` hotkey for a single-line comment and something like `key:Ctrl+Shift+/` -- for multiline comments (select a piece of code and press the hotkey). For Mac, try `key:Cmd` instead of `key:Ctrl` and `key:Option` instead of `key:Shift`.
```

````warn header="Nested comments are not supported!"
There may not be `/*...*/` inside another `/*...*/`.

Such code will die with an error:

```js run no-beautify
/*
  /* nested comment ?!? */
*/
alert( 'World' );
```
````

> [!t] t: Advice: use line comments for comments spanning multiple lines
>
> - This is because they can be safely nested when needed (e.g. to
>   temporarily comment some code for troubleshooting or for testing
>   something).
> - By hand this can be painful: that's why it's recommend to learn your
>   editor's shortcuts for this.

Please, don't hesitate to comment your code.

> [!t] t: ¿More or less comments?
>
> - I fully agree with this advice, and specially while learning: your sin
>   will typically be insufficient comments and not too much comments.
> - However, too much comments may interfere with code legibility (break its
>   "reading flow", by puting related code too separated).
> - In these cases, you can use vscode's "Fold all block comments"
>   (`ctrl+k ctrl+/`).
> - It also works for line comments spanning multiple lines.
> - Unfold all if needed with `ctrl+k ctrl+j`.
> - There is an extension "Hide comments" to hide comments even more. But it
>   works somehow like CSS's `visibility: hidden;`: they still ocuppy their
>   place. It would be good a way of producing something like CSS's
>   `display: none;` (really disapearing).
> - As you gain good programming experience, you can produce code that speaks
>   better for itself (with the structure and naming decisions). Few comments
>   are needed in this case. But currently you are probably yet too far from
>   this nirvana. So: don't hesitate to comment your code.

Comments increase the overall code footprint, but that's not a problem at all. There are many tools which minify code before publishing to a production server. They remove comments, so they don't appear in the working scripts. Therefore, comments do not have negative effects on production at all.

Later in the tutorial there will be a chapter <info:code-quality> that also explains how to write better comments.

> [!t] t: Comment toggle trick (extra)
>
> - There is a hack/trick to toggle the activation of a block of code by
>   inserting/deleting a single character.
> - Deactivated:
>
>   ```js
>   /*
>
>    alert('A');
>
>    // */
>    ```
>
> - Add a `/` before the `/*` to activate (remove again to deactivate):
>
>   ```js
>   //*
>
>    alert('A');
>
>    // */
>    ```
>
> - There is also a variant to toggle between "A" and "B" versions of code.
> - "B" activated:
>
>   ```js
>   /*
>
>    alert('A');
>
>    /*/
>
>    alert('B');
>
>    // */
>    ```
>
> - Switch to "A" activated by preceding with a single `/` character:
>
>   ```js
>   //*
>
>    alert('A');
>
>    /*/
>
>    alert('B');
>
>    // */
>    ```
>
> - I may pesonally use this for teaching purposes: prepared demos/quick
>   proofs of concept/etc.
> - But, as seen in general for deactivating code with comments: don't use
>   this for real (production professional) code.
