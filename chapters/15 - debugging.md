
# Debugging

Your code will break. A lot. This is an inevitable aspect of being a web developer.

Let's talk about how to figure out what's going on and get it working again.

## Strict mode

Strict mode is a way of telling browsers (and JavaScript debuggers) to be, well, stricter about how they parse your code. MDN explains:

> Strict mode makes several changes to normal JavaScript semantics. First, strict mode eliminates some JavaScript silent errors by changing them to throw errors.
>
> **Source:** [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

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

All modern browsers have developer tools baked in. They differ in subtle ways, so it's really a matter of preference which one you choose.


## The debugging process

There's no magic process for debugging, unfortunately.

I typically start at the last working piece of code, using `console.log()` to check what's happening until I find the piece of code I messed up or that's returning an unexpected result.


## Scoping

In the previous lesson, I mentioned that when you load your JavaScript in the footer (as you should for performance reasons) the jQuery and vanilla JS `ready()` methods aren't neccessary.

They do actually serve a useful purpose, though: scoping.

They act as a wrapper around your code, and as a result keep your variables and functions out of the global scope, where they're more likely to conflict with other scripts.

Since you typically won't need to use the  `ready()` method, you should wrap your scripts in what's called an IIFE, an Immediately Invoked Function Expression. An IIFE is simply an anonymous (unnamed) function that runs immediately.

```javascript
;(function (window, document, undefined) {
    // Do stuff...
})(window, document);
```

Because your code is wrapped in a function, all of the variables and functions inside it are local to the IIFE. They won't override variables and functions set by other scripts, nor can they be accessed by other scripts.

And if you set a variable with the same name as one in the global scope the local one takes precendence.

There are times you may want to expose a function or variable to the global scope (for example, a lightweight framework you want other scripts to be able to use), and there are other ways to keep your code out of the global scope.

But generally speaking, for the types of scripts we will be covering in this book, you should always wrap your code in an IIFE.

***Caveat:*** *If you use `ready()`, you don't need to use an IIFE. Your scripts are already scoped.*