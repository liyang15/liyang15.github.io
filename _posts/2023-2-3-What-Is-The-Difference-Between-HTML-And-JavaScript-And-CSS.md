---
layout: post
title:  "What Is The Difference Between HTML And JavaScript And CSS?"
categories: netdevops
tags: Network Engineer
date: 2023-2-3
---

## What Is The Difference Between HTML And JavaScript And CSS?

### What Is HTML?

HTML stands for Hyper-Text Markup Language and is how content is structured in code for a browser to display. Virtually, every page you view in a browser today has it’s content structured in HTML.

### What Is CSS?

CSS stands for cascading style sheets and this is how content in HTML is styled and designed. Want to make some text **BOLD**, change the color of some text, or give something a background? CSS is the main way that is achieved today.

### What Is Javascript?

Javascript is a programming language for the web. All this means is that when you want something to move, change, or react on a website javascript is normally the thing doing it. Javascript handles logic so the if then’s of web design and development.

## How Does HTML Work?

HTML is a system of elements enclosed in tags marked by the <, >, and </ symbols. Each paragraph you see is a “paragraph element”. Each heading of text you see if a “heading element”. Each image you see is an “image element”. Each element also has properties unique to them called, “attributes”. Images are perhaps the easiest to understand, for example the width and height of an image are “attributes”. Links also have attributes like where is the link pointing to or does it open in a new tab.

### HTML Code

### Shows Up As

```
<p>This is an Example Paragraph Element, everything between the "p" tags is part of the paragraph.</p>
```

This is an Example Paragraph Element, everything between the “p” tags is part of the paragraph.

```
<img src="https://nathanielkam.com/wp-content/uploads/2020/05/close-up-of-computer-keyboard-248515.jpg">
<p>The above is an example above shows how setting the "src" attribute of an image element tells your browser what image you want to display</p>
```

![img](https://nathanielkam.com/wp-content/uploads/2020/05/close-up-of-computer-keyboard-248515-1600x799.jpg)

The above is an example above shows how setting the “src” attribute of an image element tells your browser what image you want to display

```
<p style="font-weight: bold;">The style attribute can be used to style elements, in this example by making the font bold. Attributes are written inside the first set of <> and are in the form XYZ=""</p>
```

**The style attribute can be used to style elements, in this example by making the font bold.** **Attributes are written inside the first set of <> and are in the form XYZ=””**

## Which Is Better HTML Or CSS?

In modern web development HTML and CSS really cannot live without each other. Both are necessary to achieve a modern experience. It is technically possible to make an entire website without using a CSS style sheet but it would be very impractical. More so, when it comes time to update or re-brand something it will be a massive headache.

Take a look at our last HTML example for inline css styling above, notice that for every element we’d want to be bold we’d need to add style=”font-weight: bold;” to. Imagine doing that for every paragraph across a website. Now imagine we change our mind and don’t want that text bold anymore, this would mean changing EVERY paragraph elements style attribute one by one. Surely there must be a better way right? There is, that is where CSS comes from.

## How Does CSS Work?

CSS works on a system of “selectors” like the name of the html element or an attribute like class or ID. Using a CSS selectors allows us to apply the same style attribute to all members of a class or instance of an ID without needing to copy the style=”” stuff to each element one by one.

CSS can be added to your html in many ways, you remember our style attribute (remember attributes are in the form attribute=””) in the previous section that is what is called inline css. Next we can put css into a special style element (remember elements use tags). Finally, there is the external style sheet which loads in style sheets from other locations on the net.

### CSS

```
<style>
p {
 font-weight: bold;
}
</style>
```

### HTML

```
<p>Using internal css and the paragraph element css selector we can style this text without using a style attribute.</p>
<p>This also means ALL paragraph elements get the same styling.</p>
```

### Shows Up As

**Using internal css and the paragraph element css selector we can style this text without using a style attribute.**

**This also means ALL paragraph elements get the same styling.**

```
<style>
.class-name {
 font-weight: bold;
}
</style>
<p class="class-name">CSS Class selectors are proceeded by a "." They match up to the class="class-name" attribute. You can have as many instances of a class as you want.</p>
```

**CSS Class selectors are proceeded by a “.” They match up to the class=”class-name” attribute**. **You can have as many instances of a class as you want**.

```
<style>
#id-name {
 font-weight: bold;
}
</style>
<p id="id-name">CSS ID selectors are proceeded by a "#" They match up to the id="id-name" attribute. You can only have one instance of an ID per page.</p>
```

**CSS ID selectors are proceeded by a “#” They match up to the id=”id-name” attribute**. **You can only have one instance of an ID per page.**

```
<link rel="stylesheet" type="text/css" href="style.css">
<p>Using external css we can decouple css code from our HTML code making it easier to update style without potentially breaking or overwriting HTML</p>
```

**Using external css we can decouple css code from our HTML code making it easier to update style without potentially breaking or overwriting HTML**

### Why Are They Called Cascading Style Sheets?

The term cascading is what we are most interested in here, basically there is a hierarchy and inheritance to CSS that allows for maximum code reuse and efficiency. This is really where the magic of CSS comes in and is critical to understanding how to develop your brands style.

#### CSS Cascades Through HTML Elements

Elements in HTML can be grouped inside of each other using a special container element called a “div”. When these elements are nested inside of each other the styles of the outside element are “inherited” to the inside element.

##### CSS

##### HTML

##### Shows Up As

```
<style>
.outer-div {
 font-weight: bold;
}
</style>
<div class="outer">
<p>This still gets bold styling because it is inherited from the outer element</p>
</div>
```

**This still gets bold styling because it is inherited from the outer element**

#### CSS Is Styled In Order Of Specificity

This is probably the hardest part of CSS to understand and that is specificity. Basically, the MOST specific selector wins for styling. For example in our inheritance example above the paragraph was IMPLIED to be bold because it is in a div with bold styling, that is the LEAST kind of specific. If we go up to our specific class settings that is one step MORE specific because we are explicitly saying this element should have this style. If we go up to an ID that is even more specific because unlikely a class there can be only ONE item with a particular ID. Finally, if we go all the way back to our inline CSS example using the style=”” attribute that is the MOST specific because we are explicitly linking that style to that ONE thing without any references to anything else.

In a nut shell inheritance < class < id < inline when it comes to what “wins” in styling.

![html css](https://nathanielkam.com/wp-content/uploads/2020/05/close-up-of-computer-keyboard-248515-1600x799.jpg)Side by side of HTML (left) and CSS (right)

## Draw Back With CSS And HTML

For the most part CSS and HTML are what we call static resources that means they don’t change in response to the user. There are some tricks in css to make things response but in general CSS and HTML are don’t process logic. This is where Javascript comes in.

## How Does Javascript Work?

Javascript can ALSO be written into you HTML using a special “script” element. Also just like CSS it can also be imported with a “link” element. Also like CSS javascript can find and alter HTML elements by their element, class, or ID selectors. Unlike CSS javascript can manipulate HTML based on virtually any attribute, content, or really nothing at all. That’s right javascript doesn’t even need the HTML to run. Often times javascript is used to interact with the server or browser to process things for the user.

### Common Uses For Javascript Today

Some of the most common uses for javascript are

- To generate HTML dynamically, this allows web developers to import forms, popups, or even generate whole pages through logic rather than typing each of them out.
- To process animations, example the scroll to top button we have on this site uses javascript to interact with your browser and scroll you up.
- To interact with the server, example when you scroll down in facebook or instagram and the feed keeps loading javascript is talking to the server and getting the next series of posts for you and then loading it into the scroll.
- To Interact with the browser, example when you try to leave a form and it asks you are you sure, that is javascript telling your browser to do that check and confirmation. Also pretty much anytime you upload files on the internet now that is handled though javascript.

## Which Is Better JavaScript Or HTML?

The javascript vs HTML debate is much more heated today due to frameworks like Angular and React coming onto the scene. At the end of the day however even if your entire site is written in javascript it is rendered from HTML in the browser.

## Recap On HTML, CSS And Javascript

When it comes to web design and web development you will need a strong understanding of all three of these technologies to efficiently conceive, translate, and encode ideas to the web. You big take away should be to use HTML with its element tags for structure and content. Use CSS selectors on element and like attributes like class, id, and in-line style to change how things look efficiently. And finally, that any interaction with the server or browser or processing of logic will need to be done in Javascript.

If you have these basic concepts down then you understand how almost every page on the internet is displayed. Everything else is just a matter of looking up the references for what you want it to do / look like.

## How To Learn HTML, CSS, And Javascript

The #1 place I recommend for learning and looking up HTML, CSS, and JS is [w3schools](https://www.w3schools.com/) hands down (non-sponsored). Their documentation is by far the cleanest and simplest to understand and they have interactive examples that you can play with right there in the browser.

## Interested In More Technology Concepts?

Check out our general technology posts here – https://nathanielkam.com/category/technology/

Summary

![How do HTML CSS and Javascript work together?](https://nathanielkam.com/wp-content/uploads/2020/05/florian-olivo-4hbJ-eymZ1o-unsplash.jpg)

Article Name

How do HTML CSS and Javascript work together?

Description

A detailed explanation of HTML, CSS, and Javascript. Learn conceptually how they work together to create a website and why these technologies matter.

Author

Nathaniel Kam

