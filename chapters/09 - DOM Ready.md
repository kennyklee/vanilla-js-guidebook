
# DOM Ready

**Note:** If you're loading your scripts in the footer (which you should be for performance reasons), the `ready()` method isn't really needed. It's just a habit from the "load everything in the header" days.

## jQuery

jQuery uses the `.ready()` method to wait until the DOM is ready before running code.

```javascript
$( document ).ready(function() {
    // Do stuff...
});
```


## Vanilla JavaScript

Vanilla JavaScript does provide a native API to handle this: the `DOMContentLoaded` event for `addEventListener`.

The challenge is that if the DOM is already loaded by the time you call your event listener, the event never happens and your function never runs.

Here's a super lightweight helper method that does the same thing as jQuery's `ready()` method. This helper method is doing two things:

1. Checking to see if the document is already `interactive` or `complete`. If so, it runs your function immediately.
2. Otherwise, it adds a listener for the `DOMContentLoaded` event.

```javascript
/**
 * Run event after DOM is ready
  * @param  {Function} fn Callback function
 */
var ready = function ( fn ) {

	// Sanity check
	if ( typeof fn !== 'function' ) return;

	// If document is already loaded, run method
	if ( document.readyState === 'interactive' || document.readyState === 'complete' ) {
		return fn();
	}

	// Otherwise, wait until document is loaded
	document.addEventListener( 'DOMContentLoaded', fn, false );

};

// Example
ready(function() {
	// Do stuff...
});
```

If you test these, you'll see that the vanilla JavaScript version actually runs ealier than the jQuery one, even if you put the vanilla JS one second in your code.