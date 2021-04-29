---
title: "Webpack 4 Beginners Dev IT Faster"
date: 2019-08-19 15:54:28
categories:
  - Blog
tags:
    - Webpack
---

Welcome to **Part 3** of our journey into understanding **Webpack** just a wee bit better together! If you haven't read any of the previous articles I'd suggest having a goo.

- Part 1: [Zero Config](https://shaneiadt.github.io/2019/07/29/Webpack-4-Beginners/)
- Part 2: [A Little Config Goes A long Way](https://shaneiadt.github.io/2019/07/31/Webpack-4-Beginners-Configuration/)

In this instalment we shall be attempting to **speed up our development process** by configuring our project with [Webpack Dev Server](https://webpack.js.org/configuration/dev-server/).

Dev server comes with some cool out of the box configuration such as **automatic live page reloading** so every time we make a change to a file our server will re-compile & serve. Some other optionals are enabling **source-maps** for debugging.

We left off from here last time: https://github.com/shaneiadt/webpack-4-beginners/tree/master/a-little-config

## Installing Webpack-Dev-Server

Ok let's get to it, first install `webpack-dev-server`.

```
npm i -D webpack-dev-server
```

Update our npm scripts to run our dev server locally.

*Note we are still pointing to our development config webpack setup.*

```
"scripts": {
    "start": "webpack-dev-server --config webpack.config.js --open",
    "prod": "webpack --config webpack.prod.js"
},
```

We also need to update over dev configuration settings to tell Webpack Dev Server a few important things like **what directory to server locally**!

We'll also enable [**source-maps**](https://blog.teamtreehouse.com/introduction-source-maps) out of the box too :smile:

Update `webpack.config.js` to the below.

```javascript
const webpackCommon = require('./webpack.common')

module.exports = {
    ...webpackCommon['options'],
    mode: 'development',
    devtool: 'inline-source-map',
    devServer: {
        contentBase: './dist'
    }
}
```

From here yes we can run out `npm start` script to launch our dev server which will server our **./dist** directory like so.

![](/assets/images/webpack-4-beginners/dev-server-console-3.jpg)

For example sake & ease let's create an **index.html **file so our bundle.js can load into for dev purposes. The simplest way to do this is to use a plugin called `html-webpack-plugin`

Bah! What are these [**plugins**](https://webpack.js.org/configuration/plugins/) you speak of, *begone demon*!

Not to fret as you may have guessed by now *Webpack* can be configured quite extensively, (quote straight from the docs) *plugins are the backbone of Webpack*!

Let's not dive to deep into plugins just quite yet.

## Installing Our First Plugin

We're just telling our bundler to create a default html document to inject our bundle JavaScript file into...that's it...simple, now let's move on. More on **plugins** at a later stage.

1. Install the plugin.

```
npm i -D html-webpack-plugin
```

2. Add it into our `webpack.common.js` file as we want it to create a new html file for both our `development` & `production` builds / bundles.

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');

const options = {
    entry: {
        home: './src/index.js',
        misc: './src/a-different-entry-file.js'
    },
    plugins: [new HtmlWebpackPlugin()]
}

module.exports = {
    options
}
```

And that's it....now run `npm start` test. A new browser window should open up on your **localhost** displaying a blank page.

**Open your dev tools** & you should see some console logs appear!

![](/assets/images/webpack-4-beginners/dev-server-console.jpg)

## Testing Live Reloading

Brilliant stuff now let's update something in one of our files to see if our dev server is **reloading** when one of our files update.

Update `./src/message.js` to:

```javascript
export default "Webpack is even awesomer with Webpack Dev Server!!!"
```

Now save & you should see in your **terminal window** our **dev server re-compile** all our assets & *automagically* :sparkle: update everything being served locally.

![](/assets/images/webpack-4-beginners/dev-server-console-2.jpg)

## Further Configuration

We can configure our dev server a number of different ways. Here's a couple examples:

- [openPage](https://webpack.js.org/configuration/dev-server/#devserveropenpage): which page to navigate to first when your browser pops open.
- [publicPath](https://webpack.js.org/configuration/dev-server/#devserverpublicpath-): where our static files will be served & can be accessed from.
- [https](https://webpack.js.org/configuration/dev-server/#devserverhttps): we can enable https support.
- [headers](https://webpack.js.org/configuration/dev-server/#devserverheaders-): add additional headers to all responses.

These are only a few options, for more [read the docs](https://webpack.js.org/configuration/dev-server/), at the moment let's just set our **output folder** & **port number**.

Update `webpack.config.js`.

```javascript
const webpackCommon = require('./webpack.common')

module.exports = {
    ...webpackCommon['options'],
    mode: 'development',
    devtool: 'inline-source-map',
    devServer: {
        contentBase: __dirname + '/dist',
        port: 9000
    }
}
```

## Summary

In this short article we've managed to speed up our development process considerably by utilising `webpacks-dev-server`. We also configured some common settings & installed our very first plugin! *Woop Woop* :clap:

We'll dive more into webpack plugins next time to see how powerful and flexible they can make our build process.

**Github Repo**: https://github.com/shaneiadt/webpack-4-beginners/tree/master/dev-it-faster