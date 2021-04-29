---
title: "New 2021 JavaScript Features"
date: 2021-04-29 14:00:00
categories:
  - Blog
tags:
    - ECMAScript
    - JavaScript
---

It's 2021 and time for some juicy new javascript features for us developers to get our hands on.

Here's what's new in the world of ECMAScript 2021 (that I find interesting):

- Numeric separators
- String .replaceAll()
- Logical assignment operators
  - And & Equals (&&=)
  - OR & Equals (&#124;&#124;=)
  - Nullish Coalescing & Equals (??=)
- Promise.any()
- Private class methods

# Numeric Separators

This new addition aims to improve the readability of number & digit groups.

```javascript
const oneThousandAndChange = 1_000.69; // 1,000.69

const theLottery = 50_450_900.25; // 50,450,900.25
```

# String.replaceAll()

Sometimes you just want to replace every occurence of a substring but don't want to go messing around with RegEx. Now you can :smile:

```javascript
'the-old-ways-are-best'.replace(/\-/g, ' '); // the old ways are best

'but-not-when-it-comes-to-javascript'.replaceAll('-', ' '); // but not when it comes to javascript
```

# Logical Assignment Operators

Behold the power of logical operators and assignment expressions combined.

## And & Equals (&&=)

This assignment happens when the value is truthy.

```javascript
// old way
let a = 1;

if(a) a = 2;

// new way
let b = 3;

b &&= 4;
```

## OR & Equals (&#124;&#124;=)

Assign when the value is falsy.

```javascript
// old way
let c;

c = c || 3;

// new way
let d;

d ||= 4;
```

## Nullish Coalescing & Equals (??=)

Assign when the value is null or undefined. This one is pretty straight forward but can be confusing all at the same time.

```javascript
let e = null;
e ??= 10;
```

## Promise.any()

Returns the first promise that resolves.

```javascript
const firstPromise = new Promise((resolve, reject) => {
  setTimeout(() => reject(), 1000);
});

const secondPromise = new Promise((resolve, reject) => {
  setTimeout(() => reject(), 2000);
});

const thirdPromise = new Promise((resolve, reject) => {
  setTimeout(() => reject(), 3000);
});

try {
  const first = await Promise.any([
    firstPromise, secondPromise, thirdPromise
  ]);
  // Any of the promises was fulfilled.
} catch (error) {
  console.log(error);
  // AggregateError: All promises were rejected
}
```

## Private class methods

Previously by convention and not specification the unwritten rule was to use and underscore to label a method as private. But it wasn't really private now was it! But now we can woohoo!

```javascript
class TalkToMe {
  #msg;

  #privacyMatters(m){
    this.#msg = m;
  }

  _notReallyPrivate(m){
    this.#msg = m;
  }

  print(){
    console.log(this.#msg);
  }
}

const obj = new TalkToMe();
obj.privacyMatters('O yeah!');

// Output: TypeError: obj.privacyMatters is not a function

obj._notReallyPrivate('fake it til you make it');
obj.print();

// Output: 'fake it til you make it'
```