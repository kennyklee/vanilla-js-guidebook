
# Invisible Ink: Icons and Labels

There's a good chance you'd like the text on the element that toggles your content to change based on whether or not the content invisible.

For example, "Show more" when it's closed, and "Show less" when it's open. Or different icons for expand and collapse, like plus/minus signs or up/down carets.

## The approach

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

## Uh oh... clicking the icon doesn't toggle the content

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

### We also want to add this functionality on focus

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

## Deactivating other toggles in accordions

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