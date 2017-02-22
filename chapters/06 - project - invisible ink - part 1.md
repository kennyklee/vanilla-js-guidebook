
# Invisible Ink: The Basics

The best way to learn vanilla JavaScript is to actually write code.

For our first project, Invisible Ink, you and I are going to build an expand-and-collapse/accordion script. In the `projects` directory that came with your book, you'll find all of the source files for each section of this project.

The project is broken up into five parts:

1. **The Basics.** Getting a minimally viable script set up.
2. **a11y.** Adding accessibility features.
3. **Accordion Functionality.** Adding true accordion functionality (where one section closes if another is opened).
4. **Icons and Labels.** Adding the ability to change the link text depending on whether the content is expanded or collapsed.
5. **Cleanup.** Writing scripts is messy. We should cleanup and organize our code.

You can find screencasts of me working through each part---including all of the mess ups and debugging process---in the `screencasts` directory.

## The Template

In `The Basics` directory, the `jQuery` subdirectory includes the jQuery way of writing this script, while the `Vanilla JS` subdirectory includes, of course, the vanilla JS code.

The `Template` subdirectory includes a template you can use to write your own code as we go through the project. Every other part of the project only includes completed vanilla JavaScript code.

Our starting HTML looks like this:

```html
<div class="hide-me" id="hide-me">
	<p>Here's some content that I'd like to show or hide when the link is clicked.</p>
</div>

<p>
	<a class="click-me" href="#hide-me">
		Click Me
	</a>
</p>
```

Just a simple `<div>` with a class and ID, and an anchor link pointing to that content with a class on it.

When someone clicks a link with the `.click-me` class, we want to show the content in the element whose ID matches the anchor link.

## The Key Steps

1. When someone clicks a `.click-me` link, prevent the default link action from happening.
2. Get the content area who's ID matches the anchor link target.
3. If the content area is visible, hide it. If it's hidden, show it.

## The jQuery Way

Before we get into it, let's look at how you might write a script like this with jQuery.

```javascript
$( document ).ready(function() {

	// When the .click-me element is clicked
	$( '.click-me' ).on( 'click', function( event ) {

		// Prevent the default click behavior
		event.preventDefault();

		// Toggle visibility of the div that matches it's href/hash
		$( event.target.hash ).toggle();

	});

});
```

Here, when an element with the `.click-me` class is clicked, we prevent the default click behavior. `event.target` is the item was clicked for this event.

Next, we get that links `hash` (the anchor link in the href), and find the content area whose ID matches it.

Then, we toggle visibility of that content. The jQuery `.toggle()` methods adds or removes `display: none` as an inline style on the element.

If we want to hide that element by default, we would just add `style="display: none;"` to it.

## The Vanilla JavaScript Way

In vanilla JS we can use `addEventListener` to detect the click event.

```javascript
var link = document.querySelector( '.click-me' );
link.addEventListener('click', function (event) {
	// Do stuff when the link is clicked
}, false);
```

Just like in jQuery, we can prevent the default click behavior:

```javascript
var link = document.querySelector( '.click-me' );
link.addEventListener('click', function (event) {

	// Prevent the default click behavior
	event.preventDefault();

}, false);
```

In vanilla JS, you *could* toggle `display: none` using the `style` attribute, but I think it's easier to maintain and gives you more flexibility to add these to an external stylesheet. We'll add a style that hides our content areas by default, and shows them a second class is present.

```css
.hide-me {
	display: none;
}

.hide-me.active {
	display: block;
}
```

That `.active` class can be anything you want. I know some folks prefer more action or state-oriented class names like `.is-open`. Do whatever fits your approach to CSS.

The nice thing with using a class to control visibility is that if you want a content area to be visibly by default, you can just add the `.active` class to it as the starting state in your markup.

```html
<div class="hide-me active" id="hide-me">
	<p>Here's some content that I'd like to show or hide when the link is clicked.</p>
</div>
```

And then we'll toggle that class with JavaScript when the link is clicked using `classList`. The `.toggle()` method adds the class if it's missing, and removes it if it's present, just like jQuery's `.toggleClass()` method.

Just like in jQuery, we use `event.target.hash` to get the hash from the clicked link.

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

## What if there's more than one content area?

The code we've got so far is great when there's just a single expand-and-collapse area, but what if you multiple ones, like this?

```html
<div class="hide-me" id="hide-me">
	<p>1. Here's some content that I'd like to show or hide when the link is clicked.</p>
</div>

<p>
	<a class="click-me" id="click-me-1" href="#hide-me">
		Click Me 1
	</a>
</p>

<div class="hide-me" id="hide-me-2">
	<p>2. Here's some content that I'd like to show or hide when the link is clicked.</p>
</div>

<p>
	<a class="click-me" id="click-me-2" href="#hide-me-2">
		Click Me 2
	</a>
</p>

<div class="hide-me" id="hide-me-3">
	<p>3. Here's some content that I'd like to show or hide when the link is clicked.</p>
</div>

<p>
	<a class="click-me" id="click-me-3" href="#hide-me-3">
		Click Me 3
	</a>
</p>
```

You *could* write add an event listener for every content area, but that's bloated and annoying to maintain.

```javascript
/**
 * DON'T DO THIS!
 */
var link = document.querySelector( '#click-me-1' );
var link2 = document.querySelector( '#click-me-2' );
var link3 = document.querySelector( '#click-me-3' );

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

link3.addEventListener('click', function (event) {

	// Prevent the default click behavior
	event.preventDefault();

	// Show or hide the content
	var content = document.querySelector( event.target.hash );
	if ( !content ) return;
	content.classList.toggle( 'active' );

}, false);
```

Seriously, don't ever, ever do that.

You might also consider grabbing all of your links, looping through them, and adding an event listener for each one.

```javascript
var links = document.querySelectorAll( '.click-me' );
for (var i = 0; i < links.length; i++) {
	links[i].addEventListener(...);
}
```

First, this will throw an error, as you're not allowed to add event listeners in a loop. There are ways around it, but you still shouldn't do it.

It's bad for browser performance to have a bunch of event listeners on a bunch of different elements. And, if ever add one after the DOM has loaded, it won't have a listener attached.

The solution: event bubbling.

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