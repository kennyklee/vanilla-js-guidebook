
# Scoping

In the previous chapter, I mentioned that when you load your JavaScript in the footer (as you should for performance reasons) the jQuery and vanilla JS `ready()` methods aren't neccessary.

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