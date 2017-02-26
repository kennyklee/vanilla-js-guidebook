
# Project: Anchor

The best way to learn vanilla JavaScript is to actually write code. With that in mind, let's work on a project together.

Anchor is tiny little script that adds anchor links to headings.

You can find the starting template in the source code on GitHub. I would strongly encourage you to code along with me, but you can find step-by-step versions of the project in the source code as well.


## The Starting HTML

The starting template for this script is just a webpage with some paragraphs and headings. I pulled some pirate speak from Pirate Ipsum ([http://pirateipsum.me](http://pirateipsum.me/)), but the content and structure don't really matter.

```html
<h1 id="anchors-away">Anchor's Away!</h1>

<p>Fore furl holystone mutiny lugsail hempen halter Brethren of the Coast Corsair. Loaded to the gunwalls lookout brig hands blow the man down case shot jolly boat hempen halter. Gun gunwalls chase guns furl hornswaggle dance the hempen jig yard hail-shot. Pressgang quarterdeck prow lugger spanker fluke broadside reef.</p>

<h2 id="man-o-war">Man O' War</h2>

[...]
```


## Planning

For this script, we want to...

1. Grab all of the headings on the page.
2. Loop through each one, and...
	- Add a link to the end of it.
	- Make the `href` the ID of the heading.
	- Give it some obvious icon or text for people to click.

And that's it, really.


## Scope our Code

Before we get started, let's add an IIFE to keep our code out of the global scope. Let's also throw `use strict` in there so we can aggressively debug.

```javascript
;(function (window, document, undefined) {

	'use strict';

	// Code goes here...

})(window, document);
```


## Adding Links

First, let's get all of the headings on the page. We'll skip the `h1` element, since there should be only one of them, and it's typically at the top of the page.

With `querySelectorAll()`, you can use multiple selectors by separating them with a comma.

```javascript
// Get all headings
var headings = document.querySelectorAll( 'h2, h3, h4, h5, h6' );
```

Next, let's setup a `for` loop.

```javascript
// Get all headings
var headings = document.querySelectorAll( 'h2, h3, h4, h5, h6' );

// Loop through each heading
for (var i = 0; i < headings.length; i++) {
	// Do something...
}
```

Finally, let's add an anchor link to the end of each heading. For this, we'll use `heading[i].id` to get the ID, and `heading[i].innerHTML` to add content to the end of the heading text.

```javascript
// Get all headings
var headings = document.querySelectorAll( 'h2, h3, h4, h5, h6' );

// Loop through each heading
for (var i = 0; i < headings.length; i++) {

	// Add an anchor link to the end of the heading
	headings[i].innerHTML += ' <a href="#' + headings[i].id + '">#</a>';

}
```

And with just 16 lines of code, we've got a working script.

(*It's actually only 4 lines if you exclude the IIFE, all of the comments, and the whitespace.*)


## Check for an ID first

You may have noticed that one of the headings, `Corsair`, has an anchor link that just jumps you to the top of the page.

This is because that heading has no ID, so our script is adding an empty anchor. To prevent this, we should check that the heading has an ID before adding our link.

We'll use a simple `if` statement to see if the heading has an ID. If not, we'll use `continue` to skip to the next heading in the loop.

```javascript
// Loop through each heading
for (var i = 0; i < headings.length; i++) {

	// If the heading has no ID, skip to the next one
	if ( !headings[i].id ) continue;

	// Add an anchor link to the end of the heading
	headings[i].innerHTML += ' <a href="#' + headings[i].id + '">#</a>';

}
```


## Styling the anchor

Our anchor links look pretty ugly. We can address that with some lightweight styling.

First, let's add a class&mdash;`.anchor-link`&mdash;to the anchor that we can hook into.

```javascript
// Add an anchor link to the end of the heading
headings[i].innerHTML += ' <a class="anchor-link" href="#' + headings[i].id + '">#</a>';
```

Next, let's add some CSS. We'll make the link a more subtle gray and remove the underline. On active state, focus, and hover, we can make it blue again and add back the underline.

```css
.anchor-link {
	color: slategray;
	text-decoration: none;
}

.anchor-link:active,
.anchor-link:focus,
.anchor-link:hover {
	color: blue;
	text-decoration: underline;
}
```


## Bonus: Add Missing IDs

Instead of just skipping headings that don't have an ID, let's add an ID to the heading.

For this, we'll take the heading text and transform it in a valid ID. That requires us to remove spaces and invalid characters, and convert it to lower case.

First, we'll use `.replace()` to convert all of the unwanted characters to underscores. We need to use a regex pattern instead of a simple string for this one, and regex is hard. I searched StackOverflow for this one.

```javascript
// If the heading has no ID, give it one
if ( !headings[i].id ) {

	// Convert heading text to valid ID
	// Regex pattern: http://stackoverflow.com/a/9635698/1293256
	headings[i].id = headings[i].innerHTML.replace( /^[^a-z]+|[^\w:.-]+/gi, '_' );

};
```

This is a great start, but the ID starts with an uppercase character, which isn't allowed in certain browsers. Fortunately, we can use `toLowerCase()` to convert it.

```javascript
// If the heading has no ID, give it one
if ( !headings[i].id ) {

	// Convert heading text to valid ID
	// Regex pattern: http://stackoverflow.com/a/9635698/1293256
	headings[i].id = headings[i].innerHTML.replace( /^[^a-z]+|[^\w:.-]+/gi, '_' ).toLowerCase();

};
```

Now, all of our headings will have IDs and anchors.

And with that, this project is done. Nice work!