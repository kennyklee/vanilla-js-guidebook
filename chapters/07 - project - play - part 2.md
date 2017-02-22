
# Play: The Active Video

Now that we've got our player working, we can make a few enhancements.

Let's style the link for the video that's currently playing so that visitors can tell which video in the list they're watching. For bonus points, make that link inactive so visitors can't click it again.

There are hints and tips to get you started on the next page.


<div class="break"></div>


## Hints and Tips

### If you're not sure what the key steps are...

1. When the user clicks a video player link, remove the `.active` class from the currently active link.
2. Add an `.active` class the clicked link.

### If you're not sure how to remove the class from the currently active link...

You can find the currently active link using `.querySelector()`. Look for the `.play` link that has the `.active` class on it.


<div class="break"></div>


## The Key Steps

To recap, here are the key steps I identified:

1. When the user clicks a video player link, remove the `.active` class from the currently active link.
2. Add an `.active` class the clicked link.

## Adding an active link style

In the `styles.css` file, I added some styles for the active link.

```css
.play.active {
	color: #808080;
	cursor: not-allowed;
	pointer-events: none;
}
```

I set the active link to a muted gray color. I also added `cursor: not-allowed` and `pointer-events: none`. These prevent the link from being clicked by the user---ideal for a video that's already playing.

There are JavaScript-based ways to achieve the same result, but for what we're doing, I think this is sufficient.

## Changing the active link

In my `clickHandler()` method, I grab the currently active link and remove the `.active` class. Then, I add the class to the clicked link.

```javascript
var clickHandler = function (event) {

	// Check to see if a play link was clicked
	var toggle = getClosest(event.target, '.play');
	if ( !toggle ) return;

	// ...

	// Deactivate currently active link
	var active = document.querySelector( '.play.active' );
	if ( active ) {
		active.classList.remove( 'active' );
	}

	// Activate current video
	toggle.classList.add( 'active' );

};
```

And that's it. Now the currently playing video has a unique style and can't be clicked by the visitor.