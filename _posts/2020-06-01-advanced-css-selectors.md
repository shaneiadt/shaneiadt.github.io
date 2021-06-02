---
title: "Advanced CSS Selectors"
date: 2021-06-01 12:00:00
categories:
  - Blog
tags:
  - CSS
---

Bring your CSS skills to the next level with some advanced features offered to us right out-of-the-box! **NO** build step, **NO** SASS and **NO** funny business. 

![](https://media.giphy.com/media/3iA9OJ2qxV3QfRLons/giphy.gif)

# Attribute Selectors

Firstly what is an attribute? These are the additional pieces of information that we can add inside the opening & closing tags of our HTML elements. For example `id`, `title`, `name` and even `class` are all attributes.

```html
<button id="myBtn" type="button" class="btnStyle">CLICK ME PLEASE!</button>
```

Let's have a look at some attribute selectors we can use to test for some value or structure in our element attributes. These are similar to [regular expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) flags....in a round about way.

These selectors are usually defined in the following manner below. The selector could be anything like a class selector or even a universal selector.

```css
selector[attribute]
```

## Present selector `[]`

Find any element based on where it includes an attribute.

```css
button[id] {
	color: green;
}
```

## Equals selector `=`

Find any element that exactly matches the attribute value.

```css
button[class="btnStyle"] {
	color: white;
}
```

## Begins with selector `^`

Find any element that begins with the attribute value.

```css
button[class^="btn"] {
	color: orange;
}
```

## Ends with selector `$`

Find any element that ends with the attribute value.

```css
button[class$="Style"] {
	color: purple;
}
```

## Contains selector `*`

Find any element that contains the value you are looking for.

```css
button[class*="nSty"] {
	color: yellow;
}
```

# Combination Selectors

Another set of advanced CSS selectors are the combination selectors. 

These selectors can combine more than one selector.

Let's use the below HTML code as an example.

```html
<div>
	<p>...</p>
	<section> 
		<h2>...</h2>
		<p>...</p>
		<p>
			<span>...</span>
		</p>
	</section>
	<section>
		<article>
			<h2></h2>
			<p>...</p>
			<p>
				<span>...</span>
			</p>
		</article>
	</section>
</div>
```

## Descendant selectors

Find all elements that are **descendants** of a specified element.

```css
div p {
	font-size: 2em;
}
```

## Child selectors

Find all elements that are **direct** children of a specific element.

```css
div > p {
	border: 1px solid black;
}
```

## Adjacent sibling selectors

Find all elements only if it **immediately** follows the first element.

```css
h2 + p {
	font-size: 1em;
	color: orange;
}
```

## General sibling selectors

Find any elements that are **siblings** of an element.

```css
article ~ h2 {
	font-weight: bold;
}
```

Have fun CSS-ing :sunglasses: