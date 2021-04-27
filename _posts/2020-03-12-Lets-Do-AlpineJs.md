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

```HTML
<html>
    <head>
        <script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.js" defer><&#47;script>
    <&#47;head>
    <body>
        <h1>Lets Do AlpineJs<&#47;h1>
    <&#47;body>
<&#47;html>
```

## Simple Example

Let's toggle the `h1` element based on the state of the value `show`.

The `x-data` attribute maintains state within it's current element scope.

The `@click` directive binds to the buttons native click event listener.

```HTML
<html>
    <head>
        <script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.js" defer><&#47;script>
    <&#47;head>
    <body x-data="{show:false}">
        <h1 x-show="show">Lets Do AlpineJs<&#47;h1>
        <button @click="show=!show">Toggle<&#47;button>
    <&#47;body>
<&#47;html>
```

Awesome...what else do we have?

All snippets can be found at https://codepen.io/shaneiadt/pen/NWqypaV?editors=1100 which is setup with **AlpineJs** & [Bootstrap 4](https://getbootstrap.com/).

## Conditional Class Rendering

For things like Modal windows. You can bind to pretty much any element attribute.

If you have ever used Vue, a lot of this will feel familiar.

```HTML
<div x-data="{ isModalOpen: false }">

    <div class="modal" :class="{'show-modal': isModalOpen === true}" role="dialog">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Modal title<&#47;h5>
                    <button type="button" class="close" @click="isModalOpen = false">
                        <span aria-hidden="true">&times;<&#47;span>
                    <&#47;button>
                <&#47;div>
                <div class="modal-body">
                    <p>Modal body text goes here.<&#47;p>
                <&#47;div>
                <div class="modal-footer">
                    <button x-ref="modalCloseButton" type="button" class="btn btn-secondary"
                        @click="isModalOpen = false">Close<&#47;button>
                <&#47;div>
            <&#47;div>
        <&#47;div>
    <&#47;div>

  <button type="button" @click="isModalOpen = true" class="btn btn-primary">
    Open Modal
  <&#47;button>

<&#47;div>
```

## Other Directives

Here are some examples of other directives **AlpineJs** gives us.

`x-show`

```HTML
<div class="card" x-show="currentTab === 'Home'">
    <div class="card-body">
        <h5 class="card-title">Home<&#47;h5>
        <h6 class="card-subtitle mb-2 text-muted">subtitle<&#47;h6>
        <p class="card-text">Paragraph text goes here!<&#47;p>
    <&#47;div>
<&#47;div>
```

`x-model`, `x-text`, `x-html`

```HTML
<p x-html="exampleModel"><&#47;p>
<p x-text="exampleModel"><&#47;p>
<input type="text" x-model="exampleModel" />
```

`x-ref`

Here's an example fetching some data on button click & using the magic `$refs` property to update the innerHTML of a div element.

```HTML
<div>
    <button
        class="btn btn-primary"
        @click="fetch('https://jsonplaceholder.typicode.com/todos/1')
                .then(response => response.json())
                .then(json => { $refs.todo.innerHTML = JSON.stringify(json) })">
        Fetch Todo Title
    <&#47;button>

    <div x-ref="todo">
        Click button above to fetch some data
    <&#47;div>
<&#47;div>
```

`x-for`

Loop through stuff :smile:

Only catch with this one is we need to use the `template` element not a regular DOM element.

```HTML
<template x-for="item in items" :key="item">
    <div x-text="item"><&#47;div>
<&#47;template>
```

These are only a few examples, to read about the rest visit [Alpine](https://github.com/alpinejs/alpine) on Github.

This is really all you need to get going making those UI's more reactive.

There are some additional **Magic Properties** around dispatching custom events (`$dispatch`), executing functions after Alpine has made it's DOM updates (`$nextTick`) & dipping into the native event object (`$event`).

All-in-all I for one really enjoy using **Alpine** & fully encourage every web developer to give it a shot. You'll be surprised what you can do with minimal effort.

## References

- [AlpineJS](https://github.com/alpinejs/alpine)
- [CodePen Examples](https://codepen.io/shaneiadt/pen/NWqypaV?editors=1100)