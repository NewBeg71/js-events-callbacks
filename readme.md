# TODO
* Flesh out the post-Color Picker `this` section. First, show non-`this` way of doing it. Then talk about `this`, and make sure to `console.log(this)` at some point.
* Can we not pass in the word "event" as an argument to an event listener?
* Is there a better example than `<a>` to demonstrate default event behavior (e.g., button)?
* Add more sample quiz questions

# Events and Callbacks

## Screencasts
* [Robin's Screencast](https://youtu.be/S4Xvo_m6P04)
* [Andy's Screencast - p1](https://www.youtube.com/watch?v=xogI6prB-PI)
* [Andy's Screencast - p2](https://www.youtube.com/watch?v=Srd2Tx1Z7v8)

## Learning Objectives
* Explain the concept of a 'callback' and how we can pass functions as arguments to other functions.
* Describe the difference between asynchronous and synchronous program execution.
* Explain why callbacks are important to asynchronous program flow.
* Identify when to reference a function and when to invoke a function.
* Describe what an anonymous function is and when you would use one.
* Pass a named function as a callback to another function.
* Pass an anonymous function as a callback to another function.

## Framing (5 minutes / 0:05)
In order to do things on the client side and give our web applications behavior, we need programmatic access to HTML and CSS using Javascript. Enter the **document object model**, more commonly known as **the DOM**. This powerful tool allows Javascript to interface with our HTML. Now we have the ability to generate functionality that can act on HTML elements or be activated by HTML elements.

But first, a quick review. An **object** in the real world has different states and behaviors.

* Dogs have state (name, color, breed, hungry) and behavior (barking, fetching, wagging tail).
* Bicycles also have state (current gear, current pedal cadence, current speed) and behavior (changing gear, changing pedal cadence, applying brakes).

Software objects are conceptually similar to real-world objects: they too consist of states and related behaviors. Why bring objects up? What we're doing today is mapping HTML elements into JS objects so that we can change their state, or give them behavior.

## Set Up (5 minutes / 0:10)

For this class we'll be working with only two files: `index.html` and `script.js`. Create these files in your in-class folder...

```bash
$ touch index.html script.js
```

Next, set up your `index.html` file, making sure to link to `script.js`...

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
  <head>
    <title>Events and Callbacks Practice</title>
  </head>
  <body>
    <button>Click me!</button>
    <script src="script.js"></script>
  </body>
</html>
```

Now let's put a simple line of code in `script.js` to make sure it's properly linked to `index.html`...

```js
// script.js

console.log( "I'm working!" );
```

## Events (15 minutes / 0:25)

Our goal today is to make it so that when a given event occurs in our web application, the application then responds to that event.

**What is an event?** Can you answer that questions by looking at some examples?
* `click`
* `mouseover`
* `keypress`
* `scroll`
* `submit`
> Here's an [extensive list](https://developer.mozilla.org/en-US/docs/Web/Events) of Javascript events.  

In order to run code in response to any of these events, we need to define an **Event Listener**. Below you'll find a simple event listener. It's purpose? Print a message to the console whenever a button is clicked...

```js
var button = document.querySelector( "button" );

function handleClickEvent(){
  console.log( "I was clicked!" );
}

// This is the event listener!
button.addEventListener( "click", handleClickEvent );
```

Let's go through the above code examples line-by-line...

#### Selecting the Element

When want our "click handler" -- what we're calling an event listener that listens for a click event -- to trigger everytime the button is clicked. In order for this to happen, we need to represent that button in Javascript. We can "select" that button using a Javascript selector like [`.querySelector()`](http://www.w3schools.com/jsref/met_document_queryselector.asp). You'll learn more about these in the DOM class.

```js
var button = document.querySelector( "button" );
```
> `.querySelector()` selects the first HTML element that matches the passed-in argument. In the above example, that is `<button>`.  

Now the `button` variable contains a reference to the button that exists on our page.  

#### Defining the Behavior

When our button is clicked, we want some Javascript code to run that prints a message to the console. We are going to encapsulate that code in a function. We'll do something with it later.

```js
function handleClickEvent(){
  console.log( "I was clicked!" );
}
```

#### Creating the Event Listener

Now for the big step: linking our behavior with a button click. Let's look at that event listener again and go over each component...

```js
button.addEventListener( "click", handleClickEvent );
```

##### `button`

The first component is the HTML element we are applying the listener to. In this case, that is the `<button>`, which is stored in our `button` variable.

##### `.addEventListener`

Next is the Javascript method that allows us to create event listeners: `.addEventListener`. It takes two arguments...

##### `"click"`

The first argument is where we indicate what type of event we are listening to. In this case, that is a "click". We could replace this with the examples we mentioned earlier in class, like `"submit"` or `"mouseover"`.

##### `handleClickEvent`

The second argument is where we indicate what we want to happen once the event occurs. In this case, that is everything stored in the `handleClickEvent` function we defined earlier.


### Before We Go On... (5 minutes / 0:30)

Usually when we do anything with functions, we put parentheses after the function name. Here, we have `handleClickEvent` without any parens.

Try adding parentheses at the end of this line:

```js
button.addEventListener("click", handleClickEvent())
```

Refresh your page. What was different? Why?

> You'll notice that "I was clicked!" pops up immediately upon reload. Also note that the event while it does fire, isn't doing anything. When we include `()` we invoke the function expression. Without the `()`, we're using the function expression as a reference.

## Callbacks (20 minutes / 0:50)
This might not be your first time hearing it, and definitely won't be your last. A callback is a piece of executable code that is passed as an argument to other code, which is expected to invoke (or "call back") that executable code at some convenient time.

The invocation may be immediate or it might happen later. In the example above, `handleClickEvent` is our callback. The invocation happens when the button is clicked.

### Code Along

Copy and paste this into your `script.js`...

```js
var button = document.querySelector("button")
var handleClickEvent = function(){
  console.log("I was clicked!")
}
button.addEventListener("click", handleClickEvent)
button.addEventListener("click", handleClickEvent)
```

Test it out. Notice how the callback only happens once, instead of twice? That's because both callbacks have the same name: `handleClickEvent`. Because of this, Javascript thinks they're the same thing.

Now create two different callbacks, one called `handleClickEvent` and one called `otherClickEventHandler`...

```js
var button = document.querySelector("button")
var handleClickEvent = function(){
  console.log("I was clicked!")
}
var otherClickEventHandler = function(){
  console.log("I'm firing on the same click!")
}
button.addEventListener("click", handleClickEvent)
button.addEventListener("click", otherClickEventHandler)
```

Both fire on one click. That's because they have different names.

> tl;dr: You are unable to add the same function call to the same event to execute functionality twice in 1 click event. Unless you use an anonymous function...  

## Anonymous functions (10 minutes / 1:00)

Now try using what's called an **anonymous function** -- a nameless function -- to make "I was clicked!" get printed to the console twice...

```js
var button = document.querySelector("button")
button.addEventListener("click", function(){
  console.log("I was clicked!")
})
button.addEventListener("click", function(){
  console.log("I was clicked!")
})
```

Those two functions look identical, so how come Javascript fired the event twice?

When functions don't have a name, Javascript sort-of "makes up" random names for them. So even though the functions look the same to *us*, Javascript sees them as being different.

This gives us a way of "cheating" to make `handleClickEvent` fire twice...

```js
var button = document.querySelector("button")
var handleClickEvent = function(){
  console.log("I was clicked!")
}
button.addEventListener("click", handleClickEvent)
button.addEventListener("click", function(){
  handleClickEvent()
})
```

Or with two anonymous functions...

```js
var button = document.querySelector("button")
var handleClickEvent = function(){
  console.log("I was clicked!")
}
button.addEventListener("click", function(){
  handleClickEvent()
})
button.addEventListener("click", function(){
  handleClickEvent()
})
```
## Break (10 minutes / 1:10)

## You Do: Color Scheme Switcher (30 minutes / 1:40)

Clone this repo and follow the readme instructions: **[Color Scheme Switcher](https://github.com/ga-dc/color-scheme-switcher)**.

## `this`

Here's the shortest way to do it...

```js
var buttons = document.querySelectorAll("li");
for(i in buttons){
  buttons[i].addEventListener("click", function(){
    document.body.className = this.className;
  });
}
```

Here we make use of the `this` keyword. In the context of an event listener callback, `this` always refers to the object that triggered the event.

## The Event Object (10 minutes / 1:50)

Back in the code we were using in-class...

```js
var button = document.querySelector("button")
var handleClickEvent = function(){
  console.log("I was clicked!")
}
button.addEventListener("click", handleClickEvent);
```

Now, you're going to make a small change by adding an argument to the anonymous function and printing it to the console...

```js
var button = document.querySelector("button")
var handleClickEvent = function(evt){
  console.log("I was clicked!")
  console.log(evt)
}
button.addEventListener("click", handleClickEvent);
```

The `evt` stands for `event`.
> The reason we're not actually using `event` is that it's a "reserved word" in Javascript, like "if" and "return".

#### Turn and Talk (5 minutes / 1:55)

With your partner, try clicking the button and exploring what properties the MouseEvent object contains. Look for...

* A way to figure out what element was clicked on.
* A way to tell the position of the mouse when it clicked.

### Key Events (15 minutes / 2:10)

Let's explore some other events. Add a text input field into the HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Events and Callbacks Practice</title>
  </head>
  <body>
    <button>Click me!</button>
    <input placeholder="Type here!" />
    <script src="script.js"></script>
  </body>
</html>
```

#### You Do

With a partner, add an event listener for the `keyup` event to the input. Explore the `event` object again. **Can you find a way to tell which key was pressed?**

#### We do

Your code should look something like this...

```js
var button = document.querySelector("button")
var input = document.querySelector("input")
var handleClickEvent = function(evt){
  console.log("I was clicked!")
  console.log(evt)
}
var handleKeyboardEvent = function(evt){
  console.log("You used the keyboard!")
  console.log(evt)
}
button.addEventListener("click", handleClickEvent);
input.addEventListener("keyup", handleKeyboardEvent);
```

A cross-browser way of telling which key is pressed is using the `keyCode` property. For `d`, `evt.keyCode` is `68`. For Shift, it's `16`.

#### You Do

Find the keyCodes for...
* Enter
* Tab
* Delete

#### You Do

There are several other events that come up with the `input` tag. See if you can figure out the difference between:

* `keyup`
* `keydown`
* `keypress`
* `change`
* `focus`
* `blur`

There are a bunch of different browser events you can use in Javascript, all [listed at W3Schools](http://www.w3schools.com/jsref/dom_obj_event.asp).

> Some programmers have qualms with W3Schools since they're mooching off the name of the W3 without actually being related to them. However, this list is accurate and easy-to-read.

### &larr; &larr; &rarr; &rarr; &uarr; &darr; &uarr; &darr; a b &larrhk;

There's a famous cheat code used in old Konami arcade games: you press up, up, down, down, left, right, left, right, 'b', 'a', and then 'start'. This code is so famous that many websites have secret functions for when users enter the Konami Code (with 'Enter' replacing 'start').

Try it out at [Konami Code Sites](http://konamicodesites.com/).

Some of those are now defunct. But my personal favorite is [Vogue.co.uk](http://www.vogue.co.uk/)

This is possible on these websites thanks to Javascript event listeners.

#### Turn and Talk

You've seen how to detect one keypress. **How might you make something happen after a specific series of keypresses?**

## Event Defaults (5 minutes / 2:15)

Back in the code we were using in-class, replace your button with a link to Google...

```html
<body>
  <a href="http://google.com">Click me!</a>
</body>
```

Now, add an event listener to that link that brings up a `prompt` box, asking the user if they want to go to Google...

```js
var link = document.querySelector("a")
var handleClickEvent = function(e){
  var input = prompt("You sure you want to go to Google?")
}
link.addEventListener("click", handleClickEvent);
```

The problem is we don't know how to stop them from going to Google! They go anyway, whether they hit "OK" or "Cancel".

Some elements, like `<a>`, have a default action they perform. In this case, that action is "going to another webpage." You can prevent that default action with an Event property called `preventDefault`.

```js
var link = document.querySelector("a")
var handleClickEvent = function(e){
  e.preventDefault();
  var input = prompt("You sure you want to go to Google?")
}
link.addEventListener("click", handleClickEvent);
```

Now, no matter what the user clicks, they won't go to Google.

In order to make it so they that **do** go to Google on clicking OK, but **don't** on clicking 'Cancel', we can use the fact that when you click 'Cancel' on a `prompt`, it returns `null`...

```js
var button = document.querySelector("a")
var handleClickEvent = function(e){
  if(prompt("You sure you want to go to Google?") === null){
    e.preventDefault();
  }
}
button.addEventListener("click", handleClickEvent);
```

## Drag and Drop (5 minutes / 2:20)

**Q: When else might we want to prevent default event behavior?**

Replace your `script.js` with the following code...

```js
var button = document.querySelector("button")
var elementStartX, elementStartY;
var mouseStartX, mouseStartY, mouseCurrentX, mouseCurrentY;
button.addEventListener("mousedown", startDragging);
function startDragging(e){
  elementStartX = button.offsetLeft;
  elementStartY = button.offsetTop;
  mouseStartX = e.clientX;
  mouseStartY = e.clientY;
  window.addEventListener("mousemove", drag);
  window.addEventListener("mouseup", stopDragging);
}
function stopDragging(){
  window.removeEventListener("mousemove", drag);
  window.removeEventListener("mouseup", stopDragging);
}
function drag(e){
  mouseCurrentX = e.clientX;
  mouseCurrentY = e.clientY;
  button.style.left = elementStartX + (mouseCurrentX - mouseStartX) + "px";
  button.style.top = elementStartY + (mouseCurrentY - mouseStartY) + "px";
}
```

Then, add in a tiny bit of CSS into your `<head>`...

```css
button{
  position:absolute;
  top:0px;
  left:0px;
}
```

### Turn and talk

* Why are the `mousemove` and `mouseup` event listeners attached to `window` instead of to `button`? (What happens if you change them to being on `button`?)
* What's the purpose of removing the event listeners?
* Why do we **have** to use named functions here, instead of anonymous functions?
* What information do the `offsetLeft`, `offsetTop`, `e.clientX`, and `e.clientY` properties give you?
* Open the "elements" part of the inspector -- the bit that shows all of the HTML. What's happening to the `<button>` element?
* What's the difference between `function startDragging(e)` and `var startDragging = function(e)`?

## Timing Functions (10 minutes / 2:40)

Let's look at timing functions -- that is, Javascript's way of making something happen every `x` seconds.

Replace the contents of your `script.js` with this:

```js
function sayHello(){
  console.log("Hi there!")
}
setInterval(sayHello, 1000);
```

### Turn and Talk

* What does the number in `setInterval` indicate?
* Replace `setInterval` with `setTimeout`. What's the difference?

We'll make it more interesting by having the timer start on a click event, and stop on another click event.

Put a "start" and a "stop" button in your HTML...

```html
<button id="start">Start</button>
<button id="stop">Stop</button>
```

Then, replace the contents of your `script.js` with this...

```js
var start = document.getElementById("start");
var stop = document.getElementById("stop");
var singAnnoyingSong = function(){
  console.log("I know a song that gets on everybody's nerves...")
}
var songTimer;
start.addEventListener("click", function(){
  songTimer = setInterval(singAnnoyingSong, 100);
});
stop.addEventListener("click", function(){
  clearInterval(songTimer);
});
```

### Turn and Talk

* What happens when you click the "start" button a bunch of times in a row?
  * Why?
  * How is this different from events?
  * When you do this, why doesn't the "stop" button seem to work?
* How is `clearInterval` different from `removeEventListener`?
* Give `singAnnoyingSong` an argument of `e`, like we did for the event listeners. What information does it contain?

## Asynchronicity (5/150)

Run the next bit of code and you can see asynchronous program execution.

```js
function anAsyncFunction(){
  console.log("hello")
  setTimeout(function(){
    console.log("this is happening in the middle")
  }, 5000)
  console.log("goodbye")
}

anAsyncFunction();
```

Wait, what? The goodbye came before the "this is happening in the middle"!

With everything else we've seen, Javascript executes one line of code, then when that line is done, executes the next line of code. This is called being **synchronous**.

However, some operations in Javascript are **asynchronous**, meaning Javascript goes on to the next line of code without waiting for the previous line to complete.

This is limited mostly to timing functions, and operations where Javascript is loading data from some other website.

### Why doesn't Javascript wait for these operations to complete before going to the next line of code?

Because otherwise the webpage would just "hang" until the operation completes. The browser can't do anything while Javascript is actively running. We've seen this when we've encountered infinite `while` loops. Asyncrhonicity is a way of preventing the computer from freezing.

This risk is greatest when Javascript is making requests to other webpages. There's no way of knowing how long the request will take to complete. It could be near-instant, but if the target server is having a bad day, it could take who-knows-how-long. You don't want the operability of your computer to be at the mercy of some random computer somewhere else.

In this small app we made, anything we want to be sure happens **after** those 5 seconds of commuting should go inside the callback of the `setTimeout`. This way, we can be certain that it will run only when the 5 seconds are up.

## You do: Cash Register Exercise

https://github.com/ga-dc/cash-register

Let’s handle the form submission together

```js
var form = document.querySelector("form")
var userInput = document.querySelector("#newEntry")
form.addEventListener("submit", function(event){
  event.preventDefault()
  console.log(userInput.value)
})
```

## Sample Quiz Questions

1. What is the difference between synchronous and asynchronous program execution?
2. Define a function that takes a function as an argument and invokes the argument when the function is called.
