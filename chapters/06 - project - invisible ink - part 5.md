
# Invisible Ink: Cleanup

It's pretty normal for scripts to get a bit sloppy as you work on them. When I'm done, I like to clean them up so that they're more structured, better documented, and easier to maintain long-term.

## In-Code Documentation

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

## Variables, Methods, and Inits

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

## Move event listeners to named functions

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

## Make the code DRY

We've already done this, but if there are any blocks of code that are repeated in multiple places, this is also a good time to move them into a named function that you can call in multiple places.

This helps keep our code DRY and easier to maintain.