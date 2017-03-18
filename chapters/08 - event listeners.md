
# Event Listeners

## jQuery

jQuery uses the `on()` method to listen for events, and also supplies some event specific methods (like `.click()`).

```javascript
var $elem = $( '#click-me' );

$elem.on( 'click', function( event ) {
	console.log( 'clicked' );
});

$elem.click(function( event ) {
	console.log( 'also clicked' );
});
```


## addEventListner

Listen for events on an element. You can find a full list of available events on the Mozilla Developer Network. ^[[https://developer.mozilla.org/en-US/docs/Web/Events](https://developer.mozilla.org/en-US/docs/Web/Events)]

```javascript
var btn = document.querySelector( '#click-me' );
btn.addEventListener('click', function ( event ) {
	console.log( event ); // The event
	console.log( event.target ); // The clicked element
}, false);
```


## Multiple Targets

jQuery's `.on()` method automatically listens for clicks on any element that matches your selector. The vanilla JavaScript `addEventListener()` API requires you to specific specific, individual elements to listen to.

Fortunately, there's a *really* easy way to get a jQuery-like experience: event bubbling.

Instead of listening to specific elements, we'll instead listen for *all* clicks on a page, and then check to see if the clicked item has a matching selector.

```javascript
// Listen for clicks on the entire window
window.addEventListener('click', function ( event ) {

	// If the clicked element has the `.click-me` class, it's a match!
	if ( event.target.classList.contains( 'click-me' ) ) {
		// Do something...
	}

}, false);
```


## Multiple Events

jQuery's `.on()` method also allows you to pass in multiple events to listen to. With vanilla JavaScript, you need to add an event listener for each event individually.

**But...** by using a named function and passing that into your event listener, you can avoid having to write the same code over and over again.

```javascript
// Setup our function to run on various events
var someFunction = function ( event ) {
	// Do something...
};

// Add our event listeners
window.addEventListener( 'click', someFunction, false );
window.addEventListener( 'scroll', someFunction, false );
```


## Event Debouncing

Events like `scroll` and `resize` can cause huge performance issues on certain browsers. Paul Irish explains:^[[https://www.paulirish.com/2009/throttled-smartresize-jquery-event-handler/](https://www.paulirish.com/2009/throttled-smartresize-jquery-event-handler/)]

> If you’ve ever attached an event handler to the window’s resize event, you have probably noticed that while Firefox fires the event slow and sensibly, IE and Webkit go totally spastic.

Debouncing is a way of forcing an event listener to wait a certain period of time before firing again. To use this approach, we'll setup a `timeout` element. This is used as a counter to tell us how long it's been since the event was last run.

When our event fires, if `timeout` has no value, we'll assign a `setTimeout` function that expires after 66ms and contains our the methods we want to run on the event.

If it's been less than 66ms from when the last event ran, nothing else will happen.

```javascript
// Setup a timer
var timeout;

// Listen for resize events
window.addEventListener('resize', function ( event ) {
	console.log( 'no debounce' );

	// If timer is null, reset it to 66ms and run your functions.
	// Otherwise, wait until timer is cleared
	if ( !timeout ) {
		timeout = setTimeout(function() {

			// Reset timeout
			timeout = null;

			// Run our resize functions
			console.log( 'debounced' );

		}, 66);
	}
}, false);
```


## Use Capture

The last argument in `addEventListener()` is `useCapture`, and it specifies whether or not you want to "capture" the event. For most even types, this should be set to `false`. But certain events, like `focus`, don't bubble.

Setting `useCapture` to `true` allows you to take advantage of event bubbling for events that otherwise don't support it.

```javascript
// Listen for all focus events in the document
document.addEventListener('focus', function (event) {
	// Run functions whenever an element in the document comes into focus
}, false);
```