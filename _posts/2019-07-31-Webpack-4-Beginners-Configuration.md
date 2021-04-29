---
title: "Webpack-4-Beginners-Configuration"
date: 2019-07-31 19:09:58
categories:
  - Blog
tags:
    - Webpack
---

Welcome to Part 2 of our journey into understanding Webpack just a wee bit better together! If you haven't read [Part 1](https://shaneiadt.github.io/2019/07/29/Webpack-4-Beginners/) I suggest you check it out.

We left off from here: https://github.com/shaneiadt/webpack-4-beginners/tree/master/zero-config

So we've set up our basic zero config bundler project, what now?

## Two Modes, One Entry File

Let's spice things up by taking advantage of the two modes our bundler can be set to, **development** & **production**. By default Webpack will assume you are running in **production mode** if not specified.

What's the difference? Production reduces the bundle as much as it can & development contains things like comments & line-breaks that make debugging easier. We can also setup source-maps but we won't get into it for now.

We'll set our mode by updating our npm build script.

```
"scripts": {
    "build": "webpack --mode development"
},
```

and of course run the following `npm run build`.

Now have a look at our **main.js** file, you can see it's not bundled within an inch of it's life. Perfect for...you guessed it, development purposes!

## Entry & Output

What else can we configure? What if we want to specify a **different** **entry** & **output file**? Well we can do that too from our build script.

```
"scripts": {
    "build": "webpack --mode development ./src/a-different-entry-file.js --output ./temp/new-bundle.js"
},
```

If we run the above we're saying hey Webpack our entry file is now here `./src/a-different-entry-file.js` and by the way output the bundle here `./temp/new-bundle.js`. Don't worry Webpack will create the folder for us :wink:

## Multiple Entry Files

Let's say we have multiple entry files how would we manage something like that. Well with something as simple as this.

```
"scripts": {
    "build": "webpack --mode development index=./src/index.js entry2=./src/a-different-entry-file.js"
},
```

**When specifying multiple entry files we can't bundle to one output file**. Webpack will throw an error if you try anyway.

As our build script continues to grow wouldn't it be great if we could manage our configuration options an easier way....well there is!!

## Configuration File

Lets create a configuration file called `webpack.config.js` for our bundling options. This can be named anything fyi!

In order for Webpack to read this file correctly we need to export what's inside it...using `module.exports`. Add the below code into our new config file.

```
module.exports = {

}
```

Now it's time to add in our previous command-line config options.

*Note: I'll keep this section light as these config files can get a bit confusing to say the least. Check the [Webpack docs](https://webpack.js.org/concepts/) for further options.*

```
module.exports = {
    entry: "./src/index.js"
    mode: "development"
}
```

Update our build script to tell Webpack where our config file is and `npm run build`.

```
"scripts": {
    "build": "webpack --config webpack.config.js"
},
```

Awesome our bundling is coming along quite nicely. We can even split out a **production** & **development** version of our output bundles with some **common options** included.

## Two Configs with Options in Common

Create a new common config file called `webpack.common.js` and add the below common config options.

```
const options = {
    entry: "./src/index.js"
}

module.exports = {
    options
}
```

If we wanted to include multiple entry files we could.

```
const options = {
    entry: {
        home: './src/index.js',
        misc: './src/a-different-entry-file.js'
    }
}

module.exports = {
    options
}
```

Update our `webpack.config.js` to include these common options.

We'll use the [ES6 spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) to include these settings.

```
const webpackCommon = require('./webpack.common')

module.exports = {
    ...webpackCommon["options"],
    mode: "development"
}
```

Create another file called `webpack.prod.js` and add the following.

```
const webpackCommon = require("./webpack.common")

module.exports = {
    ...webpackCommon["options"],
    mode: "production"
}
```

Finally update our npm scripts for these two new bundling modes.

```
"scripts": {
    "build": "webpack --config webpack.config.js",
    "prod": "webpack --config webpack.prod.js"
},
```

Finally to test out new `prod` script `npm run prod`...:open_hands:

**:boom: We did it! :boom:**

If we check our `main.js` file now it's completely minified as we've specified in our `webpack.prod.js` config file.

I think that's enough Webpack magic for now! Next time we'll look at **Webpack Dev Server** in an effort to speed up our development process.

## Summary

We've gone over some core concepts in this article including both development & production bundling modes, multiple entry files & extracting our Webpack configuration options to a couple of external files for ease of management.

Next topic shall be (as mentioned above) **Webpack Dev Server**.

**Github Repo**: https://github.com/shaneiadt/webpack-4-beginners/tree/master/a-little-config