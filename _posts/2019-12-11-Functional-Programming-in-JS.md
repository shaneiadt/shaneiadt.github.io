---
title: "Functional Programming in JS"
date: 2019-12-11 18:10:34
categories:
  - Blog
tags: 
  - Functional Programming
  - JavaScript
---

# What is Functional Programming?

You may have heard this magical phrase **[Functional Programming](https://en.wikipedia.org/wiki/Functional_programming)** (FP) flying around  recently. Some people are touting it as the new or best approach for any project.

The truth is FP has being around since the 1930's specifically using [LISP](https://en.wikipedia.org/wiki/Lisp_(programming_language)) originally specified in 1958.

FP came from mathematics & is originally based on functional abstraction with [Lambda Calculus](https://en.wikipedia.org/wiki/Lambda_calculus).

Some of the most popular FP languages at the moment are **Haskell**, **Scala** & **Clojure**.

Luckily for us **JavaScript** supports all of the awesome conceptions we're going to cover in this article.

![](/assets/images/what-is-functional-propgramming/fp.jpg)

# Core Principles

Stick to these guys & you'll be fine:

> **A perfect function should**...

- have only **one task**
- be **predictable** and **easy to understand**
- not rely on any external **shared state**
- be **composable**
- not directly **mutate** state, it should be **immutable**
- return the **same** amount of arguments from **input** to **output**
- cause **no side-effects**

The idea behind FP is much the same as **Object Oriented Programming** (OOP):

- **Don't repeat yourself** ([DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself))
- **Memory efficient**
- **Easy to extend**
- **Easy to maintain**
- **Understandable**

With these principles we can take advantage of **Functional Programming** to create a **separation of concerns** when writing our functions.

With **no classes definitions** and a reliance on **data** & the **functions** that manipulate this data to be separate, we can **reduce the number of potential bugs** that might pop up and **increase** the overall **speed** of our application.

FP mostly relies on **well structured** data **types** such as **Arrays**, **Objects**, **Dictionaries** & **Lists**.

A little note before we continue, as we know **functions** in JS **are** considered **first class citizens**. Meaning they can be:

### **assigned to a variable** :heavy_check_mark:
```javascript
const func = () => 'BOOM'

console.log(func())
```
### **passed as arguments into other functions** :heavy_check_mark:
```javascript
const speak = (str) => console.log(str)

function doStuff(fn){
    fn('Hello')
}

doStuff(speak)
```
### **returned by functions** :heavy_check_mark:
```javascript
const giveMeAFn = () => {
    return (msg) => console.log(msg)
}

const fn = giveMeAFn()

fn('JS is super sweet!')
```

# Pure Functions

> **A Pure Function**...

- is not **influenced** **by** anything other than the **input parameters** it receives. Passing in the **same** **parameters** will always **result** in the **same output**.
- does **not** cause any **side-effects** (doesn't change any external or global scope variables)
- **never changes** any of the **parameters** it is passed in, this data should be considered **immutable**. We can however copy / **clone** any input parameters :thumbsup:
- should be [referentially transparent](https://en.wikipedia.org/wiki/Referential_transparency)....click the link for a better definition than I could muster :smile:
- should only rely on it's **local variable scope**

Following these steps will make our functions easy to test & highly composable. Here is an example of a impure & pure function:

### **impure** :flushed:

In this example we're mutating a globally scoped variable by pushing a string into the array :sob:

```javascript
const names = []

function addName(n){
    names.push(n)
}

addName('John')
```

### **pure** :innocent:

In this example we're taking in an array & string then returning a brand new array :sunglasses:

```javascript
const names = []

function addName(names, name){
    return [
        ...names,
        name
    ]
}

const newNames = addName(names, 'Donny')
```

# Can everything be Pure?

Technically retrieving an input & producing an output is in itself a side-effect. It's practically **impossible** to produce a program without some side-effects existing in some way. **Side-effects & impurity aren't bad**, but reducing the amount that occur will make our program easier to understand & more reusable.

The goal is to **reduce the amount of side-effects** required to make our program run smoothly.

# Idempotence

What the hell is this craziness??? :fearful:

In simple terms idempotence means when we run the same function with the same parameters multiple times the result will always be the same.

Your functions should not communicate with the outside world & always does what we expect it to do. This makes our code more predictable & becomes a highly useful strategy within distributed & parallel systems.

### **BAD TIME** :poop:

This will result in a **different** result every single time!

```javascript
function badtime(n){
    return Math.random(n)
}

console.log(badTime(69))
console.log(badTime(69))
console.log(badTime(69))
```
### **GOOD TIME** :metal:

This will result in **the same** result every single time!

```javascript
function goodTime(n){
    return n * n
}

console.log(goodTime(69))
console.log(goodTime(69))
console.log(goodTime(69))
```

# Imperative vs Declarative

In FP we strive for a more **declarative** set of instructions that we hand off to (usual) a complier of some kind to create a more **imperative** version of our code.

**Imperative** = "*Code that tells the machine what to do & how to do it*"

**Declarative** = "*Code that tells the machine what to do & what should happen*"

We mere humans are more declarative creatures, for example if we were to tell our friend to grab a coffee for us while on their way to your place:

**Imperative instructions**:

1. Go to the newsagent
2. Pick up an empty coffee cup
3. Push button on machine
4. Take out wallet
5. Pay at counter
6. ....etc

**Declarative instructions**:

1. Buy coffee for me

**FP** helps us be more declarative with our programs. Making them **easier to read** at a high-level :grin:

# Immutability

This means **not changing the data or state** of your program. So what do we do instead?

We can make a **copy** of our data & return a new result.

### **Mutable**

```javascript
const obj = {title: 'Mister Cucumber'}

function clone(obj){
    obj.title = 'MUTATION' // object property reference mutated
}

obj.title = 'NOOOOOO' // object property reference mutated
```

### **Immutable**
```javascript
const obj = {title: 'Mister Cucumber'}

function clone(obj){
    return {...obj}
}

const clonedObj = clone(obj)
```

This makes each function easier to test & never updates global state or causes any side-effects :smirk:

...but wait what about memory? Aren't we creating unnecessary variables & filling up our heap?

I'm not the best at explaining this so here's an article on [Structural Sharing](https://medium.com/@dtinth/immutable-js-persistent-data-structures-and-structural-sharing-6d163fbd73d2) to answer these burning questions!

# Higher Order Component & Closures

### What is a Higher Order Component (HOC):

> **A higher order component is a function that does one of two things**
    > 1) it takes **one** or **more** **functions** as **parameters** or
    > 2) it **returns** a **function** as a result

Why is this important? With **HOC** functions we can **abstract** operations & **compose** multiple functions together. Making them **highly reusable**.

Here's two examples:

```javascript
const speak = (msg) => console.log(`${msg}: Says Hello!`)

function person(fn){
  fn('Shane')
}

person(speak) 
```

```javascript
function person(name) {
  return () => {
    console.log(`My names is ${name}`)
  }
}

const p = person('Shane')
p()
```

### What is Closure:

This is a **very powerful mechanism** in JS we can use to **maintain** / contain some kind of **state**.

```javascript
function counter(){
  let count = 0
  
  function incrementCount(){
    count++
  }

  function getCount(){
    return count
  }

  return {
    getCount,
    incrementCount
  }
}

const newCounter = counter()

console.log(newCounter.getCount())

newCounter.incrementCount()
newCounter.incrementCount()
newCounter.incrementCount()

console.log(newCounter.getCount())
```

Like objects, closures as mentioned above can help us maintain some state within our program. **A closure is created when a function accesses a variable outside of it's immediate function scope**.

Pretty much if it doesn't exist within the current context go up the scope chain & ask the parent(s).

This can potentially lead to some impure operations if we're not careful. :hear_no_evil:

# Currying :curry:

A curried function is simply a **function** that takes **multiple arguments** **one at a time**.

```javascript
const add = (a) => (b) => a + b

add(2)(4)
```

Using currying we can create some awesome utility functions like so:

```javascript
const multiply = (m) => (n) => m * n

const multiplyByTwo = multiply(5)
const multiplyByThree = multiply(3)

multiplyByTwo(3)   // 15
multiplyByThree(3) // 9
```

# Partial Application

Similar to Currying but slightly different. It's a way to **partially apply** some arguments to a function.

This is a process of producing a function with a **smaller number of parameters**. Taking a function & **applying** some of the **arguments** on the **initial call**, then on the second it expects all / the rest of the arguments.

```javascript
const multiply = (a, b, c) => a * b * c

const partialMultiplyByTwo = multiply.bind(null, 2);

partialMultiplyByTwo(4, 3) // 24
```

# Memoization = Caching

Is a way to store values for use later on. It is a **dynamic programming** approach in an effort to **increase** your programs **speed**. It caches the return value based on the parameters given.

This is another example of utilising a function closure to persist our cached object in the functions local scope :muscle:

```javascript
function memoization(){
  let cache = {}

  return (value) => {

    if(value in cache) {
      return cache[value]
    }

    cache[value] = value * 10

    return cache[value];
  }
}

const memo = memoization()

console.log(1, memo(5)) // add's the property & result to it's local cache object
console.log(2, memo(5)) // see's the property in cache & returns it
```

# Arity

Let's keep things simple once again. **Arity** refers to the **number of arguments** a function takes.

:rainbow: **Fewer** arguments === Greatest of success && **flexibility**
:confounded: **More** arguments === Saddest of the times && **difficulty**

# Compose & Pipe

Composing is a system principal design pattern. It **abstracts data transformations** into smaller obvious chunks. These smaller chunks can be **composed** into the desired configuration.

This allows us to change the order of operations on our data a lot easier.

```javascript
const double = (num) => num * 2
const triple = (num) => num * 3
const square = (num) => num * num

const compose = (a, b, c) => (data) => a(b(c(data)))
const pipe = (a, b, c) => (data) => c(b(a(data)))

const doCompose = compose(triple, double, square)
const doPipe = compose(triple, double, square)

console.log('compose',doCompose(2))
console.log('pipe',doPipe(2))
```

The difference between compose & pipe is:

> **compose** applies it's arguments from **right** to **left**.

> **pipe** applies it's arguments from **left** to **right**.

Here's another example were we are invoking the **buyItem** function & calling the compose function on every function argument while passing the data along the chain.

```javascript
const compose = (f, g) => (...args) => f(g(...args));

buyItem(
  addTax,
  titleToUpperCase
)({ title: 'Teddy Bear', price: 5 })

function buyItem(...fns) {
  return fns.reduce(compose)
}

function addTax(item) {
  return {
    ...item,
    price: item.price + 10
  }
}

function titleToUpperCase(item){
  return {
    ...item,
    title: item.title.toUpperCase()
  }
}
```

# Is FP the answer to everything?

Just like most topics when it comes to computer science we shouldn't take a 100% literal approach at all times.

Nothing would ever get done in that case :dizzy_face:

**Functional Programming** does promote a more:
 - **Predictable**
 - **Easy to understand**
 - **Composable**
 - **Reduced bug environment**

With these shiny rewards awaiting there isn't anything stopping us from throwing a little sprinkle of FP magic from time to time in an effort to build better applications.

Still hungry, check out [RxJS](https://rxjs-dev.firebaseapp.com/) & [Ramda](https://ramdajs.com/) or even [Lodash](https://github.com/lodash/lodash/wiki/FP-Guide).