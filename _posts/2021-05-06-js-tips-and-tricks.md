---
title: "JS Tips & Tricks"
date: 2021-05-10 12:00:00
categories:
  - Blog
tags: 
    - Javascript
---

Let's make our dev life easier with some JS related tips & tricks :smile:

## 1. Object Destructuring

This is one of the most used ES6 features I use personally and I love it!

Extract a variable from an object and create a new variable with that data.

```javascript
const fakeObject = {
  a: "BOOM",
  b: "BANG"
};

const { a, b } = fakeObject; // { a = "BOOM", b = "BANG"}
```

## 2. Rename Destructed Variables

We can also rename these too which is very handy.

```javascript
const { a: first, b: second } = fakeObject; // { first = "BOOM", second = "BANG"}
```

## 3. Removing A Property

Another trick to remove a property while destructuring using the **rest** / **spread** operator.

```javascript
const { a:_, ...theRest } = fakeObject; // theRest = { b = "BANG" }
```

## 4. Swapping Variables

This one can come in handy when you don't want to declare some temporary variables in order to actually make the swap in memory.

```javascript
let hey = 'go', ho = 'lets', lets = 'ho', go = 'hey';

[hey, ho, lets, go] = [go, lets, ho, hey].join(' '); // 'hey ho lets go'
```

## 5. Nullish Coalescing

It sounds complicated but trust me it isn't...and is also quite useful.

If the *left-hand side* of the assignment is **null** or **undefined** assign the variable to the *right-hand side* of the operation.

```javascript
const myMovies = ['Scarface', 'Goodfellas', 'Al Capone'];
const movies = myMovieList ?? []; // ['Scarface', 'Goodfellas', 'Al Capone']

const myMusicList = null;
const music = myMusicList ?? ['Nothing!']; // ['Nothing!']
```

Here's also a new new way of doing it in 2021 :sunglasses:

```javascript
let myMovies = null;

myMovies ??= ['Nothing!']; // ['Nothing!']
```

## 6. Removing Array Duplicates

This one is great to have in your back pocket. The new [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) object lets us store unique values and combined with the spread operator can drop all duplicates from a primitive/object reference array.

```javascript
const array = ['a', 'a', 'b', 'b', 'c', 'c'];
const unique = [...new Set(array)]; // ['a', 'b', 'c']
```