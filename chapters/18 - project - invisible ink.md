
# Project: Invisible Ink

Hopefully you're starting to feel a bit more comfortable working with vanilla JavaScript. Let's work on another project together.

Invisible Ink is an expand-and-collapse accordion script.


## The Starting HTML

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


## Planning

For this script...

1. When someone clicks a `.click-me` link, prevent the default link action from happening.
2. Get the content area who's ID matches the anchor link target.
3. If the content area is visible, hide it. If it's hidden, show it.


## IIFE and Strict Mode

Just like last time, let's add an IIFE to keep our code out of the global scope. Let's also turn on strict mode so that we can aggressively debug.

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

Just like in jQuery, we can prevent the default click behavior with `event.preventDefault()`. This stops the link from changing the page URL.

```javascript
var link = document.querySelector( '.click-me' );
link.addEventListener('click', function (event) {

	// Prevent the default click behavior
	event.preventDefault();

}, false);
```

In jQuery, you might use `toggle()` to toggle the content visibility. This method adds or removes `display: none` as an inline style on the element.

In vanilla JS, you *could* toggle `display: none` using the `style` attribute, but I think it's easier to maintain and gives you more flexibility to add these to an external stylesheet. We'll add a style that hides our content areas by default, and shows them a second class is present.

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
	<p>Here's some content that I'd like to show or hide when the link is clicked.</p>
</div>
```

Next, we'll toggle that class with JavaScript when the link is clicked using `classList`. The `.toggle()` method adds the class if it's missing, and removes it if it's present, just like jQuery's `.toggleClass()` method.

We use `event.target.hash` to get the hash from the clicked link.

```javascript
var link = document.querySelector( '.click-me' );
link.addEventListener('click', function (event) {

	// Prevent the default click behavior
	event.preventDefault();

	// Get the target content
	// We use .hash instead of .href because .href returns the full URL, even for relative links
	var content = document.querySelector( event.target.hash );

	// If the content area doesn't exist, bail
	if ( !content ) return;

	// Show or hide the content
	content.classList.toggle( 'active' );

}, false);
```

A few notes about the script above. First, we're using `.hash` instead of `.href`. Even with a relative URL like `#some-id`, `.href` will return the full URL for the page.

In jQuery, if an element isn't found, jQuery silently ignores it. `querySelector` does not, and we'll get an error if we try to run any methods against a `null` element. As a result, we want to make sure the element exists before doing anything with it.


### What if there's more than one content area?

The code we've got so far is great when there's just a single expand-and-collapse area, but what if you multiple ones, like this?

```html
<p>
	<a class="click-me" id="click-me-1" href="#hide-me">
		Click Me
	</a>
</p>
<div class="hide-me" id="hide-me">
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

Seriously, don't ever, ever do that.

Instead, let's take advantage of event bubbling.

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

Congratulations! You now have a working expand-and-collapse script in vanilla JavaScript. And, it's basically identical in size to the jQuery version.


## Cutting the Mustard

We want to make sure the browser supports modern JavaScript APIs before attempting to run our code. Let's include a feature test before running our event listener.

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

You may have visitors who use older browsers that don't support ES5 (think corporate users stuck on IE8). You may also have some visitors who have modern browsers but spotty connections that fail to download your JavaScript file.

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


## Screen Readers

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

This CSS was adapted from HTML5 Boilerplate. ([https://html5boilerplate.com/](https://html5boilerplate.com/))


## Set Focus

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

With this event listener, we need to see if the focused element is inside one of our content areas. To do that, we're going to add the `getClosest()` helper method.

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

Now, our script is a lot more accessible to keyboard users.


## Accordion Functionality

Right now, our script works great for things like "read more" links.

Now, let's add the ability to use it like a real accordion, where if you open a content area, the others in the group close.

To do this we're going to add a parent element around the content areas we want to function as an accordion. Let's give it a class of `.hide-me-group`.

```html
<div class="hide-me-group">

	<p>
		<a class="click-me" href="#hide-me">
			Click Me
		</a>
	</p>

	<div class="hide-me" id="hide-me">
		<p>Here's some content that I'd like to show or hide when the link is clicked. Let's also put <a href="#">a link</a> in here.</p>
	</div>

	<p>
		<a class="click-me" href="#hide-me-2">
			Click Me, Too
		</a>
	</p>

	<div class="hide-me" id="hide-me-2">
		<p>Here's some more content that I'd like to show or hide when the link is clicked.</p>
	</div>

	<p>
		<a class="click-me" href="#hide-me-3">
			Click Me, Three
		</a>
	</p>

	<div class="hide-me" id="hide-me-3">
		<p>Here's even more content that I'd like to show or hide when the link is clicked.</p>
	</div>


</div>
```

### Closing all of the content areas in a group

When a content area is opened, we want to see if it's part of a group. If it is, we can get all of the other content areas in the group and close them. If not, we don't have to do anything.

To do this, we'll again use our `getClosest()` method to see if a parent element has the `.hide-me-group` class.

```javascript
// Show/hide content when toggle is clicked
document.addEventListener('click', function (event) {

	// Make sure a .click-me link was clicked
	if ( !event.target.classList.contains( 'click-me' ) ) return;

	// Prevent default
	event.preventDefault();

	// ...

	// If accordion, close other content area
	var group = getClosest( content, '.hide-me-group' );
	if ( group ) {
		// Close other content areas
	}

}, false);
```

If it does, we want to get all of the content areas inside of this group only. We don't want to close ungrouped content areas or ones inside another accordion. We do that using .

While you normally use `.querySelectorAll()` on the `document`, you can use it to search inside any element. In our case, we'll search just inside our content group.

```javascript
// If accordion, close other content area
var group = getClosest( content, '.hide-me-group' );
if ( group ) {

	// Get all content areas inside the group
	var groupContents = group.querySelectorAll( '.hide-me' );

}
```

Next, we want to loop through them and close them, with one exception: we want to leave open the content area that's toggled.

To do this, we'll check to see if the content area in the loop has the same ID as the toggled content area. If so, we'll skip to the next item in the loop with `continue`. Otherwise, we'll close it.

```javascript
// If accordion, close other content area
var group = getClosest( content, '.hide-me-group' );
if ( group ) {

	// Get all content areas inside the group
	var groupContents = group.querySelectorAll( '.hide-me' );

	// Close all content areas except the one that was toggled
	for ( var i = 0; i < groupContents.length; i++ ) {
		if ( groupContents[i].id === content.id ) continue;
		groupContents[i].classList.remove( 'active' );
	}

}
```


## Focus Links with the Accordion

Earlier, we added the ability to open a content area if we tabbed onto a link inside of it.

If the link is inside an accordion, we should close the other content areas in the group. We'd do this using the same code as above.

Rather than have the same code twice in our script, we should add this task to a function that we can call in multiple places as needed. We'll pass in the `content` variable as an argument.

```javascript
// If accordion, close other content area
var closeContentGroups = function (content) {

	var group = getClosest( content, '.hide-me-group' );
	if ( group ) {

		// Get all content areas inside the group
		var groupContents = group.querySelectorAll( '.hide-me' );

		// Close all content areas except the one that was toggled
		for ( var i = 0; i < groupContents.length; i++ ) {
			if ( groupContents[i].id === content.id ) continue;
			groupContents[i].classList.remove( 'active' );
		}
	}

};
```

Now we can call this method within both of our event listeners, keeping our codebase more DRY (an acryonym for Don't Repeat Yourself).

```javascript
// Show/hide content when toggle is clicked
document.addEventListener('click', function (event) {

	// Make sure a .click-me link was clicked
	if ( !event.target.classList.contains( 'click-me' ) ) return;

	// Prevent default
	event.preventDefault();

	// ...

	// If accordion, close other content area
	closeContentGroups(content);

}, false);

// Open content area if link within is brought into focus
document.addEventListener('focus', function (event) {
	var content = getClosest( event.target, '.hide-me' );
	if ( !content ) return;
	content.classList.add( 'active' );

	// If content is part of a group, close all other content areas
	closeContentGroups(content);
}, true);
```

Now our script has accordion functionality, but also supports standalone expand-and-collapse content.


## Icons and Labels

There's a good chance you'd like the text on the element that toggles your content to change based on whether or not the content invisible.

For example, "Show more" when it's closed, and "Show less" when it's open. Or different icons for expand and collapse, like plus/minus signs or up/down carets.

### The approach

A lot of times scripts will inject this content with JavScript. I think that's a lot more restrictive and complicated to implement and maintain.

Instead, we want to add an active state to the toggle link, not just the content, on click.

```javascript
// Show/hide content when toggle is clicked
document.addEventListener('click', function (event) {

	// Make sure a .click-me link was clicked
	if ( !event.target.classList.contains( 'click-me' ) ) return;

	// Prevent default
	event.preventDefault();

	// Show or hide the content
	var content = document.querySelector( toggle.hash );
	if ( !content ) return;
	content.classList.toggle( 'active' );
	toggle.classList.toggle( 'active' );

	// ...

}, false);
```

Then we can add some additional markup with our labels or icons inside our content toggle links.

```html
<a class="click-me" href="#hide-me">
	Click me
	<span class="click-me-closed">+</span>
	<span class="click-me-open">-</span>
</a>

...

<a class="click-me" href="#hide-me-2">
	<span class="click-me-closed">Show More</span>
	<span class="click-me-open">Show Less</span>
</a>
```

Then we can some conditional CSS that we would use to show or hide our labels or icons inside the link.

```css
.click-me-closed,
.click-me-open,
.invisible-ink .active .click-me-closed {
	display: none;
}

.invisible-ink .click-me-closed,
.invisible-ink .active .click-me-open {
	display: inline-block;
}
```

By default, both sets of icons are hidden. When the script loads, the `.click-me-closed` icons become visible. And when the content area is opened, the `.click-me-closed` icons become hidden again while the `.click-me-open` icons are revealed.

In this case, it's 100% appropriate to use `display: none` because we don't want screen readers to see this text if the script isn't active.

### Uh oh... clicking the icon doesn't toggle the content

You may notice that clicking the icons (or conditional labels) does not cause the content to toggle open or closed. This is because of how event bubbling works in our event listener.

When you click on one of the `<span>` element inside your link, `event.target` returns that instead of the link itself. As a result, when check for the `.click-me` class, it's not found.

We can again use `getClosest()` method to see if the clicked item is within (or includes) our `.click-me` link.

```javascript
// Show/hide content when toggle is clicked
document.addEventListener('click', function (event) {

	// Get the clicked element
	var toggle = getClosest( event.target, '.click-me' );

	// Make sure a .click-me link was clicked
	if ( !toggle || !toggle.classList.contains( 'click-me' ) ) return;

	// ...

}, false);
```


## Icons on Focus

When someone tabs into a link in one of our closed content areas and it opens, we want to also activate the toggle link for that content.

To do that, we'll search for a link whose hash matches the content ID. Since there could be an anchor links to that element that's not intended to be a content toggle, we'll also narrow our search to links with the `.click-me` class.

If a link exists, we'll add the `.active` class.

```javascript
// Get the content area's toggle
var toggle = document.querySelector( 'a[href*="#' + content.id + '"].click-me' );
if ( toggle ) {
	toggle.classList.add( 'active' );
}
```

### Deactivating other toggles in accordions

Just as we want to close other content areas inside an accordion group, we also want to deactivate the toggle links that activate them. This works *almost* exactly the same way.

The one difference: we want to compare the link hash instead of the ID.

```javascript
// If the toggle is part of a group, deactive other toggles
var group = getClosest( toggle, '.hide-me-group' );
if ( group ) {

	// Get all toggle links inside the group
	var groupToggles = group.querySelectorAll( '.click-me' );

	// Close all toggle links except the one that was clicked
	for ( var i = 0; i < groupToggles.length; i++ ) {
		if ( groupToggles[i].hash === toggle.hash ) continue;
		groupToggles[i].classList.remove( 'active' );
	}
}
```

And just like before, we also want to deactivate all other toggle links when tabbing into and opening a closed content area. Since we would also use this same exact bit of code to do it, let's also move this task to a function.

```javascript
// If the toggle is part of a group, deactive other toggles
var closeConteGroups = function (toggle) {

	var group = getClosest( toggle, '.hide-me-group' );

	if ( group ) {

		// Get all toggle links inside the group
		var groupToggles = group.querySelectorAll( '.click-me' );

		// Close all toggle links except the one that was clicked
		for ( var i = 0; i < groupToggles.length; i++ ) {
			if ( groupToggles[i].hash === toggle.hash ) continue;
			groupToggles[i].classList.remove( 'active' );
		}
	}

};
```

Now we can call this method within both of our event listeners.

```javascript
// Show/hide content when toggle is clicked
document.addEventListener('click', function (event) {

	// Get the clicked element
	var toggle = getClosest( event.target, '.click-me' );

	// Make sure a .click-me link was clicked
	if ( !toggle || !toggle.classList.contains( 'click-me' ) ) return;

	// Prevent default
	event.preventDefault();

	// ...

	// If accordion, close other content area
	closeContentToggles(toggle);
	closeContentGroups(content);

}, false);

// Open content area if link within is brought into focus
document.addEventListener('focus', function (event) {

	// If link is in a content area, open it
	var content = getClosest( event.target, '.hide-me' );
	if ( !content ) return;
	content.classList.add( 'active' );

	// Get the content area's toggle
	var toggle = document.querySelector( 'a[href*="#' + content.id + '"].click-me' );
	if ( toggle ) {
		toggle.classList.add( 'active' );
	}

	// If content is part of a group, close all other content areas
	closeContentToggles(toggle);
	closeContentGroups(content);

}, true);
```

Now our expand-and-collapse toggle links can show conditional labels and icons depending on whether or not the content is visible.


## Cleanup

It's pretty normal for scripts to get a bit sloppy as you work on them. When I'm done, I like to clean them up so that they're more structured, better documented, and easier to maintain long-term.

### In-Code Documentation

I'm obsessed with good documentation.

Not only does it make your code easier for other developers to work with. It also makes it easier for you to work with.

There's a good chance you won't work with your code for a while, then come back to it and forget what exactly a particular section of code is supposed to do or why you added it. Adding documentation in your code helps avoid this.

I use the JSDoc^[[http://usejsdoc.org/](http://usejsdoc.org/)] style of code documentation, but use whatever style of system works best for you.

```javascript
/**
 * If content is part of a group, close all other content toggles
 * @param {node} toggle The element that toggles the active content area
 */
var closeContentToggles = function (toggle) {
	var group = getClosest( toggle, '.hide-me-group' );
	if ( group ) {
		var groupToggles = group.querySelectorAll( '.click-me' );
		for ( var i = 0; i < groupToggles.length; i++ ) {
			if ( groupToggles[i].hash === toggle.hash ) continue;
			groupToggles[i].classList.remove( 'active' );
		}
	}
};
```

For a while, there was a push for a CSSDoc^[[https://timkadlec.com/2008/12/manageable-css-with-cssdoc/](https://timkadlec.com/2008/12/manageable-css-with-cssdoc/)] format for CSS documentation that never really took off. I use a slightly less robust version of that, too.

But again, whatever's most comfortable for you.

### Variables, Methods, and Inits

I organize my scripts into three sections:

1. Variables
2. Methods
3. Inits and Event Listeners

When I'm doing with a project, I'll move a few things around to fit this structure.

In Invisible Ink, there are no global variables, so I've omitted that section, but I keep all of my methods together in one spot and run all of my feature tests, inits, and event listeners down to the bottom of the script.

I also use big, easy-to-find-when-scanning-my-code section headers.

```javascript
//
// Methods
//
```

### Move event listeners to named functions

Currently, we're running our event listeners as anonymous functions within the listener.

```javascript
// Show/hide content when toggle is clicked
document.addEventListener('click', function (event) {
	// ...
}, false);
```

I like to move those to named functions under my "Methods" section.

```javascript
//
// Methods
//

/**
 * Handles our click events
 * @param  {event} event The click event
 */
var clickHandler = function (event) {
	// ...
}

//
// Inits and Event Listeners
//

// Listen for click events
document.addEventListener('click', clickHandler, false);
```

Be sure to include `event` as an argument on your handler function. `addEventListener` automatically passes it into the function, but if you don't include it on the function itself, some browsers (like Firefox) won't get the `event` to do anything with.

Named event listener functions can also be removed later in your script if you ever need to.

```javascript
document.removeEventListener( 'click', clickHandler, false );
```

And with that, you've officially completed your first vanilla JavaScript project! Let's try another.

### Make the code DRY

We've already done this, but if there are any blocks of code that are repeated in multiple places, this is also a good time to move them into a named function that you can call in multiple places.

This helps keep our code DRY and easier to maintain.