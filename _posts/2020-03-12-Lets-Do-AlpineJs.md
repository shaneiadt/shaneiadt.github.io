---
title: "Lets Do AlpineJs"
date: 2020-03-12 17:05:29
categories:
    - Blog
tags:
    - JavaScript
    - AlpineJS
---

[AlpineJs](https://github.com/alpinejs/alpine) is (quote from Github README):

> "A rugged, minimal framework for composing JavaScript behaviour in your mark-up."

Pretty straight forward definition, now why might we use this framework over one of the more popular options such as [React](https://reactjs.org/), [Vue](https://vuejs.org/) or [Angular](https://angular.io/).

## Why

- It provides a minimal amount of JS for simple DOM manipulation with no massive overheads
- **NO VIRTUAL DOM** so it's easier to setup & get going
- **NO EXTRA BUILD STEPS**, use the CDN link or the npm package....that's it!
- Declarative & very small footprint 4kb gzipped

## Installation

You can use `npm` or take a CDN approach like below.

```html
<html>
    <head>
        <script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.js" defer></script>
    </head>
    <body>
        <h1>Lets Do AlpineJs</h1>
    </body>
</html>
```

## Simple Example

Let's toggle the `h1` element based on the state of the value `show`.

The `x-data` attribute maintains state within it's current element scope.

The `@click` directive binds to the buttons native click event listener.

```html
<html>
    <head>
        <script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.js" defer></script>
    </head>
    <body x-data="{show:false}">
        <h1 x-show="show">Lets Do AlpineJs</h1>
        <button @click="show=!show">Toggle</button>
    </body>
</html>
```

Awesome...what else do we have?

All snippets can be found at https://codepen.io/shaneiadt/pen/NWqypaV?editors=1100 which is setup with **AlpineJs** & [Bootstrap 4](https://getbootstrap.com/).

## Conditional Class Rendering

For things like Modal windows. You can bind to pretty much any element attribute.

If you have ever used Vue, a lot of this will feel familiar.

```html
<div x-data="{ isModalOpen: false }">

    <div class="modal" :class="{'show-modal': isModalOpen === true}" role="dialog">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Modal title</h5>
                    <button type="button" class="close" @click="isModalOpen = false">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <p>Modal body text goes here.</p>
                </div>
                <div class="modal-footer">
                    <button x-ref="modalCloseButton" type="button" class="btn btn-secondary"
                        @click="isModalOpen = false">Close</button>
                </div>
            </div>
        </div>
    </div>

  <button type="button" @click="isModalOpen = true" class="btn btn-primary">
    Open Modal
  </button>

</div>
```

## Other Directives

Here are some examples of other directives **AlpineJs** gives us.

`x-show`

```html
<div class="card" x-show="currentTab === 'Home'">
    <div class="card-body">
        <h5 class="card-title">Home</h5>
        <h6 class="card-subtitle mb-2 text-muted">subtitle</h6>
        <p class="card-text">Paragraph text goes here!</p>
    </div>
</div>
```

`x-model`, `x-text`, `x-html`

```html
<p x-html="exampleModel"></p>
<p x-text="exampleModel"></p>
<input type="text" x-model="exampleModel" />
```

`x-ref`

Here's an example fetching some data on button click & using the magic `$refs` property to update the innerHTML of a div element.

```html
<div>
    <button
        class="btn btn-primary"
        @click="fetch('https://jsonplaceholder.typicode.com/todos/1')
                .then(response => response.json())
                .then(json => { $refs.todo.innerHTML = JSON.stringify(json) })">
        Fetch Todo Title
    </button>

    <div x-ref="todo">
        Click button above to fetch some data
    </div>
</div>
```

`x-for`

Loop through stuff :smile:

Only catch with this one is we need to use the `template` element not a regular DOM element.

```html
<template x-for="item in items" :key="item">
    <div x-text="item"></div>
</template>
```

These are only a few examples, to read about the rest visit [Alpine](https://github.com/alpinejs/alpine) on Github.

This is really all you need to get going making those UI's more reactive.

There are some additional **Magic Properties** around dispatching custom events (`$dispatch`), executing functions after Alpine has made it's DOM updates (`$nextTick`) & dipping into the native event object (`$event`).

All-in-all I for one really enjoy using **Alpine** & fully encourage every web developer to give it a shot. You'll be surprised what you can do with minimal effort.

## References

- [AlpineJS](https://github.com/alpinejs/alpine)
- [CodePen Examples](https://codepen.io/shaneiadt/pen/NWqypaV?editors=1100)