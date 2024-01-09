# The Modern JavaScript Tutorial

This repository hosts the English content of the Modern JavaScript Tutorial, published at [https://javascript.info](https://javascript.info).

Original repository has been forked here by a teacher to include inline notes to its students and share with them.

Notes are marked with [Obsidian
callouts](https://help.obsidian.md/Editing+and+formatting/Callouts)

They use a customized callout type "t" (for Teacher):

```md
> [!t] t: This is a note, body shown.
>
> Possible body

> [!t] t: This is another note, body condensed.
>
> Possible body
```

Some Obsidian configuration is also provided that renders that and also a possible `[!x]-` custom note for extra comments from a student.

Also a global index.md file has been created to make easier the navigation in vscode preview or in Obsidian.

## Translations

We'd like to make the tutorial available in many languages. Please help us to translate.

See <https://javascript.info/translate> for the details.

## Contributions

We'd also like to collaborate on the tutorial with other people.

Something's wrong? A topic is missing? Explain it to people, add it as PR 👏

**You can edit the text in any editor.** The tutorial uses an enhanced "markdown" format, easy to grasp. And if you want to see how it looks on-site, there's a server to run the tutorial locally at <https://github.com/javascript-tutorial/server>.

The list of contributors is available at <https://javascript.info/about#contributors>.

## Structure

Every chapter, article, or task has its folder.

The folder is named like `N-url`, where `N` is a number for the sorting purposes and `URL` is the URL part with the title of the material.

The type of the material is defined by the file inside the folder:

  - `index.md` stands for a chapter
  - `article.md` stands for an article
  - `task.md` stands for a task (solution must be provided in `solution.md` file as well)

Each of these files starts from the `# Main header`.

It's very easy to add something new.

---  
♥  
Ilya Kantor @iliakan
