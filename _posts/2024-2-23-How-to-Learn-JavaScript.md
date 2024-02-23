---
layout: post
title:  "How to Learn JavaScript"
categories: netdevops
tags: Network Engineer
date: 2024-2-23
---

When I first started learning frontend development, I was determined to avoid JavaScript.

I found it confusing – I didn't understand anything about how the [DOM](https://www.freecodecamp.org/news/what-is-the-dom-document-object-model-meaning-in-javascript/) worked and I didn't think I'd use it that much anyway. I already knew HTML and CSS and that's pretty much all you need for designing websites, right?

Much to my dismay, I soon discovered that not only is JavaScript very much needed for designing websites but almost all websites depend on it.

JavaScript is used in some form or the other by [97% of all websites](https://w3techs.com/technologies/details/cp-javascript) on the internet. There's just no getting away from it.

Since avoiding it didn't work, I decided to tackle the problem head on. I made an effort to write at least [one line of JavaScript code everyday](https://github.com/jemimaabu?tab=overview&from=2019-12-01&to=2019-12-31) and started building applications to learn more.

To my surprise, once I got started I realised JavaScript wasn't the monster I thought it was. I was even - dare I say it - *enjoying* learning JavaScript.

If you're trying to move from HTML and CSS to JavaScript and you're finding it difficult like I was, then this is the article for you.

The reason JavaScript seems overwhelming at first is because there's a lot to learn.

So this article will serve as a high-level beginner's guide to learning JavaScript. I'll narrow things down, cover what JavaScript is and what it's used for, and recommend some courses to help you start your JS learning journey.

## Table of Contents

1. What is JavaScript?
2. What is JavaScript Used For?
3. How to Write JavaScript Code
4. JavaScript Projects for Beginners
5. JavaScript Tutorials for Beginners

## What is JavaScript?

JavaScript is a programming language that makes web pages interactive. This involves everything from changing colors to collecting user input and passing it to a server.

[![The relationship between HTML, CSS and JavaScript](https://res.cloudinary.com/practicaldev/image/fetch/s--2DN3-0kY--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ycpvaen0ritwrsnp3765.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--2DN3-0kY--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ycpvaen0ritwrsnp3765.png)

JavaScript is used in conjunction with HTML and CSS to build fully functional webpages.

A common analogy is to think of a webpage as a human body: HTML is the skeletal structure responsible for how the body is formed, CSS is the skin and determines how the body looks, and JavaScript is the muscle that allows the body to be functional.

With the advent of JavaScript frameworks, a lot of modern websites are heavily reliant on JavaScript and [won't work without it](https://www.freecodecamp.org/news/what-the-web-looks-like-without-javascript-c7eaf09c9983/).

## What is JavaScript Used For?

JavaScript is a very complex language that can be used for anything from [client-side rendering](https://www.freecodecamp.org/news/what-exactly-is-client-side-rendering-and-hows-it-different-from-server-side-rendering-bd5c786b340d/) to [creating large-scale applications](https://sdacademy.dev/7-apps-you-can-build-with-javascript/).

We'll be focusing on the more common uses of JavaScript when building webpages:

### 1. JavaScript lets you manipulate HTML and CSS

[![Photo by Pankaj Patel](https://res.cloudinary.com/practicaldev/image/fetch/s--GMaqDWAx--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/iv1yj0re8l13zm1t0xpo.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--GMaqDWAx--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/iv1yj0re8l13zm1t0xpo.png)

You can use JavaScript to change the HTML of a page by adding or deleting elements from the DOM or by updating element attributes. You can also use it to change the styling of an element.

Most interactive actions on a webpage such as toggling a drop down menu or displaying a modal are usually done by using JavaScript to manipulate HTML and CSS as needed.

**Recommended reading:** [How to Manipulate the DOM - A Beginner's Guide](https://www.freecodecamp.org/news/how-to-manipulate-the-dom-beginners-guide/).

### 2. JavaScript lets you carry out mathematical operations and algorithms

[![Photo by JESHOOTS.COM](https://res.cloudinary.com/practicaldev/image/fetch/s--kDBGOWok--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ny1xdk2fsdllxp097wd5.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--kDBGOWok--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ny1xdk2fsdllxp097wd5.png)

JavaScript is a programming language and can be used to compute complex mathematical equations and algorithms.

This allows us to carry out calculations on the client-side such as determining the number of characters in a form or getting the height of an element from the top of the screen.

**Recommended reading:** [How to do Math in JavaScript with Operators](https://www.digitalocean.com/community/tutorials/how-to-do-math-in-javascript-with-operators).

### 3. JavaScript lets you manipulate complex data with data structures

[![Photo by Luke Chesser](https://res.cloudinary.com/practicaldev/image/fetch/s--OK9ammeJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/o9tqmb05bb6a9zjm8uvi.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--OK9ammeJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/o9tqmb05bb6a9zjm8uvi.png)

You can use JavaScript to handle data structures in different formats such as arrays or objects.

There are multiple operations you can do on these data structures such as adding and deleting elements or filtering and sorting. This is widely used in real life situations to manipulate user data.

Recommended reading: [JavaScript Data Structures](https://www.freecodecamp.org/news/data-structures-in-javascript-with-examples/).

### 4. JavaScript lets you detect and respond to user interactions on a page

[![Photo by Christin Hume](https://res.cloudinary.com/practicaldev/image/fetch/s--mbMY9UYD--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/louuu08cevug6xzagtsz.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--mbMY9UYD--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/louuu08cevug6xzagtsz.png)

JavaScript uses event listeners such as `scroll` or `click` to monitor how the user interacts with a page. These listeners are used to carry out certain functions based on the event.

For example, you can use the `onClick` event listener to display an element when a user clicks a button.

**Recommended reading:** [Understanding Events in JavaScript](https://www.digitalocean.com/community/tutorials/understanding-events-in-javascript).

### 5. JavaScript lets you communicate with a server

[![Photo by Diego Fernandez](https://res.cloudinary.com/practicaldev/image/fetch/s--MiCzmkqU--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/a149x4gq8lo51f3r30hd.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--MiCzmkqU--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/a149x4gq8lo51f3r30hd.png)

You can use JavaScript to communicate with a server or a database by sending and receiving data.

For example, when a user submits a form on a webpage, you can create a [fetch request](https://www.geeksforgeeks.org/javascript-fetch-method) to POST the data in that form to a server and process the returned response from the server.

Recommended reading: [How to Make a GET Request and POST Request in JavaScript](https://www.freecodecamp.org/news/how-to-make-api-calls-with-fetch/)

## How to Write JavaScript Code

You add JavaScript to a basic webpage as a `script` tag in an HTML file.
The code can be written directly in the `script` tag or imported as an external `.js` file using the `src` attribute.

```
<!-- using the script tag -->
<html>
  <head>
    <title>Home</title>
    <script>
      // JavaScript code goes here
    </script>
  </head>
  <body>
    <h1>Hello, World!</h1>
  </body>
</html>
```



```
<!-- importing a JS file -->
<html>
  <head>
    <title>Home</title>
    <script src="main.js"></script>
  </head>
  <body>
    <h1>Hello, World!</h1>
  </body>
</html>
```



### How to Run JavaScript Code

JavaScript is a language that runs in the browser. Most browsers are equipped with [developer tools](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools) and these tools contain a panel called the Console.

The console is a runtime environment for JavaScript code. This means you can write JavaScript code directly into the console and see the results right in your browser.

[![Console environment with Javascript code](https://res.cloudinary.com/practicaldev/image/fetch/s--fbqV9nRg--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rnd2six5h2aw1j3lfrt9.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--fbqV9nRg--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rnd2six5h2aw1j3lfrt9.png)

To open the console, navigate to the developer tools in your browser on your laptop or desktop. There are different ways to get to the developer tools depending on which browser you're using: [Chrome](https://developer.chrome.com/docs/devtools/open/), [Safari](https://support.apple.com/en-gb/guide/safari/sfri20948/mac), [Firefox](https://developer.mozilla.org/en-US/docs/Tools) or [Microsoft Edge](https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/overview).

You can also use online code editors to run JavaScript code. Popular ones include [CodePen](https://codepen.io/) and [CodeSandbox](https://codesandbox.io/dashboard).

Online code editors are a good way to get familiar with interacting with the DOM using JavaScript, as you can see all the changes happening to your project in real time.

For example, in the Pen below, I'm using JavaScript to change the text of an element with the `id` attribute `name`.



For a more in-depth look into everything about JavaScript, checkout [The JavaScript Handbook](https://www.freecodecamp.org/news/the-complete-javascript-handbook-f26b2c71719c/).

## JavaScript Projects for Beginners

When it comes to JavaScript, much like with everything else, **practice makes perfect**. The best way to learn JavaScript is to build projects, and there's no limit to projects you can build using JavaScript.

A good approach is to build things that solve problems. For instance, I was having trouble copying the content from a Word document into an HTML list, so I built a [Text-to-HTML Converter](https://jemimaabu.github.io/text-list-to-html/).

Building applications is also a good way to understand basic concepts in JavaScript and get some real-life experience with practical application.

These are some resources for ideas on projects to build when learning JavaScript:

1. [40 JavaScript Projects for Beginners](https://www.freecodecamp.org/news/javascript-projects-for-beginners/)
2. [Build 15 Javascript Projects](https://www.youtube.com/watch?v=3PHXvlpOkf4) (video)

## JavaScript Tutorials for Beginners

Aside from building projects, it also helps to follow a guided tutorial when trying to learn JavaScript.

Most courses attempt to cover all the important concepts you need to know, and some provide a certification at the end to help improve your résumé.

Here are some online courses and curricula that provide a good introduction to JavaScript, and a few that cover more advanced concepts.

## 1. [freeCodeCamp](https://www.freecodecamp.org/)

[![Learn to code — for free. Build projects. Earn certifications.](https://res.cloudinary.com/practicaldev/image/fetch/s--mRekYsEm--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/33qr41y1huz7oindki98.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--mRekYsEm--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/33qr41y1huz7oindki98.png)

If you have no prior coding knowledge, freeCodeCamp is an excellent place to begin your tech journey. They have a variety of courses, all explained in a simple-to-grasp format.

There is also a large online [forum](https://forum.freecodecamp.org/) which makes it easy to receive help or feedback from other people while taking the courses.

### Recommended course

1. [JavaScript Algorithms and Data Structures](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)

**Suited for:** Beginners

## 2. [Udemy](https://click.linksynergy.com/deeplink?id=i1rVYzXnF5I&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2F)

[![Learn from the best. Real-world experts teaching real-world skills](https://res.cloudinary.com/practicaldev/image/fetch/s--ErzvhWD9--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/olc5l55bi58o4ql6eyta.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--ErzvhWD9--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/olc5l55bi58o4ql6eyta.png)

Udemy is a paid online platform that offers courses for a wide range of skills. Most of the courses are short-term, and can be learned in your free time.

### Recommended courses

1. [The Complete JavaScript Course](https://click.linksynergy.com/deeplink?id=i1rVYzXnF5I&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-javascript-course%2F)
2. [JavaScript: Understanding the Weird Parts](https://click.linksynergy.com/deeplink?id=i1rVYzXnF5I&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Funderstand-javascript%2F)

**Suited for:** Beginner and intermediate learners

## 3. [Udacity](https://imp.i115008.net/oe4Lyn)

[![The latest digital skills, within reach. Discover the fastest, most effective way to gain job-ready expertise for the careers of the future](https://res.cloudinary.com/practicaldev/image/fetch/s--AyEWlOCN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/upogwujx7z3xusq52aru.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--AyEWlOCN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/upogwujx7z3xusq52aru.png)

Udacity is a tech-focused learning platform that offers Nanodegrees for their courses. Courses can take anywhere from 2 to 6 months, depending on how many hours you're able to commit. They're a great way of gaining in-depth knowledge in a skill relatively quickly.

### Recommended courses

1. [Learn Intermediate JavaScript](https://imp.i115008.net/DVyOqb)
2. [Full Stack JavaScript Developer](https://imp.i115008.net/rnaL0D)

**Suited for:** Intermediate developers

[![jemimaabu](https://res.cloudinary.com/practicaldev/image/fetch/s--yH8fXaQs--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://res.cloudinary.com/practicaldev/image/fetch/s--wi2sA3YA--/c_fill%2Cf_auto%2Cfl_progressive%2Ch_150%2Cq_auto%2Cw_150/https://dev-to-uploads.s3.amazonaws.com/uploads/user/profile_image/121416/b1bb7c72-d1f8-40a3-a484-baf01c44e461.JPG) ](https://dev.to/jemimaabu)[50 Online Resources To Improve Your Technical SkillsJemima Abu ・ May 3 '21 ・ 8 min read#career #webdev #beginners #productivity](https://dev.to/jemimaabu/online-resources-to-improve-technical-skills-56ma)

## 4. [Zero To Mastery](https://academy.zerotomastery.io/?affcode=441520_kedmgbvr)

[![Learn to Code. Get Hired.](https://res.cloudinary.com/practicaldev/image/fetch/s--zJStPI2e--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zwl9ns7in6xdomun22xv.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--zJStPI2e--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zwl9ns7in6xdomun22xv.png)

Zero To Mastery offers practical courses for people looking to advance their career. If you're looking to apply for a job or start a career in tech without any prior experience, it's helpful to learn a course that offers real-life usage. This is especially useful for the interview stage.

### Recommended courses

1. [JavaScript: The Advanced Concepts](https://academy.zerotomastery.io/a/aff_8pwrdc5q/external?affcode=441520_kedmgbvr)
2. [JavaScript Web Projects: 20 Projects to Build Your Portfolio](https://academy.zerotomastery.io/a/aff_j2jx8w4l/external?affcode=441520_kedmgbvr)

**Suited for:** Experts and professionals

## Conclusion

Despite how it seems, JavaScript is one of those things that gets easier with practice. Hopefully, this article will be a helpful starting point on your journey to JavaScript mastery.

If you found it useful or you have any other questions, feel free to reach out to me on [Twitter](https://twitter.com/jemimaabu) or send me a message on [my website](https://www.jemimaabu.com/).
