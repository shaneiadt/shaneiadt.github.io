---
title: "(CSS) Magic is real with Houdini"
date: 2019-03-29 15:50:25
categories:
  - Blog
tags: 
    - CSS
    - Houdini
    - JavaScript API
---

Fed up with bloated hacky JavaScript manipulating CSS? _Me too!_ 


Enter the [Houdini Task Force](/#The-Task-Force) with one _simple_ goal...outline a collection of drafts to be accepted by [W3C](https://www.w3.org/)  & added as common web standards. Well it doesn't seem that simple but these guys are making great strides for the web and developers sake.

![The struggle is real, thanks CSS!](https://media.giphy.com/media/yYSSBtDgbbRzq/giphy.gif "The struggle is real, thanks CSS!")

Houdini will in essence offer the ability to extend the CSS rendering engine with low-level JavaScript APIs. Which will allow authors to extend the CSS engine offering massive improvements & capabilities from a user experience & performance point of view.

## Specifications

- [Worklets](/#Worklets)
- [CSS Paint API](/#CSS-Paint-API)
- [CSS Animation Worklet](/#CSS-Animation-Worklet)
- [CSS Layout API](/#CSS-Layout-API)
- [Typed CSSOM](/#Typed-CSSOM)

### Worklets

[View specification](https://drafts.css-houdini.org/worklets/)

Worklets themselves aren't really anything to write home about. Similar to a [Web Worker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API) a Worklet is more of a concept that makes use of ES2015 class definitions helping to expose these low-level APIs within our browser.

So what is a Worklet? A Worklet is almost like a mini event loop ([what is the event loop](https://www.youtube.com/watch?v=8aGhZQkoFbQ&vl=en)) that can attach or reposition itself to different events it is configured to. In the words of [Surma](https://twitter.com/dassurma?lang=en) think of it as your swiss army knife to accessing these low-level APIs.

Let's look at our first low-level API that Worklets give us access to, the CSS Paint API.

#### **As most of these specs are still in the draft phase you may need to enable experimental web features to see them in action. `chrome://flags/#enable-experimental-web-platform-features`**

[Read more about Worklets](http://houdini.glitch.me/worklets)

#### CSS Paint API

[View specification](https://drafts.css-houdini.org/css-paint-api/)

Ok what the hell does this thing do you ask & why should I be excited about it! Imagine being able to programmatically control exactly what, when & how an object is drawn to the screen. With greater control using SVGs or jamming image files into your CSS may well become less of a headache.

Instead of using things like `background-image` or `border-image` you can now reference a paint worklet like so `paint(myPainter)`. Enough with the jabber show me the goodies dag-nab-it!

[Example](https://googlechromelabs.github.io/houdini-samples/paint-worklet/diamond-shape/)

[Read more about CSS Paint API](http://houdini.glitch.me/paint)

#### CSS Animation Worklet

[View specification](https://drafts.css-houdini.org/css-animationworklet/)

This super-sweet Worklet allows us to run a set of instructions on a device at it's native frame rate preventing jumpy & awkward animations. The first port of call for most animations in CSS is [CSS Transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/transition), [CSS Animations](https://developer.mozilla.org/en-US/docs/Web/CSS/animation) & the more complex [Web Animations API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API). With the CSS Animation API / Worklet we can listen for any user input even scroll events!

This topic is quite extensive so to prevent boredom I suggest checking out the example below & read more if you fancy.

[Example](https://googlechromelabs.github.io/houdini-samples/animation-worklet/spring-timing/)

[Read more about CSS Animation Worklet](https://developers.google.com/web/updates/2018/10/animation-worklet)


#### CSS Layout API

[View specification](https://drafts.css-houdini.org/css-layout-api/)

With the ability to create & control your layouts with enormous detail never imagined before! The layout worklet can even help you build your own layouts & pass them into the `display` property, like so `display: layout('myLayout')`. 


[Example](https://googlechromelabs.github.io/houdini-samples/layout-worklet/masonry/)

[Read more about the CSS Layout API](https://houdini.glitch.me/layout)

### Typed CSSOM

[View specification](https://drafts.css-houdini.org/css-typed-om/)

CSS Object Model or Cascading Style Sheets Object Model (Typed CSSOM). If you are familiar with a programming language or two you might have stumbled upon some Object Oriented stuff & data types. What this specification focuses on is the typing part, mainly the typing of CSS values as JavaScript objects.

With Typed CSSOM every CSS value is now a member of a new base class called `CSSStyleValue`. These values can then be extended to help authors manage calculations & different value types such as `rem`, `px`, `em` & `percent`.

[Example](https://codepen.io/impressivewebs/pen/mQbqGR)

## The Task Force

With engineers from the likes of Apple, Google & Microsoft working together (_i know scary right!_) on these potentially new web standards how could you not be excited!

## Conclusion

I wanted to keep these sections brief...so there you have it! Not all the specifications are fully stable at the moment but Houdini & Worklets won't be going away anytime soon. Keep your ear close to the coconut on this one peoples.

### Resources

- [Is Houdini Ready Yet?](https://ishoudinireadyyet.com/)
- [CSS Houdini Github Wiki](https://github.com/w3c/css-houdini-drafts/wiki)
- [Houdini updates on developer.google](https://developers.google.com/web/updates/tags/houdini).
- [Houdini draft specifications](https://drafts.css-houdini.org/)
- [Chrome Dev Summit Video](https://www.youtube.com/watch?v=lK3OiJvwgSc)
- [Google Chrome Lab Houdini Examples](https://googlechromelabs.github.io/houdini-samples/)