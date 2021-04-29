---
title: "Multiple Selection & Group Dragging - InteractJs & Svelte"
date: 2020-05-09 09:21:57
categories:
  - Blog
tags:
  - Drag & Drop
  - InteractJs
  - Svelte
---

If you've ever touched the [HTML Drap and Drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API) you'll know it can be a bit overwhelming somethings.

To minimise the amount of fiddling we're going to use an open source library called [interact.js](https://interactjs.io/). Check it out, they have some great demo's & documentation.

Also we will be building our little app with [Svelte](https://svelte.dev/), if you haven't heard of it I'd highly advise you go have a look....like right now it's awesome!

# TL;DR

- CodeSandbox: https://codesandbox.io/s/github/shaneiadt/interactjs-and-svelte
- Github: https://github.com/shaneiadt/interactjs-and-svelte

# Setting Things Up

Create a new CodeSandbox or bootstrap a new Svelte project somewhere.

Use npm or yarn to install a new dev dependency called **interactjs**.

That's it pretty simple I like it :)

# HTML & CSS

HTML

```html
<div id="container">
  <div class="item"><span>Item 1</span></div>
  <div class="item"><span>Item 2</span></div>
  <div class="item"><span>Item 3</span></div>
  <div class="item"><span>Item 4</span></div>
</div>
```

CSS

```css
  #container {
    background: lightgray;
    height: 100vh;
  }
  .item {
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    background: lightblue;
    width: 100px;
    height: 100px;
    cursor: pointer;
    border-radius: 10px;
    padding: 1em;
    touch-action: none;
    transition: box-shadow 0.5s;
  }
  .item:hover {
    -webkit-box-shadow: 0px 10px 15px 0px rgba(0, 0, 0, 0.1);
    -moz-box-shadow: 0px 10px 15px 0px rgba(0, 0, 0, 0.1);
    box-shadow: 0px 10px 15px 0px rgba(0, 0, 0, 0.1);
  }
  .item:nth-child(1) {
    top: 100px;
    left: 100px;
  }
  .item:nth-child(2) {
    top: 30px;
    left: 300px;
  }
  .item:nth-child(3) {
    top: 400px;
    left: 320px;
  }
  .item:nth-child(4) {
    top: 250px;
    left: 200px;
  }
```

You should end up with something that looks like this.

![HTML and CSS](/assets/images/interactjs-and-svelte/html-and-css.jpg)

# Moving Stuff Around

I won't go into too much detail with the interactjs setup & options. Follow along for now & dive into the documentation if you fancy.

To get our items to become draggable we need to first allow the DOM to render run each element through the interact function exported from the interactjs package with some options applied.

As we need to wait until the DOM has render we'll be using sveltes [onMount](https://svelte.dev/tutorial/onmount) lifecycle function.

```javascript
import { onMount } from "svelte";
import interact from "interactjs";

onMount(() => {
  const elements = [...document.querySelectorAll(".item")];

  if (elements) {
    for (const el of elements) {
      interact(el)
        .draggable({
          modifiers: [
            interact.modifiers.snap({
              targets: [
                interact.createSnapGrid({
                  x: 20,
                  y: 20
                })
              ],
              range: Infinity,
              relativePoints: [
                {
                  x: 0,
                  y: 0
                }
              ]
            }),
            interact.modifiers.restrict({
              restriction: el.parentNode,
              elementRect: {
                top: 0,
                left: 0,
                bottom: 1,
                right: 1
              },
              endOnly: true
            })
          ],
          inertia: true
        })
        .on("dragmove", event => {
          const items = document.getElementsByClassName("selected");
          const { dx, dy } = event;

          const parseDataAxis = target => axis =>
            parseFloat(target.getAttribute(`data-${axis}`));

          const translate = target => (x, y) => {
            target.style.webkitTransform = "translate(" + x + "px, " + y + "px)";
            target.style.transform = "translate(" + x + "px, " + y + "px)";
          };

          const updateAttributes = target => (x, y) => {
            target.setAttribute("data-x", x);
            target.setAttribute("data-y", y);
          };

          if (items.length > 0) {
            for (const item of items) {
              const x = parseDataAxis(item)("x") + dx;
              const y = parseDataAxis(item)("y") + dy;

              translate(item)(x, y);
              updateAttributes(item)(x, y);
            }
          } else {
            const { target } = event;
            const x = (parseDataAxis(target)("x") || 0) + dx;
            const y = (parseDataAxis(target)("y") || 0) + dy;

            translate(target)(x, y);
            updateAttributes(target)(x, y);
          }
        });
    }
  }
});
```

I hope you've just copy & pasted the above, it may look a bit complicated but take some time to read each section and hopefully it'll make a bit more sense. Another option would be to also not and carrying on :laughing:

One last step is to update our item elements setting their initial x, y position.

```html
<div id="container">
  <div class="item" data-x="0" data-y="0"><span>Item 1</span></div>
  <div class="item" data-x="0" data-y="0"><span>Item 2</span></div>
  <div class="item" data-x="0" data-y="0"><span>Item 3</span></div>
  <div class="item" data-x="0" data-y="0"><span>Item 4</span></div>
</div>
```

# Drawing Our Marquee Selection Tool

We need to do a couple of different things here such as:

1. Reference the container / canvas
2. Reference the marquee selection element we will be drawing
3. Track the mouse move position within the div container element
4. Fire an event on mouseDown & mouseUp
5. Detect when our marquee selection has collided with any div items

Ok let's do this...

## Create our variables

```javascript
let canvas;
let element;
const mouse = {
  x: 0,
  y: 0,
  startX: 0,
  startY: 0
};
```

## Event handling functions

```javascript
function mouseDown(e) {
    if (e.target.id === "container") {
      const rects = [...canvas.querySelectorAll(".selection")];

      if (rects) {
        for (const rect of rects) {
          canvas.removeChild(rect);
        }
      }

      mouse.startX = mouse.x;
      mouse.startY = mouse.y;
      element = document.createElement("div");
      element.className = "selection";
      element.style.border = "1px dashed black";
      element.style.position = "absolute";
      element.style.left = mouse.x + "px";
      element.style.top = mouse.y + "px";
      canvas.appendChild(element);
    }
  }

  function mouseMove(e) {
    setMousePosition(e);
    if (element) {
      element.style.width = Math.abs(mouse.x - mouse.startX) + "px";
      element.style.height = Math.abs(mouse.y - mouse.startY) + "px";
      element.style.left =
        mouse.x - mouse.startX < 0 ? mouse.x + "px" : mouse.startX + "px";
      element.style.top =
        mouse.y - mouse.startY < 0 ? mouse.y + "px" : mouse.startY + "px";
    }
  }

  function mouseUp(e) {
    element = null;

    const rect = canvas.querySelector(".selection");
    const boxes = [...canvas.querySelectorAll(".item")];

    if (rect) {
      const inBounds = [];

      for (const box of boxes) {
        if (isInBounds(rect, box)) {
          inBounds.push(box);
        } else {
          box.style.boxShadow = "none";
          box.classList.remove("selected");
        }
      }

      if (inBounds.length >= 2) {
        for (const box of inBounds) {
          box.style.boxShadow = "0 0 3pt 3pt hsl(141, 53%, 53%)";
          box.classList.add("selected");
        }
      }

      if (rect) canvas.removeChild(canvas.querySelector(".selection"));
    }
  }
```

## Tracking mouse position & element colliding

```javascript
function setMousePosition(e) {
  const ev = e || window.event;

  if (ev.pageX) {
    mouse.x = ev.pageX + window.pageXOffset;
    mouse.y = ev.pageY + window.pageYOffset;
  } else if (ev.clientX) {
    mouse.x = ev.clientX + document.body.scrollLeft;
    mouse.y = ev.clientY + document.body.scrollTop;
  }
}

function isInBounds(obj1, obj2) {
  const a = obj1.getBoundingClientRect();
  const b = obj2.getBoundingClientRect();

  return (
    a.x < b.x + b.width &&
    a.x + a.width > b.x &&
    a.y < b.y + b.height &&
    a.y + a.height > b.y
  );
}
```

## Update HTML

```html
<div bind:this={canvas} id="container" on:mouseup={mouseUp} on:mousedown={mouseDown} on:mousemove={mouseMove}>
  <div class="item" data-x="0" data-y="0"><span>Item 1</span></div>
  <div class="item" data-x="0" data-y="0"><span>Item 2</span></div>
  <div class="item" data-x="0" data-y="0"><span>Item 3</span></div>
  <div class="item" data-x="0" data-y="0"><span>Item 4</span></div>
</div>
```

# Phew! :sweat_smile:

That was quite a lot...but at the end of all this we should have a little some some like so...

![Drag and Drop Final](/assets/images/interactjs-and-svelte/drag-and-drop.gif)

Still interested? Checkout the sandbox or github repo :heartpulse:

- CodeSandbox: https://codesandbox.io/s/github/shaneiadt/interactjs-and-svelte
- Github: https://github.com/shaneiadt/interactjs-and-svelte