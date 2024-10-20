# Basic operators, maths

We know many operators from school. They are things like addition `+`, multiplication `*`, subtraction `-`, and so on.

In this chapter, we’ll start with simple operators, then concentrate on JavaScript-specific aspects, not covered by school arithmetic.

> [!t] t: Operators happen inside expressions or concatenate them
>
> - Most (all?) of the operators (in general) happen inside expresions: they
>   evaluate producing a result, and they may be combined/concatenated.

## Terms: "unary", "binary", "operand"

Before we move on, let's grasp some common terminology.

- *An operand* -- is what operators are applied to. For instance, in the multiplication of `5 * 2` there are two operands: the left operand is `5` and the right operand is `2`. Sometimes, people call these "arguments" instead of "operands".
- An operator is *unary* if it has a single operand. For example, the unary negation `-` reverses the sign of a number:

    ```js run
    let x = 1;

    *!*
    x = -x;
    */!*
    alert( x ); // -1, unary negation was applied
    ```
- An operator is *binary* if it has two operands. The same minus exists in binary form as well:

    ```js run no-beautify
    let x = 1, y = 3;
    alert( y - x ); // 2, binary minus subtracts values
    ```

    Formally, in the examples above we have two different operators that share the same symbol: the negation operator, a unary operator that reverses the sign, and the subtraction operator, a binary operator that subtracts one number from another.

> [!t] t: There are also "ternary" operators
>
> - In fact "ternary operator" is a common name of the "conditional operator"
>   (`...?...:...`), that we will see later in the course.

## Maths

The following math operations are supported:

- Addition `+`,
- Subtraction `-`,
- Multiplication `*`,
- Division `/`,
- Remainder `%`,
- Exponentiation `**`.

> [!t] t: `**` works only on modern ES. Compatible alternative: `Math.pow()`.

The first four are straightforward, while `%` and `**` need a few words about them.

### Remainder %

The remainder operator `%`, despite its appearance, is not related to percents.

The result of `a % b` is the [remainder](https://en.wikipedia.org/wiki/Remainder) of the integer division of `a` by `b`.

For instance:

```js run
alert( 5 % 2 ); // 1, the remainder of 5 divided by 2
alert( 8 % 3 ); // 2, the remainder of 8 divided by 3
alert( 8 % 4 ); // 0, the remainder of 8 divided by 4
```

### Exponentiation **

The exponentiation operator `a ** b` raises `a` to the power of `b`.

In school maths, we write that as a<sup>b</sup>.

For instance:

```js run
alert( 2 ** 2 ); // 2² = 4
alert( 2 ** 3 ); // 2³ = 8
alert( 2 ** 4 ); // 2⁴ = 16
```

Just like in maths, the exponentiation operator is defined for non-integer numbers as well.

For example, a square root is an exponentiation by ½:

```js run
alert( 4 ** (1/2) ); // 2 (power of 1/2 is the same as a square root)
alert( 8 ** (1/3) ); // 2 (power of 1/3 is the same as a cubic root)
```


## String concatenation with binary +

Let's meet the features of JavaScript operators that are beyond school arithmetics.

Usually, the plus operator `+` sums numbers.

But, if the binary `+` is applied to strings, it merges (concatenates) them:

```js
let s = "my" + "string";
alert(s); // mystring
```

Note that if any of the operands is a string, then the other one is converted to a string too.

For example:

```js run
alert( '1' + 2 ); // "12"
alert( 2 + '1' ); // "21"
```

See, it doesn't matter whether the first operand is a string or the second one.

Here's a more complex example:

```js run
alert(2 + 2 + '1' ); // "41" and not "221"
```

Here, operators work one after another. The first `+` sums two numbers, so it returns `4`, then the next `+` adds the string `1` to it, so it's like `4 + '1' = '41'`.

```js run
alert('1' + 2 + 2); // "122" and not "14"
```
Here, the first operand is a string, the compiler treats the other two operands as strings too. The `2` gets concatenated to `'1'`, so it's like `'1' + 2 = "12"` and `"12" + 2 = "122"`.

The binary `+` is the only operator that supports strings in such a way. Other arithmetic operators work only with numbers and always convert their operands to numbers.

Here's the demo for subtraction and division:

```js run
alert( 6 - '2' ); // 4, converts '2' to a number
alert( '6' / '2' ); // 3, converts both operands to numbers
```

## Numeric conversion, unary +

The plus `+` exists in two forms: the binary form that we used above and the unary form.

The unary plus or, in other words, the plus operator `+` applied to a single value, doesn't do anything to numbers. But if the operand is not a number, the unary plus converts it into a number.

For example:

```js run
// No effect on numbers
let x = 1;
alert( +x ); // 1

let y = -2;
alert( +y ); // -2

*!*
// Converts non-numbers
alert( +true ); // 1
alert( +"" );   // 0
*/!*
```

It actually does the same thing as `Number(...)`, but is shorter.

The need to convert strings to numbers arises very often. For example, if we are getting values from HTML form fields, they are usually strings. What if we want to sum them?

The binary plus would add them as strings:

```js run
let apples = "2";
let oranges = "3";

alert( apples + oranges ); // "23", the binary plus concatenates strings
```

If we want to treat them as numbers, we need to convert and then sum them:

```js run
let apples = "2";
let oranges = "3";

*!*
// both values converted to numbers before the binary plus
alert( +apples + +oranges ); // 5
*/!*

// the longer variant
// alert( Number(apples) + Number(oranges) ); // 5
```

From a mathematician's standpoint, the abundance of pluses may seem strange. But from a programmer's standpoint, there's nothing special: unary pluses are applied first, they convert strings to numbers, and then the binary plus sums them up.

Why are unary pluses applied to values before the binary ones? As we're going to see, that's because of their *higher precedence*.

## Operator precedence

If an expression has more than one operator, the execution order is defined by their *precedence*, or, in other words, the default priority order of operators.

From school, we all know that the multiplication in the expression `1 + 2 * 2` should be calculated before the addition. That's exactly the precedence thing. The multiplication is said to have *a higher precedence* than the addition.

Parentheses override any precedence, so if we're not satisfied with the default order, we can use them to change it. For example, write `(1 + 2) * 2`.

There are many operators in JavaScript. Every operator has a corresponding precedence number. The one with the larger number executes first. If the precedence is the same, the execution order is from left to right.

> [!t] t: Wrong: for some operators it is right to left (see below).

Here's an extract from the [precedence table](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence) (you don't need to remember this, but note that unary operators are higher than corresponding binary ones):

> [!t] t: "you don’t need to remember this"
>   - But the more you know, the more you will correctly read when no
>     parentheses are used by others...
>   - And the less redundant parentheses you will need to put in your code. But...
>   - Not all your readers will be as proficient with this as you. You
>     shouldn't write only for noobs, but neither only for elite
>     hackers... A reasonable compromise may be good here. Readability
>     is important.
>   - Bugs difficult to find may occur when we remove parenthesis or do
>     complex expressions "beyond our means" (as precedence is not
>     always intuitive).
>   - So, in doubt:
>     - When writing, better put too many parenthesis than too few (but don't
>       go crazy). An you can use intermediate steps with intermediate
>       variables to make things clearer.
>     - When reading alien code, confirm if needed if your assumptions are
>       correct (revise the precedence table, and/or put temporarily more
>       parenthesis and/or use intermediate steps/variables to check
>       that the result is the same).

| Precedence | Name | Sign |
|------------|------|------|
| ... | ... | ... |
| 14 | unary plus | `+` |
| 14 | unary negation | `-` |
| 13 | exponentiation | `**` |
| 12 | multiplication | `*` |
| 12 | division | `/` |
| 11 | addition | `+` |
| 11 | subtraction | `-` |
| ... | ... | ... |
| 2 | assignment | `=` |
| ... | ... | ... |

As we can see, the "unary plus" has a priority of `14` which is higher than the `11` of "addition" (binary plus). That's why, in the expression `"+apples + +oranges"`, unary pluses work before the addition.

> [!t] t: Full operator precedence table from MDN
>
> The "precedence table" article linked above has before the table a
> very pegagogic explanation about operators precedence.
>
> The full table does an spoiler of things we will see later in the
> course, but this is a good place to have it.
>
> I'm transcribing next the full section regarding the precedence table,
> in order to have it here at hand:
>
> ### Table
>
> The following table lists operators in order from highest precedence (18) to lowest precedence (1).
>
> Several general notes about the table:
>
> 1. Not all syntax included here are "operators" in the strict sense. For example, spread `...` and arrow `=>` are typically not regarded as operators. However, we still included them to show how tightly they bind compared to other operators/expressions.
> 2. Some operators have certain operands that require expressions narrower than those produced by higher-precedence operators. For example, the right-hand side of member access `.` (precedence 17) must be an identifier instead of a grouped expression. The left-hand side of arrow `=>` (precedence 2) must be an arguments list or a single identifier instead of some random expression.
> 3. Some operators have certain operands that accept expressions wider than those produced by higher-precedence operators. For example, the bracket-enclosed expression of bracket notation `[ … ]` (precedence 17) can be any expression, even comma (precedence 1) joined ones. These operators act as if that operand is "automatically grouped". In this case we will omit the associativity.
>
> <table>
>   <tbody>
>     <tr>
>       <th>Precedence</th>
>       <th>Associativity</th>
>       <th>Individual operators</th>
>       <th>Notes</th>
>     </tr>
>     <tr>
>       <td>18: grouping</td>
>       <td>n/a</td>
>       <td>Grouping<br><code>(x)</code></td>
>       <td>[1]</td>
>     </tr>
>     <tr>
>       <td rowspan="6">17: access and call</td>
>       <td rowspan="2">left-to-right</td>
>       <td>Member access (dot notation)<br><code>x.y</code></td>
>       <td rowspan="2">[2]</td>
>     </tr>
>     <tr>
>       <td>Optional chaining<br><code>x?.y</code></td>
>     </tr>
>     <tr>
>       <td rowspan="4">n/a</td>
>       <td>Computed member access (bracket notation)<br><code>x[y]</code></td>
>       <td>[3]</td>
>     </tr>
>     <tr>
>       <td>new<br><code>new x(y)</code></td>
>       <td rowspan="3">[4]</td>
>     </tr>
>     <tr>
>       <td>Function call<br><code>x(y)</code></td>
>     </tr>
>     <tr>
>       <td>import(x)</td>
>     </tr>
>     <tr>
>       <td>16: new</td>
>       <td>n/a</td>
>       <td>new without argument list<br><code>new x</code></td>
>     </tr>
>     <tr>
>       <td rowspan="2">15: postfix operators</td>
>       <td rowspan="2">n/a</td>
>       <td>Postfix increment<br><code>x++</code></td>
>       <td rowspan="2">[5]</td>
>     </tr>
>     <tr>
>       <td>Postfix decrement<br><code>x--</code></td>
>     </tr>
>     <tr>
>       <td rowspan="10">14: prefix operators</td>
>       <td rowspan="10">n/a</td>
>       <td>Prefix increment<br><code>++x</code></td>
>       <td rowspan="2">[6]</td>
>     </tr>
>     <tr>
>       <td>Prefix decrement<br><code>--x</code></td>
>     </tr>
>     <tr>
>       <td>Logical NOT<br><code>!x</code></td>
>     </tr>
>     <tr>
>       <td>Bitwise NOT<br><code>~x</code></td>
>     </tr>
>     <tr>
>       <td>Unary plus<br><code>+x</code></td>
>     </tr>
>     <tr>
>       <td>Unary negation<br><code>-x</code></td>
>     </tr>
>     <tr>
>       <td>typeof x</td>
>     </tr>
>     <tr>
>       <td>void x</td>
>     </tr>
>     <tr>
>       <td>delete x</td>
>       <td>[7]</td>
>     </tr>
>     <tr>
>       <td>await x</td>
>     </tr>
>     <tr>
>       <td>13: exponentiation</td>
>       <td>right-to-left</td>
>       <td>Exponentiation<br><code>x ** y</code></td>
>       <td>[8]</td>
>     </tr>
>     <tr>
>       <td rowspan="3">12: multiplicative operators</td>
>       <td rowspan="3">left-to-right</td>
>       <td>Multiplication<br><code>x * y</code></td>
>     </tr>
>     <tr>
>       <td>Division<br><code>x / y</code></td>
>     </tr>
>     <tr>
>       <td>Remainder<br><code>x % y</code></td>
>     </tr>
>     <tr>
>       <td rowspan="2">11: additive operators</td>
>       <td rowspan="2">left-to-right</td>
>       <td>Addition<br><code>x + y</code></td>
>     </tr>
>     <tr>
>       <td>Subtraction<br><code>x - y</code></td>
>     </tr>
>     <tr>
>       <td rowspan="3">10: bitwise shift</td>
>       <td rowspan="3">left-to-right</td>
>       <td>Left shift<br><code>x &#x3C;&#x3C; y</code></td>
>     </tr>
>     <tr>
>       <td>Right shift<br><code>x >> y</code></td>
>     </tr>
>     <tr>
>       <td>Unsigned right shift<br><code>x >>> y</code></td>
>     </tr>
>     <tr>
>       <td rowspan="6">9: relational operators</td>
>       <td rowspan="6">left-to-right</td>
>       <td>Less than<br><code>x &#x3C; y</code></td>
>     </tr>
>     <tr>
>       <td>Less than or equal<br><code>x &#x3C;= y</code></td>
>     </tr>
>     <tr>
>       <td>Greater than<br><code>x > y</code></td>
>     </tr>
>     <tr>
>       <td>Greater than or equal<br><code>x >= y</code></td>
>     </tr>
>     <tr>
>       <td>x in y</td>
>     </tr>
>     <tr>
>       <td>x instanceof y</td>
>     </tr>
>     <tr>
>       <td rowspan="4">8: equality operators</td>
>       <td rowspan="4">left-to-right</td>
>       <td>Equality<br><code>x == y</code></td>
>     </tr>
>     <tr>
>       <td>Inequality<br><code>x != y</code></td>
>     </tr>
>     <tr>
>       <td>Strict equality<br><code>x === y</code></td>
>     </tr>
>     <tr>
>       <td>Strict inequality<br><code>x !== y</code></td>
>     </tr>
>     <tr>
>       <td>7: bitwise AND</td>
>       <td>left-to-right</td>
>       <td>Bitwise AND<br><code>x &#x26; y</code></td>
>     </tr>
>     <tr>
>       <td>6: bitwise XOR</td>
>       <td>left-to-right</td>
>       <td>Bitwise XOR<br><code>x ^ y</code></td>
>     </tr>
>     <tr>
>       <td>5: bitwise OR</td>
>       <td>left-to-right</td>
>       <td>Bitwise OR<br><code>x | y</code></td>
>     </tr>
>     <tr>
>       <td>4: logical AND</td>
>       <td>left-to-right</td>
>       <td>Logical AND<br><code>x &#x26;&#x26; y</code></td>
>     </tr>
>     <tr>
>       <td rowspan="2">3: logical OR, nullish coalescing</td>
>       <td rowspan="2">left-to-right</td>
>       <td>Logical OR<br><code>x || y</code></td>
>     </tr>
>     <tr>
>       <td>Nullish coalescing operator<br><code>x ?? y</code></td>
>       <td>[9]</td>
>     </tr>
>     <tr>
>       <td rowspan="21">2: assignment and miscellaneous</td>
>       <td rowspan="16">right-to-left</td>
>       <td>Assignment<br><code>x = y</code></td>
>       <td rowspan="16">[10]</td>
>     </tr>
>     <tr>
>       <td>Addition assignment<br><code>x += y</code></td>
>     </tr>
>     <tr>
>       <td>Subtraction assignment<br><code>x -= y</code></td>
>     </tr>
>     <tr>
>       <td>Exponentiation assignment<br><code>x **= y</code></td>
>     </tr>
>     <tr>
>       <td>Multiplication assignment<br><code>x *= y</code></td>
>     </tr>
>     <tr>
>       <td>Division assignment<br><code>x /= y</code></td>
>     </tr>
>     <tr>
>       <td>Remainder assignment<br><code>x %= y</code></td>
>     </tr>
>     <tr>
>       <td>Left shift assignment<br><code>x &#x3C;&#x3C;= y</code></td>
>     </tr>
>     <tr>
>       <td>Right shift assignment<br><code>x >>= y</code></td>
>     </tr>
>     <tr>
>       <td>Unsigned right shift assignment<br><code>x >>>= y</code></td>
>     </tr>
>     <tr>
>       <td>Bitwise AND assignment<br><code>x &#x26;= y</code></td>
>     </tr>
>     <tr>
>       <td>Bitwise XOR assignment<br><code>x ^= y</code></td>
>     </tr>
>     <tr>
>       <td>Bitwise OR assignment<br><code>x |= y</code></td>
>     </tr>
>     <tr>
>       <td>Logical AND assignment<br><code>x &#x26;&#x26;= y</code></td>
>     </tr>
>     <tr>
>       <td>Logical OR assignment<br><code>x ||= y</code></td>
>     </tr>
>     <tr>
>       <td>Nullish coalescing assignment<br><code>x ??= y</code></td>
>     </tr>
>     <tr>
>       <td>right-to-left</td>
>       <td>Conditional (ternary) operator<br><code>x ? y : z</code></td>
>       <td>[11]</td>
>     </tr>
>     <tr>
>       <td>right-to-left</td>
>       <td>Arrow<br><code>x => y</code></td>
>       <td>[12]</td>
>     </tr>
>     <tr>
>       <td rowspan="3">n/a</td>
>       <td>yield x</td>
>     </tr>
>     <tr>
>       <td>yield* x</td>
>     </tr>
>     <tr>
>       <td>Spread<br><code>...x</code></td>
>       <td>[13]</td>
>     </tr>
>     <tr>
>       <td>1: comma</td>
>       <td>left-to-right</td>
>       <td>Comma operator<br><code>x, y</code></td>
>     </tr>
>   </tbody>
> </table>
>
> Notes:
>
> 1. The operand can be any expression.
> 2. The "right-hand side" must be an identifier.
> 3. The "right-hand side" can be any expression.
> 4. The "right-hand side" is a comma-separated list of any expression with precedence > 1 (i.e. not comma expressions). The constructor of a `new` expression cannot be an optional chain.
> 5. The operand must be a valid assignment target (identifier or property access). Its precedence means `new Foo++` is `(new Foo)++` (a syntax error) and not `new (Foo++)` (a TypeError: (Foo++) is not a constructor).
> 6. The operand must be a valid assignment target (identifier or property access).
> 7. The operand cannot be an identifier or a [private property](/en-US/docs/Web/JavaScript/Reference/Classes/Private_properties) access.
> 8. The left-hand side cannot have precedence 14.
> 9. The operands cannot be a logical OR `||` or logical AND `&&` operator without grouping.
> 10. The "left-hand side" must be a valid assignment target (identifier or property access).
> 11. The associativity means the two expressions after `?` are implicitly grouped.
> 12. The "left-hand side" is a single identifier or a parenthesized parameter list.
> 13. Only valid inside object literals, array literals, or argument lists.
>
> The precedence of groups 17 and 16 may be a bit ambiguous. Here are a few examples to clarify.
>
> - Optional chaining is always substitutable for its respective syntax without optionality (barring a few special cases where optional chaining is forbidden). For example, any place that accepts `a?.b` also accepts `a.b` and vice versa, and similarly for `a?.()`, `a()`, etc.
> - Member expressions and computed member expressions are always substitutable for each other.
> - Call expressions and `import()` expressions are always substitutable for each other.
> - This leaves four classes of expressions: member access, `new` with arguments, function call, and `new` without arguments.
>   - The "left-hand side" of a member access can be: a member access (`a.b.c`), `new` with arguments (`new a().b`), and function call (`a().b`).
>   - The "left-hand side" of `new` with arguments can be: a member access (`new a.b()`) and `new` with arguments (`new new a()()`).
>   - The "left-hand side" of a function call can be: a member access (`a.b()`), `new` with arguments (`new a()()`), and function call (`a()()`).
>   - The operand of `new` without arguments can be: a member access (`new a.b`), `new` with arguments (`new new a()`), and `new` without arguments (`new new a`).
>

> [!t] t: Summary of operators precedence
>
> - I'm omiting here some less used operators, to make a summary.
> - Evaluation order:
>   - Most are left to right (or n/a).
>   - I note only the exceptions.
> - From higher to lower priority:
>   - Grouping (parentheses)
>   - Object members and function calls.
>   - Most unary operators:
>     - Postfix
>     - Prefix
>   - Mathematical:
>     - `**` (**right to left**)
>     - `*`, `/`, `%`
>     - `+`, `-`
>   - Relations: `<`, `>`, etc
>   - Equality/inequality: `===`, `!==`, etc
>   - Logical:
>     - AND: `&&`
>     - OR: `||`
>   - Assignments (`=`, `+=`, etc), ternary `? : `, arrow `=>`,
>     spread `...x` (all these are **right to left**)
>   - Comma (concatenation): `, `
> - Different operators can be at the same priority level,
>   so when mixed at the same level they have the same strength,
>   and their evaluation order wins there.

## Assignment

Let's note that an assignment `=` is also an operator. It is listed in the precedence table with the very low priority of `2`.

> [!t] t: "= is also an operator"
>
> - I.e. `=` **produces an expression**.
> - But `let` is a pure statement, **does not produce an expression**.
>
>   ```js
>   // Error!:
>   alert(let a=3);
>   // Error!:
>   alert(let a=3;);
>   // But this does work:
>   let a;
>   alert(a=3);
>   // And sets the value besides returning it:
>   alert(a);
>   ```

That's why, when we assign a variable, like `x = 2 * 2 + 1`, the calculations are done first and then the `=` is evaluated, storing the result in `x`.

```js
let x = 2 * 2 + 1;

alert( x ); // 5
```

> [!t] t: "`=` is evaluated", but right to left
>
> - As we mentioned above.
> - E.g.: `x = y = 2 + 1;` (see also section below)

### Assignment = returns a value

The fact of `=` being an operator, not a "magical" language construct has an interesting implication.

All operators in JavaScript return a value. That's obvious for `+` and `-`, but also true for `=`.

The call `x = value` writes the `value` into `x` *and then returns it*.

Here's a demo that uses an assignment as part of a more complex expression:

```js run
let a = 1;
let b = 2;

*!*
let c = 3 - (a = b + 1);
*/!*

alert( a ); // 3
alert( c ); // 0
```

In the example above, the result of expression `(a = b + 1)` is the value which was assigned to `a` (that is `3`). It is then used for further evaluations.

Funny code, isn't it? We should understand how it works, because sometimes we see it in JavaScript libraries.

Although, please don't write the code like that. Such tricks definitely don't make code clearer or readable.

### Chaining assignments

Another interesting feature is the ability to chain assignments:

```js run
let a, b, c;

*!*
a = b = c = 2 + 2;
*/!*

alert( a ); // 4
alert( b ); // 4
alert( c ); // 4
```

Chained assignments evaluate from right to left. First, the rightmost expression `2 + 2` is evaluated and then assigned to the variables on the left: `c`, `b` and `a`. At the end, all the variables share a single value.

Once again, for the purposes of readability it's better to split such code into few lines:

```js
c = 2 + 2;
b = c;
a = c;
```
That's easier to read, especially when eye-scanning the code fast.

## Modify-in-place

We often need to apply an operator to a variable and store the new result in that same variable.

For example:

```js
let n = 2;
n = n + 5;
n = n * 2;
```

This notation can be shortened using the operators `+=` and `*=`:

```js run
let n = 2;
n += 5; // now n = 7 (same as n = n + 5)
n *= 2; // now n = 14 (same as n = n * 2)

alert( n ); // 14
```

> [!t] t: Warning
>
> - Even if e.g. `+=` is an operator (produces an expression), it cannot be
>   used if the variable is not already defined/set with something.
> - So this doesn't work:
>
>   ```js
>   // Error!:
>   let a+=3;
>   ```
>
> - But this "works" (although possibly not nice):
>
>   ```js
>   let a;
>   a+=3;
>   // Gives NaN: adding 3 to "undefined".
>   ```

Short "modify-and-assign" operators exist for all arithmetical and bitwise operators: `/=`, `-=`, etc.

Such operators have the same precedence as a normal assignment, so they run after most other calculations:

```js run
let n = 2;

n *= 3 + 5; // right part evaluated first, same as n *= 8

alert( n ); // 16
```

## Increment/decrement

<!-- Can't use -- in title, because the built-in parser turns it into a 'long dash' – -->

Increasing or decreasing a number by one is among the most common numerical operations.

So, there are special operators for it:

- **Increment** `++` increases a variable by 1:

    ```js run no-beautify
    let counter = 2;
    counter++;        // works the same as counter = counter + 1, but is shorter
    alert( counter ); // 3
    ```
- **Decrement** `--` decreases a variable by 1:

    ```js run no-beautify
    let counter = 2;
    counter--;        // works the same as counter = counter - 1, but is shorter
    alert( counter ); // 1
    ```

```warn
Increment/decrement can only be applied to variables. Trying to use it on a value like `5++` will give an error.
```

The operators `++` and `--` can be placed either before or after a variable.

- When the operator goes after the variable, it is in "postfix form": `counter++`.
- The "prefix form" is when the operator goes before the variable: `++counter`.

Both of these statements do the same thing: increase `counter` by `1`.

Is there any difference? Yes, but we can only see it if we use the returned value of `++/--`.

Let's clarify. As we know, all operators return a value. Increment/decrement is no exception. The prefix form returns the new value while the postfix form returns the old value (prior to increment/decrement).

To see the difference, here's an example:

```js run
let counter = 1;
let a = ++counter; // (*)

alert(a); // *!*2*/!*
```

In the line `(*)`, the *prefix* form `++counter` increments `counter` and returns the new value, `2`. So, the `alert` shows `2`.

Now, let's use the postfix form:

```js run
let counter = 1;
let a = counter++; // (*) changed ++counter to counter++

alert(a); // *!*1*/!*
```

In the line `(*)`, the *postfix* form `counter++` also increments `counter` but returns the *old* value (prior to increment). So, the `alert` shows `1`.

To summarize:

- If the result of increment/decrement is not used, there is no difference in which form to use:

    ```js run
    let counter = 0;
    counter++;
    ++counter;
    alert( counter ); // 2, the lines above did the same
    ```
- If we'd like to increase a value *and* immediately use the result of the operator, we need the prefix form:

    ```js run
    let counter = 0;
    alert( ++counter ); // 1
    ```
- If we'd like to increment a value but use its previous value, we need the postfix form:

    ```js run
    let counter = 0;
    alert( counter++ ); // 0
    ```

````smart header="Increment/decrement among other operators"
The operators `++/--` can be used inside expressions as well. Their precedence is higher than most other arithmetical operations.

For instance:

```js run
let counter = 1;
alert( 2 * ++counter ); // 4
```

Compare with:

```js run
let counter = 1;
alert( 2 * counter++ ); // 2, because counter++ returns the "old" value
```

Though technically okay, such notation usually makes code less readable. One line does multiple things -- not good.

While reading code, a fast "vertical" eye-scan can easily miss something like `counter++` and it won't be obvious that the variable increased.

We advise a style of "one line -- one action":

```js run
let counter = 1;
alert( 2 * counter );
counter++;
```
````

## Bitwise operators

Bitwise operators treat arguments as 32-bit integer numbers and work on the level of their binary representation.

These operators are not JavaScript-specific. They are supported in most programming languages.

The list of operators:

- AND ( `&` )
- OR ( `|` )
- XOR ( `^` )
- NOT ( `~` )
- LEFT SHIFT ( `<<` )
- RIGHT SHIFT ( `>>` )
- ZERO-FILL RIGHT SHIFT ( `>>>` )

These operators are used very rarely, when we need to fiddle with numbers on the very lowest (bitwise) level. We won't need these operators any time soon, as web development has little use of them, but in some special areas, such as cryptography, they are useful. You can read the [Bitwise Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#bitwise_operators) chapter on MDN when a need arises.

## Comma

The comma operator `,` is one of the rarest and most unusual operators. Sometimes, it's used to write shorter code, so we need to know it in order to understand what's going on.

The comma operator allows us to evaluate several expressions, dividing them with a comma `,`. Each of them is evaluated but only the result of the last one is returned.

For example:

```js run
*!*
let a = (1 + 2, 3 + 4);
*/!*

alert( a ); // 7 (the result of 3 + 4)
```

Here, the first expression `1 + 2` is evaluated and its result is thrown away. Then, `3 + 4` is evaluated and returned as the result.

```smart header="Comma has a very low precedence"
Please note that the comma operator has very low precedence, lower than `=`, so parentheses are important in the example above.

Without them: `a = 1 + 2, 3 + 4` evaluates `+` first, summing the numbers into `a = 3, 7`, then the assignment operator `=` assigns `a = 3`, and the rest is ignored. It's like `(a = 1 + 2), 3 + 4`.
```

Why do we need an operator that throws away everything except the last expression?

Sometimes, people use it in more complex constructs to put several actions in one line.

For example:

```js
// three operations in one line
for (*!*a = 1, b = 3, c = a * b*/!*; a < 10; a++) {
 ...
}
```

Such tricks are used in many JavaScript frameworks. That's why we're mentioning them. But usually they don't improve code readability so we should think well before using them.
