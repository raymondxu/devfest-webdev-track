<a id="top"></a>
# DevFest Web Development Track

<i>Learn web development with Flask.</i>

Written and developed by [Raymond](http://www.raymondxu.io), Matt, and [ADI](http://www.adicu.com).

<a href="#top" class="top" id="getting-started">Top</a>
## About This Document

### Methodology

This guide will teach you fundamental web development concepts by guiding you through the process of building a reading list app. We will begin by setting up a basic webpage in Flask, and then incrementally add features level by level. After finishing this tutorial, you will have built a fully-functional reading list app where you can display a list of your favorite books.



### Prerequisites

Basic knowledge of the Python programming language is suggested. If you don't already know Python, check out [this ADI-developed tutorial](). No knowledge of HTML, CSS or Flask is expected or required.


<a href="#top" class="top" id="table-of-contents">Top</a>
## Table of Contents

-	[Level 0: Environment Setup](#level0)
-	[Level 1: Hello World](#level1)
	-	[1.1 Subsection](#subsection)
-	[Level 2: CSS](#level2)
	-	[2.1 Subsection](#another-subsection)
-	[Level 3: APIs](#level3)
	-	[3.1 Subsection](#another-subsection)
-	[Level 4: Databases](#level4)
	-	[4.1 Subsection](#another-subsection)
-	[Level 5: User sessions](#level5)
	-	[5.1 Subsection](#another-subsection)
-   [Additional Resources](#additionalresources)


------------------------------
<a href="#top" class="top" id="section">Top</a>
## Level 0: Environment Setup

Before we get started, follow [this guide](http://learn.adicu.com/setup/) to set up your environment.


<a href="#top" class="top" id="level1">Top</a>
## Level 1: Hello World


<a href="#top" class="top" id="what-is-flask">Top</a>
## 1.1 What is Flask

[Flask][flask] is a Python microframework.  Microframeworks are bare-bones, customizable tools that make it easy to build web apps, and Flask lets us do this in Python.  It is very easy to setup and has excellent documentation on its [website][flask].

<a id="how-a-flask-app-works"></a>
### 1.1.1 How a Flask App Works

Flask works with a [client-server model][client-server].  The server, written in Python, has functions that take requests from clients (i.e. your web browser) and return web content to be displayed by the client.  

Think about it like a restaurant:  The patron (the client, or web browser) gets their meal (the web page) by telling it to the waiter (the Flask server).  Then, depending on the order (the request), the waiter gives it to the cook (back-end functions, optional) and then the server returns the cooked meal to the patron. 

In addition to serving static pages (meals off the menu), Flask servers have the ability of serving [dynamic web pages][dynamic-content], or pages that are generated every time you load the page (made to order).  For example, dynamic content could be of such pages include information stored in a database or a user account.

<a id="the-anatomy-of-a-flask-app"></a>
### 1.1.2 The Anatomy of a Flask App

This is a very basic directory structure for a Flask webapp.

	ProjectDirectory/
	├── app.py
	├── requirements.txt
	├── static/
	│   ├── css/
	│   ├── img/
	│   └── js/
	└── templates/

-	`ProjectDirectory/` - Everything for your app goes in this folder.  Rename this to the name of your app.
-	`app.py` - All of the Python/Flask code and server logic gets written in this file.
-	`requirements.txt` - A list of all of the dependencies for your project.  See more about dependencies and installing them in [section 6.3 of our Python tutorial](http://adicu.com/intro-webdev/python/#pip).
-	`static/` - This folder holds all your static files.  Static files include:
	-	`js/` - Javascript files, which allow interactive web content. We'll talk about these [later](#installation-and-template-setup).
	-	`css/` - CSS files, which style our app.  We'll talk about these [later](#css) too.
	-	`img/` - Image files.
-	`templates/` - This folder holds all your Flask templates.  Our HTML files will go here.  There are special features offered by Flask that make templates different than basic HTML files, explored in [Section 3.2](#templating-in-flask).

<a href="#top" class="top" id="hello-world-in-flask">Top</a>
## 1.2 Hello World in Flask

In order to write our first Flask app, we only need to edit one file: `app.py`.  It's that easy!

<a id="editing-app-py"></a>
### 1.2.1 Editing app.py

First, import the `Flask` class from the `flask` module.

```python
from flask import Flask
```

Then, construct the Flask `app` variable.  We'll pass around this variable whenever we want to access information about the server. We pass `__name__` into the `Flask()` function so that your flask app is associated with the directory structure we created in [Section 1.1](#the-anatomy-of-a-flask-app).

```python
app = Flask(__name__)
```

Next, apply the `@app.route("/")` [decorator][decorators] to a function called `hello()`.  It returns just the string `"Hello World!"`.  By doing this, we are making a [route][route].  The `route()` decorator binds the URL `http://yourwebapp.com/` to this function, effectively adding a new page to your app.  Functions with the `route()` decorator can return text strings or HTML, and whatever is returned will be displayed by the client.

```python
@app.route("/")
def hello():
    return "Hello World!"
```

Finally, call `app.run(host="0.0.0.0")` when the file is executed.  Once `app.run(host="0.0.0.0")` is called, the server will start accepting requests from the client.

```python
if __name__ == "__main__":
    app.run(host="0.0.0.0")
```

This leaves us with a completed Hello World program in Flask:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run(host="0.0.0.0")
```

<a id="running-a-flask-app"></a>
### 1.2.2 Running a Flask App

To run our Hello World app, just type the following command in the Project Directory folder:

```bash
$ python app.py
```

If it's working, it should print the lines:

```bash
* Running on http://127.0.0.1:5000/
```

Point your browser to that URL and bask in the awesomeness!

> Wondering what that weird URL is?  `127.0.0.1` is the IP address for [localhost][localhost], or your local computer.  When we run a Flask server with `python app.py`, it is running only on your machine, not the internet.  The `:5000` bit is the [port][port], or specific place where your app is running.  Developing locally is much easier and safer than publishing your app to the internet every time you want to test something, and is considered good practice.  Also, instead of typing out `http://127.0.0.1:5000` every time you want to see your app, you can also point your browser to `http://localhost:5000`.  The two are equivalent.

<a id="developing-with-flask"></a>
### 1.2.3 Developing with Flask

Flask is great for development.  It offers very helpful error messages and prints stack traces well in the browser, if instructed to.  To enable these debugging features and (more importantly) automatic reload, edit `app.py` and add a statement configuring the `app` object.

```python
from flask import Flask

app = Flask(__name__)
app.config["DEBUG"] = True  # Only include this while you are testing your app

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run(host="0.0.0.0")
```

With this modification, edit the string returned by the `hello()` function and refresh your browser to watch it change!  When you run the server now, it should also print:

	* Restarting on reloader

<a href="#top" class="top" id="working-with-routes">Top</a>
## 1.3 Working with Routes

Let's define a few more routes for our app. Again, [routes][route] are paths that can be visited by the user of the app. We use the [decorator][decorators] `app.route("/somepath")` to tie the decorated function to the path given in the parenthesis.

<a id="static-routes"></a>
### 1.3.1 Static Routes

First, lets make a route that lets the client get the name of the creator of this app.  Start by defining a function called `name()`, and have it return your name as a string.


```python
def name():
  return "Your Name"
```

Now we'll apply the decorator `app.route()`.  Inside the parenthesis for the decorator, include the path `"/name"`.  Paths in Flask always start with a `/`.

```python
@app.route("/name")
def name():
  return "Your Name"
```

Save.  With your server [running](#running-a-flask-app) or [reloaded](#developing-with-flask),  point your browser to `"http://localhost:5000/name"` and your name will appear!  Our `/name` route is static, because it returns the same string every time.

Now, make another static route accessible at `http://localhost:5000/website` that returns the URL of your Github, Twitter, or personal website (don't forget the `http://`!). We'll use this route later.

> We won't show the code for the website route here, but it's implementation is in the sample code for your reference.

<a id="dynamic-routes"></a>
### 1.3.2 Dynamic Routes

Dynamic routes are what make using Flask so valuable.  Start off by making a static route called `search`.  It can return any string you want.  We'll edit it to return the results of our web search.

```python
@app.route("/search")
def search():
  return "Search"
```

To make our route dynamic, first we will modify the url to take a variable parameter named `search_query`.

```python
@app.route("/search/<search_query>")
```

Then, modify the `search` function to take a string variable `search_query`, and return that.

```python
@app.route("/search/<search_query>")
def search(search_query):
  return search_query
```

Save and reload your server as needed, and navigate to `http://localhost:5000/search/test` and see `test` appear as the returned page.  If you change what comes after the `/search/` in the URL, it will be displayed in the browser.  We will soon modify this route to return actual search results.

<a id="the-add-route"></a>
### 1.3.3 Extension: The Add Route

Let's make a dynamic route that adds two numbers (just for practice).  Just like with all the other routes, we'll start by writing the associated function.

```python
def add(x, y):
	return x + y
```

Now, we'll apply our decorator.  What goes inside the parenthesis? We'll start with `"/add"`, and then we add `/<varname>` for each variable, like so:

```python
@app.route("/add/<x>/<y>")
def add(x, y):
	return x + y
```

This might look good, but if you try and visit `localhost:5000/add/3/4`, you'll see `34` displayed!  Why is this? Flask treats all variables as strings, so we were performing string concatenation, not integer addition!

Let's change our function so it adds `x` and `y` as integers:

```python
@app.route("/add/<x>/<y>")
def add(x, y):
	return int(x) + int(y)
```

Great!  Or maybe not.  If we run our server and visit `localhost:5000/add/3/4`, we'll see `TypeError: 'int' object is not callable`.  This is because we can *only return strings*.  Convert our sum back to string after summing and we're done.

```python
@app.route("/add/<x>/<y>")
def add(x, y):
	return str(int(x) + int(y))
```

<a id="custom-error-messages"></a>
### 1.3.4 Extension: Custom Error Messages

We now have a good deal of paths that users of our app can visit: `/`, `/name`, `/website`, `/search/<search_query>`, and `/add/<x>/<y>`.  But what happens if the user navigates to a path other than these?

With your server reloaded, point your browser to `localhost:5000/test` (which we don't have a route for).  You'll see an error message that says "Not Found".  This error is called a *404 error* (we'll go into why it's called "404" in [section 2.1.5](#http)).  A 404 error is shown whenever the client requests a path that the server doesn't support.  

With Flask, we can create custom 404 error pages.  To do this, we use the `app.errorhandler(code)` decorator instead of `app.route(path)`, and modify what we return slightly.

First, create a function to handle the 404 error.

```python
def page_not_found():
	return "Sorry, this page was not found."
```

Next, apply the decorator, passing in the error code `404`. We'll also need to add a function parameter into the `page_not_found` function as well; let's call this `error`.

```python
@app.errorhandler(404)
def page_not_found(error):
	return "Sorry, this page was not found."
```

When we implement error pages in Flask, we also have to indicate to the client's browser that we are returning an error message.  To do this, we return a [tuple](python/#tuples) containing the message and the error code:

```python
@app.errorhandler(404)
def page_not_found(error):
	return "Sorry, this page was not found.", 404
```

Again, the 404 error code tells the browser that the page was "Not Found".  Visit `localhost:5000/test` to see our custom message in action!


<a href="#top" class="top" id="level2">Top</a>
## Level 2: CSS

[CSS][css], or Cascading Style Sheets, is a styling language that is used to arrange and stylize HTML elements.  CSS is extremely powerful, but also fairly hard to learn.  Every different browser interprets CSS slightly differently, and there are a lot of tricks and best practices that are hard to learn.  As such, CSS is best learned by lots and lots of practice.  The [ADI Resources][learn] page has links to a lot of different tutorials and walkthroughs, if you want more practice after styling your Flask app.


<a href="#top" class="top" id="css-basics">Top</a>
## 2.1 CSS Basics

CSS is a very simple language.  At it's core, CSS is made up of three parts: *selectors*, *properties*, and *values*.  Selectors (explored in depth in [section 4.1.2](#advanced-selectors)) are used to select which elements are being styled.  For example:

```css
p { }
```

selects all `<p>` elements.

Inside the braces `{ }` are the *properties* and *values* separated by colons `:`, with semicolons `;` at the end of each property / value pair.  For example:

```css
p { color: blue; }
```

would make all the `<p>` tags blue. Here, `color` is a property and `blue` is a value.  In terms of syntax, that's really it.  If we wanted to make all `<p>` tags blue and all `<strong>` tags red, here would be our CSS:

```css
p {
	color: blue;
}
strong {
	color: red;
}
```

Notice that whitespace is not relevant to syntax.  Finally, comments should be surrounded in `/* */`.

```css
/* this is a comment */
```

There is an extremely large collection of CSS properties and values, all of which are documented excellently at (surprise) the [Mozilla Developer Network][mdn].  Again, avoid incorrect or out of date information by appending `mdn` to any Google searches related to CSS you might have!


<a id=""></a>
### 2.1.1 Applying CSS Styles

There are three ways to apply CSS to HTML elements.  The first is called *inline styling*.  Every element can be given a `style` attribute that can take CSS styles that apply to this element.  Doing this, we can avoid selectors entirely because all of our styling effects only the element that we are adding the `style` attribute to.  For example, this `<p>` tag will have blue text:

```html
<p style="color: blue">This text is blue.</p>
```

Inline styles are problematic, however.  Not only do we clutter the text that is being displayed, but also we would have to duplicate our styling for every paragraph we have!

```html
<p style="color: blue">This text is blue.</p>
<p style="color: blue">An so is this text.</p>
<p style="color: blue">And this text.</p>
```

Because of this, inline styles should be *absolutely avoided* unless you are writing for mail clients that ignore CSS that isn't inline.

We can also apply CSS in a `<style>` element, in the `<head>`, like so:

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>CSS Demo</title>
		<style>
p {
	color: blue;
}
		</style>
	</head>
	<body>
		<p>This text is blue.</p>
		<p>An so is this text.</p>
		<p>And this text.</p>
	</body>
</html>
```

This is great, because the body of the document is a lot less cluttered.  As you can imagine, this is only relocates the problem.  now the head of the document is messy and full of CSS styles.  As your HTML gets longer so does your CSS, and now documents can reach unmanageable length.

For the same reason we might break up a large programming project into classes and functions, we want to get our CSS out of our `.html` document entirely.  Enter the `<link>` tag, which lets us reference external `.css` stylesheets!  Link tags only need an open tag, and have no content or end tag.  Also, be sure to always include the `rel` attribute as 'stylesheet' and the `type` attribute as `text/css` alongside the `href` attribute pointing to the `.css` file.

<h1 class="join"></h1>
```html
<!DOCTYPE html>
<!-- demo.html -->
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>CSS Demo</title>
		<link href="demo.css" 
			rel="stylesheet"
			type="text/css">
	</head>
	<body>
		<p>This text is blue.</p>
		<p>An so is this text.</p>
		<p>And this text.</p>
	</body>
</html>
```
```css
/* demo.css */
p {
	color: blue;
}
```
<h1 class="clear"></h1>

Using an external CSS file is the best practice, barring special circumstances.  You can even include multiple CSS files to keep things organized.

```html
<!DOCTYPE html>
<!-- demo.html -->
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>CSS Demo</title>
		<link href="blue.css" rel="stylesheet" type="text/css">
		<link href="red.css" rel="stylesheet" type="text/css">
	</head>
	<body>
		<p>This text is blue.</p>
		<strong>And this text is red.</strong>
	</body>
</html>
```

<h1 class="join"></h1>
```css
/* blue.css */
p {
	color: blue;
}
```
```css
/* red.css */
strong {
	color: red;
}
```
<h1 class="clear"></h1>


### 2.1.2 Selectors

In the above examples `p` and `strong` in the `.css` files are *selectors*.  Selection can be done in a variety of different ways.  The most basic selection is by element name.

<h1 class="join"></h1>
```html
<!-- demo-name.html -->
<p>This text is blue</p>
<strong>This text is not</strong>
```
```css
/* demo-name.css */
p {
	color: blue;
}
```
<h1 class="clear"></h1>

If you provide a `class` attribute, the `.classname` syntax will select all elements with `class="classname"`.  Elements can have multiple classes using the syntax `class="classname1 classname2"`.

<h1 class="join"></h1>
```html
<!-- demo-class.html -->
<p class="blue">This text is blue</p>
<p>This text is not blue</p>
<p class="blue underline">
	This text is blue and underlined.
</p>
```
```css
/* demo-class.css */
.blue {
	color: blue;
}
.underline {
	/* Use the "text-decoration"
	property to underline text. */
	text-decoration: underline;
}
```
<h1 class="clear"></h1>

You can also give an element a unique `id` and it can be selected with the `#idname` syntax.  Do not give two elements the same id.

<h1 class="join"></h1>
```html
<!-- demo-id.html -->
<p id="blue">This text is blue</p>
<p>This text is not blue</p>
```
```css
/* demo-id.css */
#blue {
	color: blue;
}
```
<h1 class="clear"></h1>

There are also *relational selectors* for finding elements in the HTML document.  

Using the *descendant* selector syntax, or `selector1 selector2`, you can restrict `selector2` to only elements that are descendants of an element selected by `selector1`. For example:

<h1 class="join"></h1>
```html
<!-- demo-descendant.html -->
<div class="blue">
	<h1>This text is not blue</h1>
	<p>This text is blue</p>
	<span>
		<p>This text is also blue</p>
	</span>
</div>
<p>This text is not blue</p>
```
```css
/* demo-descendant.css */
.blue p {
	color: blue;
}
```
<h1 class="clear"></h1>

Similarly, the child selector syntax (`selector1 > selector2`) will restrict `selector2` to only children (not grandchildren or other descendants) of elements selected by `selector1`.  For example:

<h1 class="join"></h1>
```html
<!-- demo-child.html -->
<div class="blue">
	<h1>This text is not blue</h1>
	<p>This text is blue</p>
	<span>
		<p>This text is NOT blue,
			because its parent is the 
			span tag, which does not 
			have the "blue" class.
		</p>
	</span>
</div>
<p>This text is not blue</p>
```
```css
/* demo-child.css */
.blue > p {
	color: blue;
}
```
<h1 class="clear"></h1>

Finally, you can combine two selectors with a comma (`,`).  These two CSS files are the same:

<h1 class="join"></h1>
```css
/* demo.css */
p {
	color: blue;
}
.blue {
	color: blue;
}
```
```css
/* demo.css */
.blue, p {
	color: blue;
}
```
<h1 class="clear"></h1>

<a id="basic-properties-and-values"></a>
### 2.1.3 Basic Properties and Values

Learning CSS, for the most part, is about learning the knitty gritty details.  For an in-depth, comprehensive walkthrough of CSS properties and and how to apply them, check out [HTML Dog][htmldog]'s excellent [CSS tutorial series][htmldog-css]. In [section 4.2](#external-libraries), we will be applying CSS en-masse, using external libraries that provide shortcuts to a stylized webpage.  While using these libraries is good practice, these libraries always need to be accompanied by some extra CSS code for your own website.  For this reason, it's important to understand some basic CSS properties.

#### Color and Background-Color

You can change the background color of an element with the `background-color` attribute, just as you can change the text color with the `color` attribute.  There are many CSS properties that have colors as values, and for all of them colors can be represented in multiple ways ([detailed in-depth at MDN](mdn-colors)), the most simple (and limited) being *keywords* (like `blue`, `red`, etc.).

<h1 class="join"></h1>
```html
<!-- demo-color.html -->
<body>
	<p class="blue">
		This text is blue.
	</p>
	<p class="b-and-y">
		This text is yellow on black.
	</p>
</body>
```
```css
/* demo-color.css */
.blue { 
	color: blue;
}
.b-and-y {
	color: yellow;
	background-color: black;
}
```
<h1 class="clear"></h1>


#### Height and Width

The most basic CSS property is `height` and `width`.  They change the size of the *content area* of the selected element.  Height and weight take measurements of length ([detailed in depth at MDN][mdn-length]), the simplest being *pixels*, or *px*. 

<h1 class="join"></h1>
```html
<!-- demo-height-width.html -->
<body>
	<div class="blue first"></div>
	<div class="red second"></div>
	<div class="blue third"></div>
</body>
```
```css
/* demo-height-width.css */
.blue { background-color: blue; }
.red { background-color: red; }
.first {
	height: 100px;
	width: 200px;
}
.second {
	height: 150px;
	width: 150px;
}
.third {
	height: 50px;
	width: 300px;
}
```
<h1 class="clear"></h1>

![demo-height-width](img/demo-height-width.png)

#### Borders

You can also give your elements borders.  To create a visible border, you need to set three properties for the selected element: `border-width`, `border-style`, and `border-color`.  These three properties can be combined into one `border` attribute.  The values `width`, `style`, and `color` can be provided in any order.  The border will wrap around the box, not the text (note that the third resized `<p>` has a square border).

<h1 class="join"></h1>
```html
<!-- demo-height-width.html -->
<body>
	<p class="first">
		Black border, 3px wide.
	</p>
	<p class="second">
		Same as above
	</p>
	<p class="third">
		A thick, dashed blue border.
	</p>
</body>
```
```css
/* demo-border.css */
.first {
	border-width: 3px;
	border-style: solid;
	border-color: black;
}
.second {
	border: 3px solid black;
}
.third {
	height: 100px;
	width: 100px;
	border: 10px dashed blue;
}
```
<h1 class="clear"></h1>

![demo-border](img/demo-border.png)

#### Margin and Padding

Setting the `margin` and `padding` attributes for an element creates space around it.  The content area (changed by the `height` and `width` properties) border, margin, and padding make up the *box model*.  

![boxmodel](img/boxmodel.png)

*Photo credit to the [MDN page on the box model][mdn-box-model].*

For the most part, margin is used to create space outside of the element, and padding is used to make space inside the element.  The border sits just between the two.  

You can set margins with the `margin-top`, `margin-right`, `margin-bottom`, and `margin-left` attributes, and you can set padding with the `padding-top`, `padding-right`, `padding-bottom`, and `padding-left` attributes.  Values for these attributes are in units of length, and can even be negative.

The four margin and padding attributes can each be combined into `margin` and `padding`, and depending on how many arguments are provided, different of these properties will be set.  See the table for the details of how this works (`margin` is used, but the same goes for `padding`).

CSS | Shorthand | `-top` | `-right` | `-bottom` | `-left`
----|----|----|----|----|----
`margin: 1px 2px 3px 4px` | `T R B L` | `1px` | `2px` | `3px` | `4px`
`margin: 1px 2px 3px`     | `T R&L B` | `1px` | `2px` | `3px` | `2px`
`margin: 1px 2px`         | `T&B R&L` | `1px` | `2px` | `1px` | `2px`
`margin: 1px`             | `T&R&B&L` | `1px` | `1px` | `1px` | `1px`

Here are a couple different examples of padding and margins in action:

<h1 class="join"></h1>
```html
<!-- demo-height-width.html -->
<body>
	<div class="first">One</div>
	<div class="second">Two</div>
	<div class="third">Three</div>
</body>
```
```css
/* demo-margin-padding.css */
body {
	/* so that the effects of margins on the divs are easier to see. */
	margin: 0;
	padding: 0;
	border: 50px solid green;
}
div {
	/* Every div starts off as a yellow square with a black border. */
	height: 50px;
	width: 50px;
	background-color: yellow;
	border: 2px solid black;
}
.first {
	/* Offset from the wall on the left. */
	margin-left: 20px;
}
.second {
	/* Offset from block one by 50px. */
	margin-top: 50px;
	/* And the wall by 5px. */
	margin-left: 5px;
	/* 20px larger in each dimension because padding is inside the border, but text is offset by 20px (that's why the text doesn't hug the border). */
	padding:20px;
}
.third {
	/* Offset from the bottom by 10px, and 25px higher and to the left than expected because of the negative values. */
	margin: -25px 0 10px -25px;
	/* Taller with the text separated from the top of the box. */
	padding-top:20px;
}
```
<h1 class="clear"></h1>

![demo-margin-padding](img/demo-margin-padding.png)

<a id="using-the-inspector"></a>
### 2.1.4 Using the Inspector

One of the most important skills to learn as a web developer writing CSS is to learn how to use the inspector in your favorite browser (Internet Explorer not allowed).  The inspector lets you see the HTML, CSS, and JavaScript that your web browser is rendering, live!  You can inspect your own web page to find bugs or make tweaks to your code, or inspect other pages to learn how to imitate a desired HTML/CSS/JS effect.

With a web page open in FireFox, there are [three ways to inspect the page][inspect-ff]:

-   Choose the "Inspector" option from the "Web Developer" menu (which is a submenu in the "Tools" menu on the Mac).
-   Press `Ctrl`+`Shift`+`C` (`Cmd`+`Option`+`C` on the Mac OS X and Linux).
-   Right-click an element on a web page and select "Inspect Element".

With a web page open in Chrome, there are [three ways to inspect the page][inspect-chrome]:

-   Select the Chrome menu  at the top-right of your browser window, then select Tools > Developer tools.
-   Use `Ctrl`+`Shift`+`I` (or `Cmd`+`Opt`+`I` on Mac) to open the DevTools.
-   Right-click on any page element and select Inspect element.

To [inspect a page in Safari][inspect-safari], you first have to enable the Develop menu. Go into Safari's preferences, and check the “Show Develop menu in menu bar” checkbox in the "Advanced" pane. Then, you open the inspector in three ways:

-   Choose the "Show Web Inspector" option in the "Develop" menu.
-   Press `Cmd`+`Option`+`I`.
-   Right-click an element on a web page and select "Inspect Element".


<a id="adding-css-to-our-project"></a>
### 2.2 Applying CSS to Our Project

Now that you've learned the basics of CSS, let's add some styling to our web page!

# +CONTENT


<a href="#top" class="top" id="level3">Top</a>
## Level 3: APIs




<a href="#top" class="top" id="level4">Top</a>
## Level 4: Databases


<a href="#top" class="top" id="level5">Top</a>
## Level 5: User sessions


<a href="#top" class="top" id="additionalresources">Top</a>
## Additional Resources

Along with this tutorial, there is a wealth of information available on Python all across the web. Below are some good places to start:

- [ADI Resources][learn]
- [Codecademy][codecademy]



[github]: https://github.com/RaymondXu/devfest-webdev-track.git
[learn]: http://adicu.com/learn
[codecademy]: http://www.codecademy.com
[adi]: http://adicu.com
 
