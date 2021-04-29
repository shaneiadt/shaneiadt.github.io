---
title: "Start Using The Shadow DOM"
date: 2019-09-11 09:22:27
categories:
  - Blog
tags:
    - HTML
    - DOM
    - Shadow DOM
---

**What the heck is this Shadow DOM** I've been hearing about?

The **Shadow DOM** was created to **improve encapsulation** of components on the web. Previously & still to this day companies such as Facebook & Twitter expose their *like* & *follow* buttons through an `iframe` element.

![](/assets/images/start-using-the-shadow-dom/facebook-like.jpg)![](/assets/images/start-using-the-shadow-dom/twitter-follow.jpg)

This ensures the styles of the button remain intact when used on other web pages. Yes the Shadow DOM is awesome for **scoping styles** to it's child elements :clap:

You can think of the Shadow DOM as a *slimline* version of the full [Document Object Model](https://www.w3.org/TR/WD-DOM/introduction.html) (DOM).

The good news is now we can create a similar encapsulated component without the need for any iframe use!

> Note: Shadow DOM is supported by default in Firefox (63 and onwards), Chrome, Opera, and Safari. The new Chromium-based Edge (75 and onwards) supports it too; the old Edge didn't.

## Let The Ninja Training Begin

Let's start off with a simple HTML document.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Simple DOM example</title>
  </head>
  <body>
      <section>
        <h1>What is the Shadow DOM?</h1>
        <p>That's a pretty dang good question!</p>
        <div id="root">
            <p>This is our fallback text</p>
        </div>
      </section>
  </body>
</html>
```

Now this will produce a DOM structure like so.

![Example DOM Tree](/assets/images/start-using-the-shadow-dom/example-dom-tree.jpg)

The Shadow DOM works by **allowing hidden DOM tree elements** to be **attached** to what is called the **shadow root**, which is just the root node / parent element pretty much.

An interesting thing to note here is the Shadow DOM isn't anything new, browsers have been using these shadow trees to render for example a range `input slider` or the browsers `video controls`.

Here's an example of the `input` element with a **range** type using the Shadow DOM API to render the slider.

The slider...

<input type="range">

Here's a screenshot of what it looks like from dev tools.

![Input Range Slider](/assets/images/start-using-the-shadow-dom/input-range-slider.jpg)

> Note: you must enable the "Show user agent shadow DOM" option within the dev tools settings to see this in your console.

## Using The Shadows

Now let's attempt to create our own element that uses this Shadow DOM API.

First we need to select our element make it the **shadow host**, which is a regular DOM element which the shadow DOM is attached to.

```javascript
const shadowHost = document.querySelector("#root");
const shadow = shadowHost.attachShadow({mode: 'open'});
```

The difference with mode `open` & `closed` is with it set to `open` you can access the shadow DOM using JavaScript written in the main page context.

Here's what the element looks like from the dev tools now.

![Setting up the shadow host](/assets/images/start-using-the-shadow-dom/shadow-host.jpg)

## Encapsulation FTW!

Now that we have our **shadow host** setup we can start adding elements into it using the [DOM API](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model). Let's create a simple `paragraph` tag & append it to over shadow DOM tree.

```javascript
const paragraph = document.createElement("p");
paragraph.innerText = `
    It's an awesome way to create
    re-useable encapsulated web components!
`;
shadow.appendChild(paragraph)
```

Which results in a little something like so...

![Attaching a paragraph element into our Shadow DOM](/assets/images/start-using-the-shadow-dom/attach-paragraph.jpg)

## Adding Some Style

Because our shadow DOM is scoped to itself we can now add our own styles & they will be encapsulated to the elements we've attached / appended.

```javascript
const styles = document.createElement("style");
styles.textContent = `
    p {
        font-weight: bold;
        font-size: 50px;
        width: 50%;
        color: purple;
    }
`;
shadow.appendChild(styles);
```

![Adding some style](/assets/images/start-using-the-shadow-dom/adding-some-style.jpg)

Pretty cool yeah I know! Our fall-back text is in place just encase the current browser doesn't support the Shadow DOM API.

Here's the complete example we used in this article, why not test it out locally :thumbsup:

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Simple DOM example</title>
</head>

<body>
    <section>
        <h1>What is the Shadow DOM?</h1>
        <p>That's a pretty dang good question!</p>
        <div id="root">
            <p>This is our fallback text</p>
        </div>
    </section>
    <script>
        const shadowHost = document.querySelector("#root");
        const shadow = shadowHost.attachShadow({
            mode: 'open'
        });

        const paragraph = document.createElement("p");
        paragraph.innerText = `
            It's an awesome way to create
            re-useable encapsulated web components!
        `;
        shadow.appendChild(paragraph)

        const styles = document.createElement("style");
        styles.textContent = `
            p {
                font-weight: bold;
                font-size: 50px;
                width: 50%;
                color: purple;
            }
        `;
        shadow.appendChild(styles);
    </script>
</body>

</html>
```

## Lessons Learned

The Shadow DOM itself is a collection of HTML elements (a tree) which is rendered like any other element. But unlike the DOM, a Shadow DOM Tree requires to be attached to an element within the regular DOM. Without the DOM, we are nothing :bow:

Start creating re-usable web components using the Shadow DOM API today & amaze all that surround you :laughing: :joy: