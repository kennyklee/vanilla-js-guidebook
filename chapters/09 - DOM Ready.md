
# DOM Ready

Wait until the DOM is ready before running code.

**Note:** *If you're loading your scripts in the footer (which you should be for performance reasons), the `ready()` method isn't really needed. It's just a habit from the "load everything in the header" days.*


Vanilla JavaScript provides a native way to do this: the `DOMContentLoaded` event for `addEventListener`.

**But...** if the DOM is already loaded by the time you call your event listener, the event never happens and your function never runs.

Below is a super lightweight helper method that does the same thing as jQuery's `ready()` method. This helper method does two things:

1. Check to see if the document is already `interactive` or `complete`. If so, it runs your function immediately.
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

## Browser Compatibility

Works in all modern browsers, and IE9 and above.