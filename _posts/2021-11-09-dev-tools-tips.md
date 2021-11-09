---
title: "Chrome Dev Tools Tips"
date: 2021-11-09 13:00:00
categories:
  - Blog
tags:
  - Chrome Dev Tools
---

Here's a short list of tips you can try out when debugging in Chrome Dev Tools that might (hopefully) make your life a little easier :smile:

## CSS Overview

This is a handy feature to get some great information about a HTML document. Including colors, fonts, unused declarations and even media queries.

> How to enabled: Settings > Experiments > CSS Overview

![](/assets/images/dev-tools-tips/CSS-Overview.png)

![](/assets/images/dev-tools-tips/CSS-Overview-Tab.png)

## Font Editor

Quickly edit fonts with this style editor pane.

> How to enabled: Settings > Experiments > Enable new Font Editor within Styles Pane
 
![](/assets/images/dev-tools-tips/Font-Editor.png)

![](/assets/images/dev-tools-tips/Font-Editor-Icon.png)

![](/assets/images/dev-tools-tips/Font-Editor-Panel.png)

## Get Current Element

This one I find personally very useful when you are inspecting an element and just want to grab it really quick from the console. Instead of finding the id or querying for the element some way try this one out.

First inspect the element, then run `$0` inside the console window and boom you have the element.
  
![](/assets/images/dev-tools-tips/Currently-Selected-Element-1.png)

![](/assets/images/dev-tools-tips/Currently-Selected-Element-2.png)

## Last Evaluation

Get the previous console window evaluation by running `$_`.

![](/assets/images/dev-tools-tips/Previous-Evaluation.png)

## Design Mode

This one is pretty fun run `document.designMode="on"` in your console and edit any text as if it's markdown right in the browser. No need for a screenshot with this tip...you'll have to try it out yourself :smile: