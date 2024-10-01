# Hello, world!

This part of the tutorial is about core JavaScript, the language itself.

But we need a working environment to run our scripts and, since this book is online, the browser is a good choice. We'll keep the amount of browser-specific commands (like `alert`) to a minimum so that you don't spend time on them if you plan to concentrate on another environment (like Node.js). We'll focus on JavaScript in the browser in the [next part](/ui) of the tutorial.

So first, let's see how we attach a script to a webpage. For server-side environments (like Node.js), you can execute the script with a command like `"node my.js"`.


## The "script" tag

JavaScript programs can be inserted almost anywhere into an HTML document using the `<script>` tag.

For instance:

```html run height=100
<!DOCTYPE HTML>
<html>

<body>

  <p>Before the script...</p>

*!*
  <script>
    alert( 'Hello, world!' );
  </script>
*/!*

  <p>...After the script.</p>

</body>

</html>
```

```online
You can run the example by clicking the "Play" button in the right-top corner of the box above.
```

The `<script>` tag contains JavaScript code which is automatically executed when the browser processes the tag.

> [!t] t: alert() is bad practice, but forgiven here
>
> - js.info uses the straighforward `alert()` a lot. We will use them happily
>   for immediateness in learning.
> - But keep in mind that in real world web pages, it isn't currently a good
>   practice to use them (for reasons not mentioned here).
> - Instead of `alert()` (blocking and tied to the browser environment),
>   we can use `console.log()` (permits also executing in node most of the
>   examples of the Part I of jsinfo on JS language).
> - Besides `alert()` and `console.log()`, another typical JS "Hello world"
>   for browser is to do some DOM manipulation (using e.g. `textContent`
>   property).

## Modern markup

The `<script>` tag has a few attributes that are rarely used nowadays but can still be found in old code:

The `type` attribute: <code>&lt;script <u>type</u>=...&gt;</code>
: The old HTML standard, HTML4, required a script to have a `type`. Usually it was `type="text/javascript"`. It's not required anymore. Also, the modern HTML standard totally changed the meaning of this attribute. Now, it can be used for JavaScript modules. But that's an advanced topic, we'll talk about modules in another part of the tutorial.

The `language` attribute: <code>&lt;script <u>language</u>=...&gt;</code>
: This attribute was meant to show the language of the script. This attribute no longer makes sense because JavaScript is the default language. There is no need to use it.

Comments before and after scripts.
: In really ancient books and guides, you may find comments inside `<script>` tags, like this:

    ```html no-beautify
    <script type="text/javascript"><!--
        ...
    //--></script>
    ```

    This trick isn't used in modern JavaScript. These comments hide JavaScript code from old browsers that didn't know how to process the `<script>` tag. Since browsers released in the last 15 years don't have this issue, this kind of comment can help you identify really old code.


## External scripts

If we have a lot of JavaScript code, we can put it into a separate file.

Script files are attached to HTML with the `src` attribute:

```html
<script src="/path/to/script.js"></script>
```

Here, `/path/to/script.js` is an absolute path to the script from the site root. One can also provide a relative path from the current page. For instance, `src="script.js"`, just like `src="./script.js"`, would mean a file `"script.js"` in the current folder.

We can give a full URL as well. For instance:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js"></script>
```

> [!t] t: Possible URL's
>
> - When loading external JS file (script element with src attribute),
>   we can use any type of valid URL there that resolves to the file:
>   as seen in these examples it can be absolute (with possible implicit
>   parts), but it can also be relative.
> - We cover elsewhere the details on possible URLs and their resolution.

To attach several scripts, use multiple tags:

```html
<script src="/js/script1.js"></script>
<script src="/js/script2.js"></script>
…
```

```smart
As a rule, only the simplest scripts are put into HTML. More complex ones reside in separate files.

The benefit of a separate file is that the browser will download it and store it in its [cache](https://en.wikipedia.org/wiki/Web_cache).

Other pages that reference the same script will take it from the cache instead of downloading it, so the file is actually downloaded only once.

That reduces traffic and makes pages faster.
```

````warn header="If `src` is set, the script content is ignored."
A single `<script>` tag can't have both the `src` attribute and code inside.

This won't work:

```html
<script *!*src*/!*="file.js">
  alert(1); // the content is ignored, because src is set
</script>
```

We must choose either an external `<script src="…">` or a regular `<script>` with code.

The example above can be split into two scripts to work:

```html
<script src="file.js"></script>
<script>
  alert(1);
</script>
```
````

> [!t] t: 3 ways of adding JS to HTML
>
> There are [3 ways of adding JS to your page (from MDN Learn)](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript#how_do_you_add_javascript_to_your_page).
> Thir names are:
> - **External JS**: external .js file loaded with a script element (src
>   attribute).
> - **Internal JS**: JS specified as contents of a script element.
> - **Inline JS -handlers-**: JS specified as the value of some particular
>   attributes on different HTML elements.
>   - E.g.:
>
>     ```html
>     <button onclick="alert('Hi!');">Click to greet</button>
>     ```
>
>   - This possibility is not mentioned by jsinfo: it is a bad practice, but
>     you may see this used in bad or old code. Do not confuse it with modern
>     FE frameworks that tend to specify HTML+CSS+JS as componentes,
>     all-in-one place (e.g. with the JS extension named JSX). If done that
>     way, it is a modern good practice.

> [!t] t: Single shared global scope by default:
> - For now: no matter how you load JS code of the 3 ways, the code sees and
>   shares a unique global scope.
> - This is for good (quick and dirty), and for bad (not scaling).
> - It can be avoided with some techniques. The modern one is with ES6 modules
>   specified somehow with the "type" attribute, as mentioned in section above
>   "Modern markup".

> [!t] t: Quotes and double quotes in JS. Equivalent, but... Consistency?
> - In this Hello wolrd page in jsinfo, you will see examples of strings
>   using 'single quotes' and others using "double quotes".
> - Other languages allow only one of the two ways, or reserve a different
>   meaning for them. But in JS, they are equivalent.
> - I would recommend to choose a convention between the two, and stick to
>   it for consistency.
>   - If you are collaborating in a project, just follow the convention
>     specified by their chosen (programming) style guide. If not known,
>     just follow the convention you see used (or more used) in its
>     codebase.
>   - If it is our own code, you can choose your convention:
>     - Double quotes: are typically more familiar when you come from other
>       languages. Also JSON does not allow single quotes, so when writing
>       directly JSON, you don't have to change your mindset.
>     - Single quotes: are maybe more popular in JS world.
>   - You may use the alternate one as an exception for the convenience of
>     avoid escaping possible included quotes in the text.
> - In ES6+, there is a third option with nuances, the \`backticks\`, that
>   we will cover later in the course.
> - jsinfo is absolutely inconsistent in its use of single vs double quotes
>   (as you will see all over the course). Do not imitate this.

## Summary

- We can use a `<script>` tag to add JavaScript code to a page.
- The `type` and `language` attributes are not required.
- A script in an external file can be inserted with `<script src="path/to/script.js"></script>`.


There is much more to learn about browser scripts and their interaction with the webpage. But let's keep in mind that this part of the tutorial is devoted to the JavaScript language, so we shouldn't distract ourselves with browser-specific implementations of it. We'll be using the browser as a way to run JavaScript, which is very convenient for online reading, but only one of many.
