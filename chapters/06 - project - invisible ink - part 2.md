
# Invisible Ink: a11y

Web pages are accessible to all users by default.

When we write CSS and JavaScript, we will often make them less accessible---particularly to people who use assistive technologies like screen readers or who navigate with a keyboard---by accident because we don't realize how our code impacts the way they navigate the web.

In this chapter, we're going to make some accessibility enhancements (a11y for short) to our script to make it accessible to everyone who visits our site.

## Scoping

This isn't really an a11y thing, but we want to scope our code with IIFE to make sure there are no conflicts with other scripts that will cause it to break.

While we're add it, let's turn on strict mode.

```javascript
;(function (window, document, undefined) {

	'use strict';

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

## classList Polyfill

We use `classList` a few times in our script, and that's only supported back to IE10. We can push support for our script back to IE9 by adding a lightweight polyfill.^[[https://github.com/eligrey/classList.js/](https://github.com/eligrey/classList.js/)]

You can drop this outside of your IIFE, because we want all scripts we write to take advantage of this, and it won't cause any conflicts.

## Feature Test

We want to make sure the browser supports modern JavaScript APIs before attempting to run our code. Let's include a feature test before running our event listener.

```javascript
;(function (window, document, undefined) {

	'use strict';

	// Feature test
	// We're checking for both querySelector and addEventListener, since we use both of those in our script
	// You only need to include modern APIs in your supports var, and only ones you actually use.
	var supports = 'querySelector' in document && 'addEventListener' in window;
	if ( !supports ) return;

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

We're checking for both `querySelector` and `addEventListener`, since we use both of those in our script. You only need to include ES5 APIs in your `supports` variable, and only ones you actually use.

## Don't hide content until JavaScript is loaded

You may have visitors who use older browsers that don't support ES5 (think corporate users stuck on IE8). You may also have some visitors who have modern browsers but spotty connections that fail to download your JavaScript file.

You could have a bug in another script that cause your script to break, too. JavaScript is incredibly fragile.

Whatever the reason, you don't want your visitors stuck without access to your content.

We should conditionally hide our content areas only after the script has loaded. To do that, we'll add a unique class to our `<html>` element when the script loads.

```javascript
;(function (window, document, undefined) {

	'use strict';

	// Feature test
	// We're checking for both querySelector and addEventListener, since we use both of those in our script
	// You only need to include modern APIs in your supports var, and only ones you actually use.
	var supports = 'querySelector' in document && 'addEventListener' in window;
	if ( !supports ) return;

	// Add a class when the script loads
	document.documentElement.classList.add( 'invisible-ink' );

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

## Don't hide content from screen readers

We're currently using `display: none` to hide our content areas. This makes them invisible to screen readers.

When the content area is opened again, it might not always be obvious to the visitor what just happened. You can, to an extend, get around this with aria roles.

But, we might be better served by treating the expand-and-collapse script like a visually enhanced anchor link content.

To do this, we'll only visually hide the content so that screen readers can still read it. To them, the link to expand the content will appear to be just another anchor link, while sighted users will see the content expand-and-collapse.

```css
.invisible-ink .hide-me {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	white-space: nowrap;
	width: 1px;
}

.hide-me.active {
	clip: auto;
	height: auto;
	margin: 0;
	overflow: visible;
	position: static;
	white-space: normal;
	width: auto;
}
```

This CSS was adapted from HTML5 Boilerplate.^[[https://html5boilerplate.com/](https://html5boilerplate.com/)]

## Bringing content into focus

Visually impaired users and people with neuromuscular conditions that make it difficult to use a mouse will often navigate your site by keyboard. One way to do this is by pressing the "tab" key to jump from link to link.

When you click on a normal anchor link, it shifts focus to that content area. Clicking tab will jump you to the next link inside that content area, rather than the next link after the one you just clicked.

Our script as it's currently written deviates from this behavior. We can fix this using the JavaScript `.focus()` API.

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

	// If content is active, set focus
	if ( content.classList.contains( 'active' ) ) {
		content.focus();
	}

}, false);
```

Here, we're checking to see if the content has the `.active` class on it, which would mean that it's open. If it is, then we'll shift focus to it.

Try adding a few links inside your hidden content areas, and then toggle them open and press "tab."

You've probably noticed that you're not tabbing into the link in your content are as you'd expect. That's because the browser doesn't recognize our content as a valid focusable element.

Focus is normally reserved for things like links and form inputs. However, we can tell the browser to make content focusable by adding a `tabindex` attribute. Positive `tabindex` integers are reserved for elements you want to tab through, but setting it to `-1` let's you focus programatically with JavaScript while keep it out of the normal tab flow.

```html
<div class="hide-me" id="hide-me" tabindex="-1">
	<p>1. Here's some content that I'd like to show or hide when the link is clicked.</p>
</div>
```

You *could* add a `tabindex` to all of your content areas, but it would be great if we could let JavaScript handle it for us. And we can!

```javascript
if ( content.classList.contains( 'active' ) ) {
	content.focus();
	if ( document.activeElement.id !== content.id ) {
		content.setAttribute( 'tabindex', '-1' );
		content.focus();
	}
}
```

`document.activeElement` gets the element that's currently in focus. We want to compare the ID of that element to the ID of our content area. If they're the same, our content area is in focus. If not, we can use `setAttribute` to add a `tabindex` and then try again.

### Removing the outline

You may notice that now your content area has a blue outline when it's opened.

Browsers add this to element currently in focus so that sighted keyboard users know where they are on the page. It's an important accessibility feature that you generally shouldn't remove.

**But...** since the focused element isn't one they can interact with, it's ok to remove the outline in this case. We'll add `outline: 0` to our CSS file.

```css
.invisible-ink .hide-me {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	outline: 0;
	padding: 0;
	position: absolute;
	white-space: nowrap;
	width: 1px;
}
```

## What about sighted keyboard users?

Now, keyboard users can tab through all of your links. Even if they're using a screen reader, they can still read all of the content.

But... not all keyboard users are visually impaired. Some simply struggle to use a mouse or finding navigating with a keyboard easier. Right now, they can tab into links in one of our hidden content areas, but can't actually see it.

We want to open a collapsed content area if the user tabs into a link inside it. We can do that by adding an event listener for `focus` events.

```javascript
// Open content area if link within is brought into focus
document.addEventListener('focus', function (event) {
	// Open the closed content area
}, true);
```

With this event listener, we need to see if the focused element is inside one of our content areas. To do that, we're going to add the `getClosest()` function from the *Ditching jQuery Cheat Sheet*. This climbs up the DOM tree looking for a parent item that has a particular class, data attribute, or other selector.

```javascript
// Add the full method, obviously
var getClosest = function ( elem, selector ) {...}

// Open content area if link within is brought into focus
document.addEventListener('focus', function (event) {

	// Check to see if the focused element is inside a .hide-me element
	var content = getClosest( event.target, '.hide-me' );

	// If it's not, bail
	if ( !content ) return;

	// Otherwise, open it
	content.classList.add( 'active' );

}, true);
```

First, we check to see if the focused element has a parent with the `.hide-me` class. If not, we bail on our function. If it does, we use the `.add()` API in `classList` to add our `.active` class and open the content area.

Now, our script is a lot more accessible.