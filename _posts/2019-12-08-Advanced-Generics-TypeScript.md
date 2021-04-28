---
title: "Advanced Generics - TypeScript"
date: 2019-12-18 18:05:34
categories:
  - Blog
tags:
  - TypeScript
  - Generics
---

## Quick Re-cap on Generics

**TypeScript generics** are pure awesomeo-5000! :space_invader:

They give us the ability to define a _dynamic type_ & use it within our applications. With the power generics give us we can make our classes & functions highly flexible.

Let's start with a simple example:

```typescript
class LotsOfNumbers {
  constructor(public collecton: number[]) {}

  get(index: number): number {
    return this.collecton[index];
  }
}

class LotsOfStrings {
  constructor(public collecton: string[]) {}

  get(index: number): string {
    return this.collecton[index];
  }
}
```

We have two classes that are extremely similar in nature. The main difference is the elements type stored in our collection array.

Now let's look at a **generic-i-fied** version that can expect both data types!

```javascript
class LotsOfAnything<T> {
  constructor(public collecton: T[]) {}

  get(index: number): T {
    return this.collecton[index];
  }
}
```

We can think of **T** just like an argument, we could name it whatever we want but **T** is the current convention.

Now any time we create a new instance of of **LotsOfAnything** we just specify the type along with it.

```typescript
const strings = new LotsOfAnything<string>(['a', 'b', 'c']);
const numbers = new LotsOfAnything<number>([1, 2, 3]);
```

An awesome example of reusable code :boom:

Moving on...

## Type Inference & Generics

Using our example above, if we remove the type annotation when creating our new objects from our **LotsOfAnything** class you'll notice everything still works fine!

```typescript
const strings = new LotsOfAnything(['a', 'b', 'c']);
const numbers = new LotsOfAnything([1, 2, 3]);
```

Strange you might think but **TypeScript** is a clever one you see...this is an example of **Type Inference**.

When we pass in an array to the class constructor TypeScript knows it's an array of strings / numbers. It looks at the collection & determines the type for us :grin:

## Function Generics

Consider the two functions below.

```typescript
function printStrings(arr: string[]): void {
  for (let value in arr) {
    console.log(value);
  }
}

function printNumbers(arr: number[]): void {
  for (let value in arr) {
    console.log(value);
  }
}
```

We can see some clear code duplication here, but how can we tidy things up?...I'll give you a hint...it rhymes with "**_lemerics_**" :wink:

```typescript
function print<T>(arr: <T>[]): void{
  for (let value in arr) {
    console.log(value);
  }
}
```

Now we have a function that can print an array with multiple types. **SUPER SICK**! How do we call this function?

```typescript
print<number>([6, 9, 6]);

// or with type inference

print([6, 9, 6]);
```

TypeScript is super intelligent don't underestimate the compiler :dragon_face:

Generally as a good "_rule of thumb_" it's best to include the type annotation when using Generics with classes & functions even though most times it can be figured out automatically.

This just helps rule out any accidental bugs :beetle:

## Generic Constraints

Where does the buck stop?

Right now :flushed: ...consider the below two classes.

```typescript
class Car {
  print() {
    console.log('Car');
  }
}

class House {
  print() {
    console.log('House');
  }
}
```

Now let's write a simple function that accepts a generic dynamic type & attempt to call the print method on each object in a given array.

```typescript
function printHousesOrCars<T>(arr: T[]): void {
  for (let i = 0; i < arr.length; i++) {
    arr[i].print();
  }
}
```

TypeScript will have an issue calling a method on a dynamically typed object.

However we can get around this by specifying a constraint to promise when we invoke the print method that it will exist....using Interfaces woop woop!

```typescript
interface Printable {
  print(): void;
}

function printHousesOrCars<T extends Printable>(arr: T[]): void {
  for (let i = 0; i < arr.length; i++) {
    arr[i].print();
  }
}

printHousesOrCars<House>([new House(), new House(), new House()]);
printHousesOrCars<Car>([new Car(), new Car(), new Car()]);
```

Generics can also maintain a contract by extending an interface like above. We are using a generic constraint to limit the type of arguments the function accepts.

Pretty neat right :smirk:
