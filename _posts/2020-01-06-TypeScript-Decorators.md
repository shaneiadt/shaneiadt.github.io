---
title: "TypeScript Decorators"
date: 2020-01-06 18:23:35
categories:
  - Blog
tags:
  - TypeScript
  - Decorators
---

First of all just so we're clear, the idea of **classes** is just that, an idea. Classes by default **do not exist in JavaScript**, they're just syntactic sugar. JavaScript was developed with a **prototypical inheritance** model.

## Prototype Inheritance

What does that mean? Let's take a look at the following **TypeScript** vs **JavaScript** output once complied.

```typescript
// TypeScript
class Person {
  name: string = 'Dave';

  speak(): void {
    console.log(`hi my name is ${this.name}`);
  }
}
```

Straight forward class called **Person** with a single property & method. Now look what the TS compiler does...

```javascript
// JavaScript (Compiled TypeScript to ES5)
var Person = (function() {
  function Person() {
    this.name = 'Dave';
  }
  Person.prototype.speak = function() {
    console.log('hi my name is ' + this.name);
  };
  return Person;
})();
```

- A **Person** variable is defined with the result of an [IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE).

- We also see there is a function declared with some property called **name** attached to it.

- Then another property called **speak** that is equal to an anonymous function on the Person functions prototype property.

Everything in JS inherits from the Object class but only Functions have access to this prototype property. In the above example the TS complier is creating what is known as a **Constructor Function**.

A **Constructor Function** gives us the ability to create a **blueprint** or a new **object type** in our program. New objects can be created using this blueprint with the `new` keyword like so:

```javascript
var dave = new Person();
dave.speak(); // hi my name is Dave
```

We can also define new functions on different properties after an instance of this object is created. All previous & new objects created using this Person type will have / share access to it's parents prototype.

```javascript
var dave = new Person();
dave.speak(); // hi my name is Dave

dave.age = 38;

Person.prototype.sayAge = function() {
  console.log(`I'am ${this.age} years old`);
};

dave.getAge(); // I'am 38 years old
```

With that brief reminder out of the way...on to decorators!!!

## Decorators

What is a decorator?

> _A decorator is a function used to modify properties or methods on a class_

> **Note**: [JavaScript decorators](https://www.sitepoint.com/javascript-decorators-what-they-are/) are not the same as TypeScript decorators.

We can use decorators to update properties, methods, getters & setters of a class. Also decorators at the moment are still in an experimental phase.

If you want to use them in your TypeScript projects add the below to your tsconfig file.

```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

Here's an example of a decorator in use.

```typescript
class Person {
  name: string = 'red';

  get getName(): string {
    return `My name is ${this.name}`;
  }

  @testDecorator
  speak(): void {
    console.log('BOOM');
  }
}

function testDecorator(
  target: any,
  key: string,
  desc: PropertyDescriptor
): void {
  console.log('target:', target);
  console.log('key:', key);
}
```

A decorator in it's simplest form is just a function that accepts some predefined arguments, after that it can do whatever it want's related to that property/class.

**What are these arguments?**

- 1st argument is the **prototype** of the object (_Person.prototype_)
- 2nd argument is the **key** of the **property**/**method**/**accessor** the decorator is applied to (_'speak'_)
- 3rd argument is the property descriptor (_null_ - more on this below)

So this would render the same result as the decorator:

```typescript
testDecorator(Person.prototype, 'speak', null);
```

> **Note**: Decorators are applied when the code for this class is run initially for the first time.
> **NOT WHEN AN INSTANCE OF THIS CLASS IS CREATED**!

A decorator function relates closely to functionally composition & looks a little like a higher-order component.

The **3rd argument** supplied to our decorator function is the **property description** of the current property/method this decorated is applied to.

Every property/method on a class has a special property descriptor that outlines a few **attributes** related to itself.

For example whether this property is **writable** & what it's **current value** is.

Read more about these **property attributes** [here](https://codeburst.io/javascript-object-property-attributes-ac012be317e2).

Let's update our example with a decorator for logging any errors throw.

```typescript
class Person {
  name: string = 'red';

  get getName(): string {
    return `My name is ${this.name}`;
  }

  @logError
  speak(): void {
    throw new Error();
    console.log('BOOM');
  }
}

function logError(target: any, key: string, desc: PropertyDescriptor): void {
  const method = desc.value;

  desc.value = function() {
    try {
      method();
    } catch (e) {
      console.log('Opps, no dice!');
    }
  };
}

new Person().speak();
```

### Decorator Factories

In the above decorate example we are getting a reference to the current value of the property (which is a function) & assigning it to a new variable called `method`.

So whenever the `speak` function is invoked it will be wrapped in the `try/catch` block we wrote inside our decorator.

What if we wanted to reuse this logError decorator & provide a custom error message. We can't just simply passing some params like so:

```typescript
@logError('Ohhh no explosions!') // TypeScript will slap you!!
speak(): void {
  throw new Error();
  console.log('BOOM');
}
```

We need to use something called a **decorator factory**. Which is just like a normal factory function in that it returns a function. Here's an example:

```typescript
function logError(errorMessage: string) {
  return function(target: any, key: string, desc: PropertyDescriptor): void {
    const method = desc.value;

    desc.value = function() {
      try {
        method();
      } catch (e) {
        console.log(errorMessage);
      }
    };
  };
}
```

Now we can pass in an argument with our function similar to the example above.

### Property Access Limitation

One limitation to using decorators is the fact we can't get direct access to the value of any properties defined within our class.

Attempting to read the persons name from a decorator like so will not work:

```typescript
class Person {
  @testDecorator
  name: string = 'red';
}

function testDecorator(target: any, key: string) {
  console.log(target.name);
}
```

There are also two other types of decorators we can use: **parameter decorators** & **class decorators**.

```typescript
@classDecorator
class Person {
  @testDecorator
  name: string = 'red';

  @testDecorator
  get getName(): string {
    return `My name is ${this.name}`;
  }

  @logError('Ohhh no explosions!')
  speak(@paramDecorator lang: string): void {
    console.log('BOOM');
  }
}

function classDecorator(constructor: typeof Person) {
  console.log(constructor);
}

function paramDecorator(target: any, key: string, index: number) {
  console.log(key, index);
}

function testDecorator(target: any, key: string) {
  console.log('target', target);
  console.log('key', key);
}

function logError(errorMessage: string) {
  return function(target: any, key: string, desc: PropertyDescriptor): void {
    const method = desc.value;

    desc.value = function() {
      try {
        method();
      } catch (e) {
        console.log(errorMessage);
      }
    };
  };
}
```

So we have 5 different types of decorators at our disposal:

- Class decorators
- Method decorators
- Accessor decorators
- Property decorators
- Parameter decorators

If you fancy reading more on TypeScript decorators [check this out](https://www.typescriptlang.org/docs/handbook/decorators.html). :thumbsup:
