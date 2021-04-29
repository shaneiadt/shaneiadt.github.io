---
title: "Webpack 4 Beginners"
date: 2019-07-29 18:56:02
categories:
  - Blog
tags: 
  - Webpack
---

In this article we will run over the basic setup for [Webpack 4](https://webpack.js.org/). Why Webpack & not some other bundler like [Parcel](https://parceljs.org/) or [RollUp](https://rollupjs.org/guide/en/)? I know Webpack so I figured that would be a good start, lets keep it simple shall we!

![](https://i.imgflip.com/1tlvet.jpg)

Bundlers in general can make your dev life a lot easier & far more productive, but some can absorb you into config hell! For example here's a zero configuration Webpack project minus the hair plugging.

Let's create our project & setup npm as our package manager of choice. The -y flag tells npm to use the default package.json configuration, I don't care for questions thank you very much.

```
mkdir webpack-4-begginners
cd webpack-4-beginners
npm init -y
```

Oh yeah we need to install Webpack 4 & Webpack CLI as our development dependencies (the -D flag is shorthand for --save-dev) & also if you don't already have it [Node](https://nodejs.org/en/download/)(I'm currently using v10.15.3).

```
npm i webpack webpack-cli -D
```

Now we can create our main index.js file which will be the entry point for our application...or at least that's what we'll tell our bundler. By default with zero config Webpack will look for our entry file in the src folder of the current directory. So let us oblige. Otherwise you'll get an error & have to create it anyway, suit yourself really.

```
mkdir src
cd src

echo >> index.js (Windows)

touch index.js (Mac)
```

Let's throw something into our index file.

```
console.log('Webpack 4 Beginners')
```

All we need to do now is tell Webpack to do it's magic on the stuff using the scripts section of our package.json file.

```
{
  "name": "webpack-4-beginners",
  "version": "1.0.0",
  "description": "",
  "main": "./src/index.js",
  "scripts": {
    "build": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.38.0",
    "webpack-cli": "^3.3.6"
  }
}
```

Let the bundling commence!!! Alternatively you could run `webpack` straight from the command-line.

```
npm run build
```

Will you look at that we did it! A lovely **dist** folder containing a **main.js** file with some crazy looking JavaScript inside.

This is our bundled js believe it our not. To demonstrate copy the code from your main.js file open your console window in Chrome (or whatever browser you're using) & run it.

![](/assets/images/webpack-4-beginners/first-bundle.jpg)

We now have a simple module bundler setup, what now? Let's test our bundler to make sure it's actually working.

Create a new file in your src directory called message.js with the below js.

```
export default "Webpack is Awesome!"
```

Update your index.js file to the below

```
import message from './message'

console.log(message)
```

Now run our bundler again `npm run build` and boom our main.js file has been updated once more. Let's inspect the output as we're only using console.log through the command-line. Run `node dist/main.js` from your project directory & you should see something **Awesome** appear. :sparkles:

Our bundler is looking pretty good now, these are just the basics of course. If you fancy diving into some more details around configuring your bundler why not check out the [docs](https://webpack.js.org/concepts/).

In the next article I will be covering **Webpack Dev Server** to improve our development workflow, along with **Entry** & **Output** configuration our your bundler.

**Github Repo**: https://github.com/shaneiadt/webpack-4-beginners

Exciting times I know woop woop! Go forth and bundle my friends :wave: