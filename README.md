# Introduction to CSS

## Objectives

1. Explain how styles are applied to the DOM (i.e., what does "cascading" mean?)
2. Describe how the different selectors look
3. Explain selector specificity

## Working in style(s)
![James Brown](https://media4.giphy.com/media/3oEjI8PRXbQNk8dt2U/200.gif)

Standard HTML without any mockup makes our content render in the browser's default styles, which are generally incredibly
dull. It makes our HTML look like a document, which was the original intention of HTML. Nowadays, HTML is used for apps,
games, and much more — these barely resemble documents, if at all.

So how do these developers achieve a wildly different look for their content? Do they possess some kind of ancient arcane
knowledge (some would argue they do)? Nope, they use **CSS** to make things look pretty! CSS stands for Cascading Style
Sheets. CSS is a language that allows us to specify what our elements on screen look like. For example:

```css
p {
  font-size: 16px;
  color: red;
}
```

This tells the browser 'I want my `<p>` tags to have a font size of 16px, and I want its color to be red'. Not terribly
hard, right?

## Scope of CSS in this course
The purpose of this lesson is to quickly get to grips with how CSS works on a very high level. We're not concerned with
writing much CSS ourselves. We do have to know how CSS works and — more specifically — how CSS selectors work. Selecting
DOM nodes in JS to work with uses CSS selectors, so it's important to know how these work!

To make our end results a little prettier, we _will_ be using a CSS framework — this allows us to have custom styling
without writing any (or a lot, anyway) CSS ourselves. We just have to use the classes that the framework handily
provides.

## Where CSS lives
99% of CSS in any application is usually declared in external stylesheets. These are then referenced using the `<link>`
tag in the `<head>` of the document:

```html
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

Sometimes, they are declared inline in the `<head>`:

```html
<head>
  <style>
    p {
      font-size: 16px;
      color: red;
    }
  </style>
</head>
```

Lastly, we can also set inline styles on a specific DOM element:

```html
<p style="font-size: 16px; color: red;">I am a paragraph.</p>
```

This inline way of styling things is avoided as much as possible. It's generally only used to hide and show things, if
at all.

## Flow like a waterfall
![Waterfall](https://media.giphy.com/media/Y1y8jQIVsiPFS/giphy.gif)

Let's go back to what CSS stands for: **Cascading** Style Sheets. An element can be matched by several selectors (more on
that soon), and the properties of those selectors can overwrite each other. That may sound complicated, so here's an
example:

```css
body {
  font-size: 24px;
  color: blue;
}

p {
  color: red;
}
```

Assuming that the browser's default font size is 16px, what font size and color will the `<p>` tag have?

The solution is: 24px font size, and a red color. The font size is inherited from the `<body>` ruleset, and the color is
`red` because it's been specified both on the `<body>` and `<p>` tag. The ruleset for making `<p>` red has precedence,
since its selector is more _specific_. It effectively overwrites its inherited `font-size` property with its own value.

There's a lot more to be said about this, but since we won't actually write CSS ourselves, it's kind of out of the scope
of this course. Feel free to read up on it though, since you will definitely be using CSS from time to time when working
on front-end stuff!

## Classes and ID's
We can also add `class` and `id` attributes to our DOM nodes to make it easier to work with our styles. Let's say we had
a style for making things red. We can't just go and create a custom `<red>` element, so we can use a class instead and
reuse that across our code:

```css
.red {
  color: red;
}
```

```html
<p>
  I am a <span class="red">paragraph</span> with some red <span class="red">words</span>.
</p>
```

We can also add multiple classes to the same HTML element:

```css
.red {
  color: red;
}

.big {
  font-size: 24px;
}
```

```html
<p>
  I am a <span class="red big">paragraph</span> with some red <span class="red big">words</span>.
</p>
```

As you can see, using classes to group certain styles together allows us to create generic groups of style properties
that we can reuse in our code. Neat!

ID's, on the other hand, are used to specifically target one element. The DOM spec specifies that an ID can be used _only
once_ in your entire DOM.

This would be valid HTML:

```html
<p id="paragraph">I am a paragraph.</p>
```

But this would be invalid, since there are two nodes sharing the same ID:

```html
<p id="paragraph">I am a paragraph.</p>
<p id="paragraph">I am another paragraph.</p>
```

ID's are generally only used to access elements through JavaScript (though classes can also be used to this end).

## Selectors
![Bo Selector](https://media.giphy.com/media/M8Oyl5kERXxv2/giphy.gif)

_Bo Selector._

In a nutshell, selectors are a group of identifiers to select the elements we want to style. An identifier can be:

- A tag name (e.g. `p` or `div`)
- A class (e.g. `.red`)
- An ID (e.g. `#something-specific`)
- [And many more specific ones](https://developer.mozilla.org/en/docs/Web/Guide/CSS/Getting_started/Selectors)

Let's examine:

```css
.red {
  color: red;
}
```

This selector selects any elements that have the `red` string in their `class` attribute. 

```css
.red p {
  color: indigo;
}
```

This selector selects any `<p>` elements that are nested _inside_ elements with the `red` class. `<p>` tags that are not
inside another element with the `red` class will not be affected.

```css
.red,
.green {
  font-weight: bold;
}
```

This CSS rule makes elements that have either the `.red` or the `.green` class bold. The comma means _AND_ — this
selector, and this one, and so on...

## Selector specificity
![Krusty on specificity](https://media.giphy.com/media/3o6MbohtFgsEHPgENO/giphy.gif)

Any CSS declaration has a certain 'weight' attached to it, determined by the selector type. In short, this means that
some selectors are more specific — and higher up the priority list — than others.

To be more _specific_ (drum roll, please), let's look at an example:

```css
p {
  font-size: 14px;
}

.bigger {
  font-size: 16px;
}

#paragraph {
  font-size: 24px;
}
```

With this CSS, let's take a look at some different paragraphs and try to figure out what their font size is:

```html
<p>I am a generic paragraph</p>
```

Font size: 14px. We're just using a tag name here, nothing too specific about that.

```html
<p class="bigger">I am a bigger paragraph</p>
```

Font size: 16px. Since we added a class, and a class selector has more weight than a tag, the font size gets bumped up.

```html
<p id="paragraph">I am a specific paragraph</p>
```

Font size: 24px. The ID takes precedence over the tag selector.

```html
<p class="bigger" id="paragraph">I am a specific paragraph</p>
```

Font size: still 24px. The ID selector has more weight than the class and tag ones, so the right font size is still applied.

Selector specificity is very important to keep in mind when authoring your CSS. If you don't consider the specificity
rules, you're guaranteed to end up with wonky cascading CSS that'll make you want to tear your hair out.

Save your scalp. Respect the specificity.

To learn more about CSS selector specificity, MDN has
[a great article](https://developer.mozilla.org/en/docs/Web/CSS/Specificity) that goes really in-depth!

## Using JS to select DOM nodes
Let's say we want to select all HTML elements with the `red` class using JS. The CSS selector for that would be `.red`.
We can select elements in the DOM using JS by using either `document.querySelector()` or `document.querySelectorAll()`.
The difference between the two is that `document.querySelector()` will return the first matching element, while
`document.querySelectorAll()` will return a `NodeList` (comparable to an Array) of _any_ elements that match. If you're
using jQuery, you can select things using the `$()` syntax instead.

For example, to select all red elements in the document:

```js
const redElements = document.querySelectorAll('.red');

// Or, if using jQuery:
const redElements = $('.red');
```

The benefit of using jQuery instead of the native selectors is that, among a host of other things, jQuery hides the
`NodeList` abstraction, returning an array instead. You don't have to use jQuery to select DOM nodes though, as we saw
above!

Let's select another element, this time we want to select any `.big` elements, as well as any `<p>` tags inside an
element with the `red` class:

```js
const boldElementsAndRedParagraphs = document.querySelectorAll('.big, .red p');
```

See something familiar? We're using the exact same CSS syntax for selectors in our JS. One less thing to learn and worry
about!

While we definitely skimmed over a lot of stuff, we have achieved some semblance of high-level CSS knowledge that we can
use to make our apps a little prettier.

## Resources

- [CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
