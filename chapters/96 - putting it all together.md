
# Putting it all together

Let's put everything we've learned together with a small, working script.

When someone clicks a link with the `.click-me` class, we want to show the content in the element whose ID matches the anchor link.

```html
<p>
	<a class="click-me" href="#hide-me">
		Click Me
	</a>
</p>

<div class="hide-me" id="hide-me">
	<p>Here's some content that I'd like to show or hide when the link is clicked.</p>
</div>
```

## IIFE and Strict Mode

Let's start by creating an IIFE to keep our code out of the global scope. Let's also turn on strict mode so that we can aggressively debug.

```javascript
;(function (window, document, undefined) {

	'use strict';

	// Code goes here...

})(window, document);
```


## Core Functionality

We can use `addEventListener` to detect clicks on the `.click-me` button.

```javascript
var link = document.querySelector( '.click-me' );
link.addEventListener('click', function (event) {
	// Do stuff when the link is clicked
}, false);
```

We can prevent the default click behavior with `event.preventDefault()`. This stops the link from changing the page URL.

```javascript
var link = document.querySelector( '.click-me' );
link.addEventListener('click', function (event) {

	// Prevent the default click behavior
	event.preventDefault();

}, false);
```

To show and hide the content, we *could* toggle `display: none` using the `style` attribute, but I think using CSS in an external stylesheet is easier to maintain and gives you more flexibility. We'll add a style that hides our content areas by default, and shows them a second class is present.

```css
.hide-me {
	display: none;
}

.hide-me.active {
	display: block;
}
```

That `.active` class can be anything you want. Some people prefer more action or state-oriented class names like `.is-open`. Do whatever fits your approach to CSS.

The nice thing with using a class to control visibility is that if you want a content area to be visible by default, you can just add the `.active` class to it as the starting state in your markup.

```html
<div class="hide-me active" id="hide-me">
	<p>Here's some content that I'd like open by default, and hidden when the link is clicked.</p>
</div>
```

Next, we'll toggle that class with JavaScript when the link is clicked using `classList`. The `classList.toggle()` method adds the class if it's missing, and removes it if it's present (just like jQuery's `.toggleClass()` method).

We'll use `event.target.hash` to get the hash from the clicked link, and `querySelector` to get the content it points to.

```javascript
var link = document.querySelector( '.click-me' );
link.addEventListener('click', function (event) {

	// Prevent the default click behavior
	event.preventDefault();

	// Get the target content
	var content = document.querySelector( event.target.hash );

	// If the content area doesn't exist, bail
	if ( !content ) return;

	// Show or hide the content
	content.classList.toggle( 'active' );

}, false);
```

A few notes about the script above. First, we're using `.hash` instead of `.href`. Even with a relative URL like `#some-id`, `.href` will return the full URL for the page.

If an element is not found with `querySelector`, our script will throw an error if we try to run any methods against the `null` element. As a result, we want to make sure the element exists before doing anything with it.


### What if there's more than one content area?

The code we've got so far is great when there's just a single expand-and-collapse area, but what if you multiple ones, like this?

```html
<p>
	<a class="click-me" id="click-me-1" href="#hide-me-1">
		Click Me
	</a>
</p>
<div class="hide-me" id="hide-me-1">
	<p>Here's some content that I'd like to show or hide when the link is clicked.</p>
</div>

<p>
	<a class="click-me" id="click-me-2" href="#hide-me-2">
		Click Me, Too
	</a>
</p>
<div class="hide-me" id="hide-me-2">
	<p>Here's some more content that I'd like to show or hide when the link is clicked.</p>
</div>
```

You *could* write add an event listener for every content area, but that's bloated and annoying to maintain.

```javascript
/**
 * DON'T DO THIS!
 */
var link = document.querySelector( '#click-me-1' );
var link2 = document.querySelector( '#click-me-2' );

link.addEventListener('click', function (event) {

	// Prevent the default click behavior
	event.preventDefault();

	// Show or hide the content
	var content = document.querySelector( event.target.hash );
	if ( !content ) return;
	content.classList.toggle( 'active' );

}, false);

link2.addEventListener('click', function (event) {

	// Prevent the default click behavior
	event.preventDefault();

	// Show or hide the content
	var content = document.querySelector( event.target.hash );
	if ( !content ) return;
	content.classList.toggle( 'active' );

}, false);
```

Seriously, don't ever do that. Instead, let's take advantage of event bubbling.

### Event Bubbling

Instead of identifying the individual elements to listen to, you can listen to all events within a parent element, and check to see if they were made on the elements you care about.

In our case, let's watch all clicks on the `document`, and then check to see if the clicked element has the `.click-me` class using the `.contains` method for `classList`. If it does, we'll run our script. Otherwise, we'll bail.

```javascript
document.addEventListener('click', function (event) {

	// Make sure a .click-me link was clicked
	if ( !event.target.classList.contains( 'click-me' ) ) return;

	// Prevent default
	event.preventDefault();

	// Show or hide the content
	var content = document.querySelector( event.target.hash );
	if ( !content ) return;
	content.classList.toggle( 'active' );

}, false);
```

Congratulations! You now have a working expand-and-collapse script in vanilla JavaScript. And, it's more-or-less identical in size to the jQuery version.


## Cutting the Mustard

We want to make sure the browser supports our JavaScript functions and browser APIs before attempting to run our code. Let's include a feature test before running our event listener.

We're checking for `querySelector`, `addEventListener`, and `classList`. I'm comfortable with IE10+ support for this, so I've decided to skip the polyfill for this project.

```javascript
;(function (window, document, undefined) {

	'use strict';

	// Feature test
	var supports = 'querySelector' in document && 'addEventListener' in window && 'classList' in document.createElement('_');
	if ( !supports ) return;

	// Listen for click events
	document.addEventListener('click', function (event) {

		// Make sure a .click-me link was clicked
		if ( !event.target.classList.contains( 'click-me' ) ) return;

		// Prevent default
		event.preventDefault();

		// Show or hide the content
		var content = document.querySelector( event.target.hash );
		if ( !content ) return;
		content.classList.toggle( 'active' );

	}, false);

})(window, document);
```


## Progressively Enhance

You may have visitors who use older browsers that don't support ES5 (like corporate users stuck on IE8). You may also have some visitors who have modern browsers but spotty connections that fail to download your JavaScript file (like a commuter on a train headed into a tunnel).

You could have a bug in another script that cause your script to break, too. JavaScript is incredibly fragile.

Whatever the reason, you don't want your visitors stuck without access to your content.

We should conditionally hide our content areas only after the script has loaded. To do that, we'll add a unique class to our `<html>` element when the script loads.

```javascript
;(function (window, document, undefined) {

	'use strict';

	// Feature test
	var supports = 'querySelector' in document && 'addEventListener' in window && 'classList' in document.createElement('_');
	if ( !supports ) return;

	// Add a class when the script loads
	document.documentElement.classList.add( 'invisible-ink' );

	// Listen for click events
	document.addEventListener('click', function (event) {

		// Make sure a .click-me link was clicked
		if ( !event.target.classList.contains( 'click-me' ) ) return;

		// Prevent default
		event.preventDefault();

		// Show or hide the content
		var content = document.querySelector( event.target.hash );
		if ( !content ) return;
		content.classList.toggle( 'active' );

	}, false);

})(window, document);
```

Then we'll hook into that class in our CSS file to conditionally hide content.

```css
.invisible-ink .hide-me {
	display: none;
}

.hide-me.active {
	display: block;
}
```

And now we have a lightweight, vanilla JavaScript expand-and-collapse widget that supports any browser that can access the web.