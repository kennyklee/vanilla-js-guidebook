
# Invisible Ink: Accordion Functionality

Right now, our script works great for things like "read more" links.

Now, let's add the ability to use it like a real accordion, where if you open a content area, the others in the group close.

## Creating an accordion group

To do this we're going to add a parent element around the content areas we want to function as an accordion. Let's give it a class of `.hide-me-group`.

Let's also move the links to toggle the content areas above their target content areas.

```html
<div class="hide-me-group">
	<p>
		<a class="click-me" href="#hide-me">
			Click Me 1
		</a>
	</p>
	<div class="hide-me" id="hide-me">
		<p>1. Here's some content that I'd like to show or hide when the link is clicked.</p>
	</div>

	<p>
		<a class="click-me" href="#hide-me-2">
			Click Me 2
		</a>
	</p>
	<div class="hide-me" id="hide-me-2">
		<p>2. Here's some content that I'd like to show or hide when the <a href="#">link</a> is clicked.</p>
	</div>

	<p>
		<a class="click-me" href="#hide-me-3">
			Click Me 3
		</a>
	</p>
	<div class="hide-me" id="hide-me-3">
		<p>3. Here's some content that I'd like to show or hide when the link is clicked.</p>
	</div>
</div>
```

## Closing all of the content areas in a group

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

Next, we want to loop through them and close them, with one exception: we want to leave open the content area that toggled.

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

## Closing accordions when tabbing through links

In the last chapter, we added the ability to open a content area if we tab onto a link inside it.

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