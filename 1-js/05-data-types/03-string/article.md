# Strings

In JavaScript, the textual data is stored as strings. There is no separate type for a single character.

The internal format for strings is always [UTF-16](https://en.wikipedia.org/wiki/UTF-16), it is not tied to the page encoding.

> [!t] t: The page enconding is typically UTF-8
>
> - So the encoding of the JS code we run, the HTML and the CSS is
>   generally in UTF-8. UTF-16 is what is used internally in the JS
>   engine (using conversions as needed).
> - In general, also use UTF-8 whenever possible for external
>   communications (AJAX, cookies, form fields, SQL queries if you rare
>   in node.js, ...). Exceptions: in particular cases where a
>   different one is expected.

## Quotes

Let's recall the kinds of quotes.

Strings can be enclosed within either single quotes, double quotes or backticks:

```js
let single = 'single-quoted';
let double = "double-quoted";

let backticks = `backticks`;
```

Single and double quotes are essentially the same. Backticks, however, allow us to embed any expression into the string, by wrapping it in `${…}`:

```js run
function sum(a, b) {
  return a + b;
}

alert(`1 + 2 = ${sum(1, 2)}.`); // 1 + 2 = 3.
```

Another advantage of using backticks is that they allow a string to span multiple lines:

```js run
let guestList = `Guests:
 * John
 * Pete
 * Mary
`;

alert(guestList); // a list of guests, multiple lines
```

Looks natural, right? But single or double quotes do not work this way.

If we use them and try to use multiple lines, there'll be an error:

```js run
let guestList = "Guests: // Error: Unexpected token ILLEGAL
  * John";
```

Single and double quotes come from ancient times of language creation, when the need for multiline strings was not taken into account. Backticks appeared much later and thus are more versatile.

Backticks also allow us to specify a "template function" before the first backtick. The syntax is: <code>func&#96;string&#96;</code>. The function `func` is called automatically, receives the string and embedded expressions and can process them. This feature is called "tagged templates", it's rarely seen, but you can read about it in the MDN: [Template literals](mdn:/JavaScript/Reference/Template_literals#Tagged_templates).

## Special characters

It is still possible to create multiline strings with single and double quotes by using a so-called "newline character", written as `\n`, which denotes a line break:

```js run
let guestList = "Guests:\n * John\n * Pete\n * Mary";

alert(guestList); // a multiline list of guests, same as above
```

As a simpler example, these two lines are equal, just written differently:

```js run
let str1 = "Hello\nWorld"; // two lines using a "newline symbol"

// two lines using a normal newline and backticks
let str2 = `Hello
World`;

alert(str1 == str2); // true
```

There are other, less common special characters:

| Character | Description |
|-----------|-------------|
|`\n`|New line|
|`\r`|In Windows text files a combination of two characters `\r\n` represents a new break, while on non-Windows OS it's just `\n`. That's for historical reasons, most Windows software also understands `\n`. |
|`\'`,&nbsp;`\"`,&nbsp;<code>\\`</code>|Quotes|
|`\\`|Backslash|
|`\t`|Tab|
|`\b`, `\f`, `\v`| Backspace, Form Feed, Vertical Tab -- mentioned for completeness, coming from old times, not used nowadays (you can forget them right now). |

As you can see, all special characters start with a backslash character `\`. It is also called an "escape character".

Because it's so special, if we need to show an actual backslash `\` within the string, we need to double it:

```js run
alert( `The backslash: \\` ); // The backslash: \
```

So-called "escaped" quotes `\'`, `\"`, <code>\\`</code> are used to insert a quote into the same-quoted string.

For instance:

```js run
alert( 'I*!*\'*/!*m the Walrus!' ); // *!*I'm*/!* the Walrus!
```

As you can see, we have to prepend the inner quote by the backslash `\'`, because otherwise it would indicate the string end.

Of course, only the quotes that are the same as the enclosing ones need to be escaped. So, as a more elegant solution, we could switch to double quotes or backticks instead:

```js run
alert( "I'm the Walrus!" ); // I'm the Walrus!
```

Besides these special characters, there's also a special notation for Unicode codes `\u…`, it's rarely used and is covered in the optional chapter about [Unicode](info:unicode).

## String length

The `length` property has the string length:

```js run
alert( `My\n`.length ); // 3
```

Note that `\n` is a single "special" character, so the length is indeed `3`.

```warn header="`length` is a property"
People with a background in some other languages sometimes mistype by calling `str.length()` instead of just `str.length`. That doesn't work.

Please note that `str.length` is a numeric property, not a function. There is no need to add parenthesis after it. Not `.length()`, but `.length`.
```

## Accessing characters

To get a character at position `pos`, use square brackets `[pos]` or call the method [str.at(pos)](mdn:js/String/at). The first character starts from the zero position:

```js run
let str = `Hello`;

// the first character
alert( str[0] ); // H
alert( str.at(0) ); // H

// the last character
alert( str[str.length - 1] ); // o
alert( str.at(-1) );
```

As you can see, the `.at(pos)` method has a benefit of allowing negative position. If `pos` is negative, then it's counted from the end of the string.

So `.at(-1)` means the last character, and `.at(-2)` is the one before it, etc.

The square brackets always return `undefined` for negative indexes, for instance:

```js run
let str = `Hello`;

alert( str[-2] ); // undefined
alert( str.at(-2) ); // l
```

We can also iterate over characters using `for..of`:

```js run
for (let char of "Hello") {
  alert(char); // H,e,l,l,o (char becomes "H", then "e", then "l" etc)
}
```

## Strings are immutable

Strings can't be changed in JavaScript. It is impossible to change a character.

Let's try it to show that it doesn't work:

```js run
let str = 'Hi';

str[0] = 'h'; // error
alert( str[0] ); // doesn't work
```

The usual workaround is to create a whole new string and assign it to `str` instead of the old one.

For instance:

```js run
let str = 'Hi';

str = 'h' + str[1]; // replace the string

alert( str ); // hi
```

In the following sections we'll see more examples of this.

## Changing the case

Methods [toLowerCase()](mdn:js/String/toLowerCase) and [toUpperCase()](mdn:js/String/toUpperCase) change the case:

```js run
alert( 'Interface'.toUpperCase() ); // INTERFACE
alert( 'Interface'.toLowerCase() ); // interface
```

Or, if we want a single character lowercased:

```js run
alert( 'Interface'[0].toLowerCase() ); // 'i'
```

> [!t] t: String is not affected in place (they are immutable)
>
> - To change a variable, we must reassign the output to it.
> - For example:
>
>   ```js
>   let str='hello';
>   str = str.toUpperCase();
>   ```
> - Similar thing applies to any string method: the string
>   itself is unaffected.

## Searching for a substring

There are multiple ways to look for a substring within a string.

### str.indexOf

The first method is [str.indexOf(substr, pos)](mdn:js/String/indexOf).

It looks for the `substr` in `str`, starting from the given position `pos`, and returns the position where the match was found or `-1` if nothing can be found.

> [!t] t: position is an index (starts with 0), **included**

For instance:

```js run
let str = 'Widget with id';

alert( str.indexOf('Widget') ); // 0, because 'Widget' is found at the beginning
alert( str.indexOf('widget') ); // -1, not found, the search is case-sensitive

alert( str.indexOf("id") ); // 1, "id" is found at the position 1 (..idget with id)
```

The optional second parameter allows us to start searching from a given position.

For instance, the first occurrence of `"id"` is at position `1`. To look for the next occurrence, let's start the search from position `2`:

```js run
let str = 'Widget with id';

alert( str.indexOf('id', 2) ) // 12
```

If we're interested in all occurrences, we can run `indexOf` in a loop. Every new call is made with the position after the previous match:

```js run
let str = 'As sly as a fox, as strong as an ox';

let target = 'as'; // let's look for it

let pos = 0;
while (true) {
  let foundPos = str.indexOf(target, pos);
  if (foundPos == -1) break;

  alert( `Found at ${foundPos}` );
  pos = foundPos + 1; // continue the search from the next position
}
```

The same algorithm can be layed out shorter:

```js run
let str = "As sly as a fox, as strong as an ox";
let target = "as";

*!*
let pos = -1;
while ((pos = str.indexOf(target, pos + 1)) != -1) {
  alert( pos );
}
*/!*
```

```smart header="`str.lastIndexOf(substr, position)`"
There is also a similar method [str.lastIndexOf(substr, position)](mdn:js/String/lastIndexOf) that searches from the end of a string to its beginning.

It would list the occurrences in the reverse order.
```

There is a slight inconvenience with `indexOf` in the `if` test. We can't put it in the `if` like this:

```js run
let str = "Widget with id";

if (str.indexOf("Widget")) {
    alert("We found it"); // doesn't work!
}
```

The `alert` in the example above doesn't show because `str.indexOf("Widget")` returns `0` (meaning that it found the match at the starting position). Right, but `if` considers `0` to be `false`.

So, we should actually check for `-1`, like this:

```js run
let str = "Widget with id";

*!*
if (str.indexOf("Widget") != -1) {
*/!*
    alert("We found it"); // works now!
}
```

### includes, startsWith, endsWith

The more modern method [str.includes(substr, pos)](mdn:js/String/includes) returns `true/false` depending on whether `str` contains `substr` within.

> [!t] t: pos is an index (starts with 0), **included**

It's the right choice if we need to test for the match, but don't need its position:

```js run
alert( "Widget with id".includes("Widget") ); // true

alert( "Hello".includes("Bye") ); // false
```

The optional second argument of `str.includes` is the position to start searching from:

```js run
alert( "Widget".includes("id") ); // true
alert( "Widget".includes("id", 3) ); // false, from position 3 there is no "id"
```

The methods [str.startsWith](mdn:js/String/startsWith) and [str.endsWith](mdn:js/String/endsWith) do exactly what they say:

```js run
alert( "*!*Wid*/!*get".startsWith("Wid") ); // true, "Widget" starts with "Wid"
alert( "Wid*!*get*/!*".endsWith("get") ); // true, "Widget" ends with "get"
```

## Getting a substring

There are 3 methods in JavaScript to get a substring: `substring`, `substr` and `slice`.

`str.slice(start [, end])`
: Returns the part of the string from `start` to (but not including) `end`.

> [!t] t: Both are index (start with 0). start **included**, end **exluded**

    For instance:

    ```js run
    let str = "stringify";
    alert( str.slice(0, 5) ); // 'strin', the substring from 0 to 5 (not including 5)
    alert( str.slice(0, 1) ); // 's', from 0 to 1, but not including 1, so only character at 0
    ```

    If there is no second argument, then `slice` goes till the end of the string:

    ```js run
    let str = "st*!*ringify*/!*";
    alert( str.slice(2) ); // 'ringify', from the 2nd position till the end
    ```

    Negative values for `start/end` are also possible. They mean the position is counted from the string end:

    ```js run
    let str = "strin*!*gif*/!*y";

    // start at the 4th position from the right, end at the 1st from the right
    alert( str.slice(-4, -1) ); // 'gif'
    ```

`str.substring(start [, end])`
: Returns the part of the string *between* `start` and `end` (not including `end`).

    This is almost the same as `slice`, but it allows `start` to be greater than `end` (in this case it simply swaps `start` and `end` values).

    > [!t] t: slice in that case returns empty string instead.

    For instance:

    ```js run
    let str = "st*!*ring*/!*ify";

    // these are same for substring
    alert( str.substring(2, 6) ); // "ring"
    alert( str.substring(6, 2) ); // "ring"

    // ...but not for slice:
    alert( str.slice(2, 6) ); // "ring" (the same)
    alert( str.slice(6, 2) ); // "" (an empty string)

    ```

    Negative arguments are (unlike slice) not supported, they are treated as `0`.

`str.substr(start [, length])`
: Returns the part of the string from `start`, with the given `length`.

    In contrast with the previous methods, this one allows us to specify the `length` instead of the ending position:

    ```js run
    let str = "st*!*ring*/!*ify";
    alert( str.substr(2, 4) ); // 'ring', from the 2nd position get 4 characters
    ```

    The first argument may be negative, to count from the end:

    ```js run
    let str = "strin*!*gi*/!*fy";
    alert( str.substr(-4, 2) ); // 'gi', from the 4th position get 2 characters
    ```

    This method resides in the [Annex B](https://tc39.es/ecma262/#sec-string.prototype.substr) of the language specification. It means that only browser-hosted Javascript engines should support it, and it's not recommended to use it. In practice, it's supported everywhere.

Let's recap these methods to avoid any confusion:

| method | selects... | negatives |
|--------|-----------|-----------|
| `slice(start, end)` | from `start` to `end` (not including `end`) | allows negatives |
| `substring(start, end)` | between `start` and `end` (not including `end`)| negative values mean `0` |
| `substr(start, length)` | from `start` get `length` characters | allows negative `start` |

```smart header="Which one to choose?"
All of them can do the job. Formally, `substr` has a minor drawback: it is described not in the core JavaScript specification, but in Annex B, which covers browser-only features that exist mainly for historical reasons. So, non-browser environments may fail to support it. But in practice it works everywhere.

Of the other two variants, `slice` is a little bit more flexible, it allows negative arguments and shorter to write.

So, for practical use it's enough to remember only `slice`.
```

## Comparing strings

As we know from the chapter <info:comparison>, strings are compared character-by-character in alphabetical order.

Although, there are some oddities.

1. A lowercase letter is always greater than the uppercase:

    ```js run
    alert( 'a' > 'Z' ); // true
    ```

2. Letters with diacritical marks are "out of order":

    ```js run
    alert( 'Österreich' > 'Zealand' ); // true
    ```

    This may lead to strange results if we sort these country names. Usually people would expect `Zealand` to come after `Österreich` in the list.

To understand what happens, we should be aware that strings in Javascript are encoded using [UTF-16](https://en.wikipedia.org/wiki/UTF-16). That is: each character has a corresponding numeric code.

There are special methods that allow to get the character for the code and back:

`str.codePointAt(pos)`
: Returns a decimal number representing the code for the character at position `pos`:

    ```js run
    // different case letters have different codes
    alert( "Z".codePointAt(0) ); // 90
    alert( "z".codePointAt(0) ); // 122
    alert( "z".codePointAt(0).toString(16) ); // 7a (if we need a hexadecimal value)
    ```

`String.fromCodePoint(code)`
: Creates a character by its numeric `code`

    ```js run
    alert( String.fromCodePoint(90) ); // Z
    alert( String.fromCodePoint(0x5a) ); // Z (we can also use a hex value as an argument)
    ```

Now let's see the characters with codes `65..220` (the latin alphabet and a little bit extra) by making a string of them:

```js run
let str = '';

for (let i = 65; i <= 220; i++) {
  str += String.fromCodePoint(i);
}
alert( str );
// Output:
// ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~
// ¡¢£¤¥¦§¨©ª«¬­®¯°±²³´µ¶·¸¹º»¼½¾¿ÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖ×ØÙÚÛÜ
```

See? Capital characters go first, then a few special ones, then lowercase characters, and `Ö` near the end of the output.

Now it becomes obvious why `a > Z`.

The characters are compared by their numeric code. The greater code means that the character is greater. The code for `a` (97) is greater than the code for `Z` (90).

- All lowercase letters go after uppercase letters because their codes are greater.
- Some letters like `Ö` stand apart from the main alphabet. Here, its code is greater than anything from `a` to `z`.

> [!t] t: Overview of "groups" of characters (and their order) in Unicode
>
> - Unicode point codes are grouped in [unicode
>   blocks](https://en.wikipedia.org/wiki/Unicode_block).
>   - The first block of 128 chars is called "Basic Latin". It
>     corresponds to the characters of ASCII standard (in their order).
>   - The next block of 128 chars is called "Latin-1 Supplement". It
>     corresponds of the second half of the Extended ASCII "ISO 8859-1",
>     the most popular Extended ASCII that contains most of the missing
>     chars for covering European most popular languages (common
>     accented chars, etc).
>   - Then come other blocks with different sizes: stargin with some for
>     complementing latin scripts even more, then others for covering
>     other scripts and representations.
> - We can look in more detail the first blocks in the
>   [list of unicode characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters):
>   - We see some "groups" of characters, and their order, which will
>     be the default sort order in JS.
>   - These groups in their order with some examples of contained
>     characters:
>     - Basic Latin
>       - 32 control chars: null character, line feed, carriage
>         return, horizontal tab, ... Many are not used anymore.
>       - 16 punctuation/symbols: space, !, (, ), ., +, -, /, ...
>       - 10 digits: 0, 1, ... 9
>       - 7 punctuation/symbols: :, ;, <, =, >, ?, @
>       - 26 uppercase letters: A, B, ..., Z
>       - 6 punctuation/symbols: [, \, ], ^, _, \`
>       - 26 lowercase letters: a, b, ..., z
>       - 4 punctuation/symbols: {, |, }, ~
>       - 1 control char: Delete
>     - Latin-1 Supplement
>       - 32 control characters: most are not used anymore.
>       - 32 punctuation/symbols: non-breaking space, ¡, ©, ®, ¿, ...
>       - 30 uppercase letters (most accented): ..., Á, ..., É, ..., Í, ... Ñ, ..., Ó, ..., Ú, ...
>         - 1 math symbol (inserted inside that group): ×
>       - 32 lowercase letters (most accented): ..., á, ..., é, ..., í, ... ñ, ..., ó, ..., ú, ...
>         - 1 math symbol (inserted inside that group): ÷
>     - So the default sort order of the groups is:
>       numbers, ASCII standard uppercase, ASCII standard lowercase,
>       ASCII extended uppercase, ASCII extended lowercase.
>     - And there are different punctuation and symbols around these
>       groups, with no particular logic. E.g. - and / are before
>       numbers, but _ and \ are between uppercase and lowercase
>       standard ASCII, and | is after uppercase standard ASCII...
> - BMP, UTF32, UTF-16 and UTF-8:
>   - Recent versions of Unicode have ~150 000 point codes (characters).
>     With UTF-32 (not used normally), they are coded each one with 32b
>     (4B), that leaves plenty of room (2^32 ~ 4 000 000 000).
>   - We've seen that the point codes are grouped in "blocks" (typically
>     representing different languages).
>   - Several blocks are grouped in "planes".
>   - The most important is the first plane, Plane 0, called BMP (Basic
>     Multilingual Plane).
>   - It covers the first 2^16 ~= 65000 code points.
>   - It covers the normally used languages in the world (including
>     Chinese, etc, that have each thousands of characters).
>   - UTF-16 codifies 1 char of BMP in 16b = 2B. For BMP characters (the
>     overwhelming majority), it is a fixed length encoding (as UTF-32),
>     but taking less place. This good compromise makes that many OS
>     (e.g. Windows) and languages (e.g. JS) code internaly in UTF-16.
>   - But UTF-16 is not restricted to the BMP. The characters outside
>     BMP simply take the double of space 16b+16b=32b, 2B+2B=4B, but
>     only this.
>   - In short: UTF-16 is a variable length encoding, but most of the
>     time a character in it uses 16b.
>   - UTF-8: uses 8b=1B if in the ASCII standard range (first Unicode
>     block of 128 chars). And increasingly more, up to 32b=4B for
>     increasingly far code points. E.g. ASCII extended take 2B, but
>     Chinese characters take 3B (contrary to 2B in UTF-16).
>   - In short: UTF-8 is a variable enconding but much more often
>     variable. Only fixed to 8b=1B when only ASCII standard is used.
>     - It takes less space than UTF-16 for many scripts.
>     - It takes more for Chinese and others.
>     - It is ASCII-compatible.
>     - In any case, it is most recommended for
>       communications/interoperativity (at least in a web techs
>       context).

### Correct comparisons [#correct-comparisons]

The "right" algorithm to do string comparisons is more complex than it may seem, because alphabets are different for different languages.

So, the browser needs to know the language to compare.

Luckily, modern browsers support the internationalization standard [ECMA-402](https://www.ecma-international.org/publications-and-standards/standards/ecma-402/).

It provides a special method to compare strings in different languages, following their rules.

The call [str.localeCompare(str2)](mdn:js/String/localeCompare) returns an integer indicating whether `str` is less, equal or greater than `str2` according to the language rules:

- Returns a negative number if `str` is less than `str2`.
- Returns a positive number if `str` is greater than `str2`.
- Returns `0` if they are equivalent.

> [!t] t: Read: `str.localeCompare(str2)` is positive ("true") if str comes after str2
>
> - Is negative ("false") if it comes before.
> - Is zero if same place.
>   - They can be in the "same place" even if different when we activate
>     insensitiveness (see config options below).

For instance:

```js run
alert( 'Österreich'.localeCompare('Zealand') ); // -1
```

> [!t] t: Don't count on -1 or +1: per spec, can be any negative or positive.

This method actually has two additional arguments specified in [the documentation](mdn:js/String/localeCompare), which allows it to specify the language (by default taken from the environment, letter order depends on the language) and setup additional rules like case sensitivity or should `"a"` and `"á"` be treated as the same etc.

> [!t] t: Important for non-english, but not straighforward to use.
>
> - Constructor
>   [`Intl.Collator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Collator/Collator):
>   - Provides an alternative way of doing locale comparison,
>     recommended when having to do "many comparisons": e.g.
>     sorting or filtering an array.
>   - One first instatiates an object with the desired
>     configuration. Something like
>     `collator = new Intl.Collator(/*[options]*/);`.
>   - Then, this object provides a method `collator.compare(str, str2)`
>     with same result than `str.localeCompare(str2)`.
> - There are 2 optional configuration parameters, similar in
>   both methods:
>   - locale (language/script/region/variant) specification
>   - further options.
> - With `str.localCompare()` they are placed, each time, after the
>   string to compare.
> - With `Intl.Collator` they are placed once in the constructor.
> - If no locale (language) is specified, by default it is taken from
>   the "environment".
> - One can check this default locale detected with:
>
>   ```js
>   (new Intl.Collator()).resolvedOptions().locale
>   ```
>
> - In the browser, this may be the locale of the
>   end-user, but I'm not sure that it is always like this.
> - Since there may be nuances in ordering among languages, we can
>   ensure the locale used with an optional parameter, for being sure of
>   the one that is used.
> - For example, to force spanish language:
>
>   ```js
>   // With non-locale comparison:
>   alert( 'zorro' > 'ámbar' );
>     // false: no good! zorro shouldn't be before ámbar.
>   // Locale compare with 'es' (spanish):
>   alert( 'zorro'.localeCompare('ámbar', 'es') );
>     // positive: good! zorro is after ámbar.
>   // Using Intl.Collator:
>   const collator = new Intl.Collator('es');
>   alert( collator.compare('zorro', 'ámbar') );
>     // positive: good! zorro is after ámbar.
>   // Note: With local 'en' (english):
>   alert( 'zorro'.localeCompare('ámbar', 'en') );
>     // also positive, as with locale 'es'. But we don't know if this
>     // can be generalized for every case. If we want to ensure that
>     // we compare "in the spanish way", we specify 'es'
>   ```
>
> - Further configuration can be made with an input "object" containing
>   options. We specify in the object the particular ones we want to
>   diverge from their default values.
> - [Options for the comparison](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Collator/Collator#options):
>   - "usage" can be one of:
>     - "sort" (default): for sorting (ordering).
>     - "search": optimized for searching (just knowing if matches or
>       not).
>   - "sensitivity" can be one of:
>     - "case": only case sensitive (but not accent sensitive):
>       a ≠ b, a = á, a ≠ A
>     - "accent": only accent sensitive (but not case sensitive):
>       a ≠ b, a ≠ á, a = A
>     - "variant": both case and accent sensitive (normal comparison):
>       a ≠ b, a ≠ á, a ≠ A 
>     - "base": both case and accent insensitive:
>       a ≠ b, a = á, a = A
>     - Default is "variant" (ultra-sensitive) for "sort". For "search",
>       it depends on the locale.
>   - "ignorePunctuation":
>     - Boolean, false by default.
>     - If true, it ignores any punctuation character when comparing:
>       "hellobye" = "hello bye" = "hello - bye" = "hello,._-/\n%bye"
>   - "numeric":
>     - Boolean, false by defult.
>     - Compares consecutive digits in numeric order (placing '10' after
>       '2').
>     - Does not handle well decimals.
>   - Other (see the reference).
> - Example of ultra-insensitive search:
>
>   ```js
>   const collator = new Intl.Collator(
>     'es',
>     { sensitivity: 'base', ignorePunctuation: true });
>   alert( collator.compare('Martínez-Polo', 'martinez polo') );
>     // 0 (equal)
>   // Note: could also be done with `str.localCompare()`, but each time
>   // one has to provide the config, and it may be less performant.
>   ```
>
> - Examples of numeric comparison
>
>   ```js
>   const collator = new Intl.Collator( 'en', { numeric: true });
>   alert( '10€' > '2€' );
>     // false, no good.
>   alert( collator.compare('10€', '2€') );
>     // positive, good
>   alert( '1.50€' > '1€' );
>     // false, no good
>   alert( collator.compare('1.50€', '1€') );
>     // negative, no good! Needed '1.00€' to work.
>   alert( collator.compare('$1.50', '$1') );
>     // positive, good... but needed to **end** with numbers
>   ```
>
> - Example of locale+numeric comparison, showing that the numbers
>   inside a string are considered as a whole before advancing to the
>   following character:
>
>   ```js
>   const collEs = new Intl.Collator('es');
>   const collEsNum = new Intl.Collator('es', { numeric: true });
>   const arr = ['Z', 'a', 2, 'z', 10, 'á', 1, 'a10b', 'a2b', 'a2', 'a2a',
>     'aa', 'ab', 'a11a'];
>   alert( arr.sort(collEs.compare)+'' );
>     // "1,10,2,a,á,a10b,a11a,a2,a2a,a2b,aa,ab,z,Z"
>   alert( arr.sort(collEsNum.compare)+'' );
>     // "1,2,10,a,á,a2,a2a,a2b,a10b,a11a,aa,ab,z,Z"
>   // Note: casting to string with +'' to avoid ... and need to unfold.
>   ```
>
> - Browser compatibility:
>   - Intl related stuff may not have universal support. E.g.
>     [`Intl.Collator` support](https://caniuse.com/mdn-javascript_builtins_intl_collator)
>     is 97% as of 2024 (and it is also needed for using options in
>     str.localeCompare). Gracefull degradation and/or polyfill could be
>     considered here.
>   - Besides, even when supported, not every possible locale is
>     available on every browser installation (would make them huge).
>     - One can check availability of an array of locales like this:
>
>       ```js
>       // Are english, spanish, french and balinese locales supported?
>       alert ( Intl.Collator.supportedLocalesOf(
>         ['en', 'es', 'fr', 'ban']);
>         // ['en', 'es', 'fr'] : balinese missing, rest are supported.
>       // Is corsican locale supported?
>       alert ( Intl.Collator.supportedLocalesOf('co');
>         // [] : no (empty array).
>       ```
>
>     - If for comparison we specify a locale that is not available, it
>       fallbacks to the default runtime locale.
> - More on locales:
>   - We are simplifying here using locales that are whole languages.
>   - A locale can be more specific than that:
>     - 'en': english language in general.
>     - 'en-US': english language, in the region of the US.
>     - 'zh-Hans-CN': Chinese language, written in Hans script
>       (simplified script, vs the traditional script), in the region of
>       China (Chinese is used also in other regions, with possible
>       variations: Hong Kong, Taiwan, ...).

## Summary

- There are 3 types of quotes. Backticks allow a string to span multiple lines and embed expressions `${…}`.
- We can use special characters, such as a line break `\n`.
- To get a character, use: `[]` or `at` method.
- To get a substring, use: `slice` or `substring`.
- To lowercase/uppercase a string, use: `toLowerCase/toUpperCase`.
- To look for a substring, use: `indexOf`, or `includes/startsWith/endsWith` for simple checks.
- To compare strings according to the language, use: `localeCompare`, otherwise they are compared by character codes.

> [!t] t: Or better use `Intl.Collator`.

There are several other helpful methods in strings:

- `str.trim()` -- removes ("trims") spaces from the beginning and end of the string.
- `str.repeat(n)` -- repeats the string `n` times.
- ...and more to be found in the [manual](mdn:js/String).

Strings also have methods for doing search/replace with regular expressions. But that's big topic, so it's explained in a separate tutorial section <info:regular-expressions>.

> [!t] t: This may deserve a "spoiler alert" introduction for the basics.

> [!t] t: At least, know `str.repace()` and `str.replaceAll()`
>
> - Subtitute something (first ocurrence) with a given string (or all
>   occurences in the second case).
> - They can be used with first parameter being a regular expressions,
>   but also with a litteral string (les powerful, but often enough and
>   quite simple to be used).
> - Examples:
>
>   ```js
>   let s = "HelLO world";
>   s.replace("wo","X"); // "HelLO Xrld"
>   s.replaceAll("l", "XX"); // "HeXXLO worXXd"
>   s.replace("wor", ""); // "HelLO ld"
>   ```

Also, as of now it's important to know that strings are based on Unicode encoding, and hence there're issues with comparisons. There's more about Unicode in the chapter <info:unicode>.
