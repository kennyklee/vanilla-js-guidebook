
# Ajax/HTTP Requests

## Standard Ajax/HTTP Requests

Ajax requests have really good browser support, but require a few steps and can be a bit tedious to write. Later in this lesson, we'll look at a lightweight, super helpful micro-library. But first, let's look at how to make Ajax requests without any help.

1. We setup our request with `XMLHttpRequest()`.
2. We create an `onreadystatechange` callback to run when the request completes.
3. We open and send our request.

For testing purposes, we'll pull data from JSON Placeholder.^[[https://jsonplaceholder.typicode.com/](https://jsonplaceholder.typicode.com/)]

```javascript
// Set up our HTTP request
var xhr = new XMLHttpRequest();

// Setup our listener to process compeleted requests
xhr.onreadystatechange = function () {
	// Only run if the request is complete
	if ( xhr.readyState !== 4 ) return;

	// Process our return data
	if ( xhr.status === 200 ) {
		// What do when the request is successful
		console.log( xhr );
	} else {
		// What do when the request fails
		console.log('The request failed!');
	}

	// Code that should run regardless of the request status
	console.log('This always runs...');
};

// Create and send a GET request
// The first argument is the post type
// The second argument is the endpoint URL
xhr.open( 'GET', 'https://jsonplaceholder.typicode.com/posts' );
xhr.send();
```

### Browser Compatibility

Works in all modern browsers, and IE7 and above.


## JSONP

For security reasons, you cannot load JSON files that reside on a different server. JSONP is a way to get around this issue. Sitepoint explains:^[[https://www.sitepoint.com/jsonp-examples/](https://www.sitepoint.com/jsonp-examples/)]

> JSONP (which stands for JSON with Padding) builds on this technique and provides us with a way to access the returned data. It does this by having the server return JSON data wrapped in a function call (the “padding”) which can then be interpreted by the browser.

With JSONP, you need to use a global callback function to actually do something with the data you get. When you request your JSON data, you pass in that callback as an argument on the URL. When the data loads, it runs the callback function.

`getJSONP` is a helper function I wrote to handle JSONP requests. Pass in your URL endpoint as the first argument, and your callback function (as a string) as the second argument.

For testing purposes, we're using JSFiddle's Echo service^[[http://doc.jsfiddle.net/use/echo.html](http://doc.jsfiddle.net/use/echo.html)] in the example below.

```javascript
/**
 * Get JSONP data
 * @param  {String}   url      The JSON URL
 * @param  {Function} callback The function to run after JSONP data loaded
 */
var getJSONP = function ( url, callback ) {

	// Create script with url and callback (if specified)
	var ref = window.document.getElementsByTagName( 'script' )[ 0 ];
	var script = window.document.createElement( 'script' );
	script.src = url + (url.indexOf( '?' ) + 1 ? '&' : '?') + 'callback=' + callback;

	// Insert script tag into the DOM (append to <head>)
	ref.parentNode.insertBefore( script, ref );

	// After the script is loaded (and executed), remove it
	script.onload = function () {
		this.remove();
	};

};

// Example
var logAPI = function ( data ) {
	console.log( data );
}

getJSONP( 'http://jsfiddle.net/echo/jsonp/?text=something&par1=another&par2=one-more', 'logAPI' );
```

### Browser Compatibility

Works in all modern browsers, and IE6 and above.


## Atomic

Atomic^[[https://github.com/toddmotto/atomic](https://github.com/toddmotto/atomic)] is an insanely useful library from Todd Motto. It weighs just 1.5kb minified, and makes working with Ajax/HTTP and JSONP absurdly easy.

```javascript
// After you've included Atomic with your scripts...

// GET
atomic.get('https://jsonplaceholder.typicode.com/posts')
	.success(function (data, xhr) {
		// What do when the request is successful
		console.log(data);
		console.log(xhr);
	})
	.error(function () {
		// What do when the request fails
		console.log( 'The request failed!' );
	})
	.always(function (data, xhr) {
		// Code that should run regardless of the request status
	});


// POST
atomic.post('http://jsonplaceholder.typicode.com/posts', {
	title: 'foo',
	body: 'bar',
	userId: 1
})
	.success(function (data, xhr) {
		// What do when the request is successful
		console.log(data);
		console.log(xhr);
	})
	.error(function () {
		// What do when the request fails
		console.log( 'The request failed!' );
	})
	.always(function (data, xhr) {
		// Code that should run regardless of the request status
	});


// JSONP

// Callback method
var logAPI = function ( data ) {
	console.log( data );
}

// Get JSONP
atomic.jsonp('http://jsfiddle.net/echo/jsonp/', 'logAPI', {
	text: 'something',
	par1: 'another',
	par2: 'one-more',
	bool: true
});
```

### Browser Compatibility

Works in all modern browsers, and IE8 and above.
