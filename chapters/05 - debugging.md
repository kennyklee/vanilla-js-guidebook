
# Debugging

Your code will break. A lot. This is an inevitable aspect of being a web developer.

Let's talk about how to figure out what's going on and get it working again.

## Strict mode

Strict mode is a way of telling browsers (and JavaScript debuggers) to be, well, stricter about how they parse your code. MDN explains:^[[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)]

> Strict mode makes several changes to normal JavaScript semantics. First, strict mode eliminates some JavaScript silent errors by changing them to throw errors.

This is highly desirable. I know that sounds counterintuitive. Why would you want more errors in your code?

Here's the thing: there were already errors in your code. The browser just wasn't telling you about them, so they might crop up in unexpected ways that are harder to find.

Turning on strict mode helps you find errors sooner, before they become bigger problems. And it forces you to write better code.

Always use strict mode on your scripts.

### How do you activate strict mode?

Simple, just add this to your scripts:

```javascript
'use strict';
```

## Developer tools and the console

If you're not already familiar with your browser's developer tools, and in particular, the console tab, you should play around with them a bit.

The console tab is where JavaScript errors will get displayed. The error will typically specify what the error is, what file it's occuring, and what line number it's on. It will also let you click that line number to jump directly to the error in the file.

You can also write JavaScript directly in the console window, which is useful for quickly testing out a few lines of code.

All modern browsers have developer tools baked in, but I think Google Chrome^[[https://www.google.com/chrome/](https://www.google.com/chrome/)] and Mozilla Firefox Developer Edition^[[https://www.mozilla.org/en-US/firefox/developer/](https://www.mozilla.org/en-US/firefox/developer/)] provide the best ones.

They differ in subtle ways, so it's really a matter of preference which one you choose.

### console.log()

The `console.log()` JavaScript method provides you with a way to output values from your scripts in the console window. It's incredibly useful when trying to figure out why something isn't working.

For example:

```javascript
var someElem = document.querySelector( '#some-element' );
console.log(someElem);
```

That would output the HTML for the element that has the ID `#some-element`, or, if no matching element is found, `null`.

As you'll see in the screencasts that come with this book, I use `console.log()` quite a bit to figure out why something isn't working.

## The debugging process

There's no magic process for debugging, unfortunately.

I typically start at the last working piece of code, using `console.log()` to check what's happening until I find the piece of code I messed up or that's returning an unexpected result.