
# Play: Multiple Video Players

While this is perhaps an unlikely scenario, it's possible that someone might include two different video players on a single page. Currently, if you added a second video player and links, all of the videos will load in the first player.

Let's update our script so that each set of video links plays in it's own video player.

As always, if you get stuck, hints and tips are on the next page.


<div class="break"></div>


## Hints and Tips

### If you're not sure what the key steps are...

1. When a visitor clicks a video link, get the target player for that video.
2. Load video in the appropriate player.

### If you're not sure how to get the right player...

Currently, we're using `.querySelector()` to grab the iframe in the first `.video` element. We instead want to grab the `.video` element specific to the video that was clicked.

There are two ways to do this:

1. Wrap the player and video links in a parent container, and find the player within that parent.
2. Add a unique identifier for the player to each link as a data attribute.


<div class="break"></div>


## The Key Steps

To recap, here are the key steps I identified:

1. When a visitor clicks a video link, get the target player for that video.
2. Load video in the appropriate player.

## Restructuring the markup

To allow for greater layout flexibility, I decided to use a data attribute to each link to target the correct video player. I gave each player a unique ID, and passed in that ID via the `[data-player]` attribute.

```html
<div class="video" id="video-1">
	<iframe width="560" height="315" src="https://www.youtube.com/embed/QnM2tVZr7Fs?rel=0" frameborder="0" allowfullscreen></iframe>
</div>

<p><strong>Videos</strong></p>

<ul>
	<li>
		<a class="play active" data-player="#video-1" href="https://www.youtube.com/watch?v=QnM2tVZr7Fs">Who got to the North Pole first?</a>
	</li>
	<li>
		<a class="play" data-player="#video-1" href="https://www.youtube.com/watch?v=QWveXdj6oZU">Rapping, deconstructed</a>
	</li>
	<li>
		<a class="play" data-player="#video-1" href="https://www.youtube.com/watch?v=rNu8XDBSn10">The Difference between the United Kingdom, Great Britain and England Explained</a>
	</li>
	<li>
		<a class="play" data-player="#video-1" href="https://youtu.be/hUhisi2FBuw">The Ingenious Design of the Aluminum Beverage Can</a>
	</li>
</ul>
```

## Getting the correct video player

Inside my `clickHandler`, I grab the video player ID from the clicked link. If none exists, I use the generic `.video` class. This allows people who are not using multiple players to exclude the data attribute.

I get the player iframe with `.querySelector()` scoped to the target player.

```javascript
var clickHandler = function (event) {

	// Check to see if a play link was clicked
	var toggle = getClosest(event.target, '.play');
	if ( !toggle ) return;

	// Get the video player
	var playerSelector = toggle.hasAttribute( 'data-player' ) ? toggle.getAttribute( 'data-player' ) : '.video';
	var player = document.querySelector( playerSelector + ' iframe' );
	if ( !player ) return;

	// ...

};
```

I can use the exact same setup in the `if` statement that runs on page load, simply changing `toggle` to `links[i]` to get the current link in the loop.

```javascript
// Get the video player
var playerSelector = links[i].hasAttribute( 'data-player' ) ? links[i].getAttribute( 'data-player' ) : '.video';
var player = document.querySelector( playerSelector + ' iframe' );
```

## Targeting the right currently active video link

With our current setup, our script with deactivate the currently active link for the first player regardless of which player's link was clicked.

We need to only search for an active link for our target player. To do that, we'll scope our `.querySelector()` search to links whose `[data-player]` attribute matches our player. If no player ID is provided, will instead just search the whole document.

```javascript
// Deactivate currently active link
var activeSelector = toggle.hasAttribute( 'data-player' ) ? '[data-player="' + toggle.getAttribute( 'data-player' ) + '"]' : '';
var active = document.querySelector( activeSelector + '.play.active' );
if ( active ) {
	active.classList.remove( 'active' );
}
```

Don't forget to make this update in the `if` statement loop, too.

And thanks it. Now we can load as many video players onto the page as we want, and arrange them in any layout that we want.