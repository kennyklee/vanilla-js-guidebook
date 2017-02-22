
# Play: Cleanup

Now that we have a fully functional script, let's go ahead and clean it up for easier maintenance and readability going forward.

Hints and tips are on the next page.


<div class="break"></div>


## Hints and Tips

There are four key steps to cleaning up the script...

1. Add any missing in-code documentation.
2. Organize our code into logical groups. I personally like Variables, Methods, and Inits/Event Listeners.
3. Move anonymous event listeners to named functions, if you haven't already.
4. Move any repeated code into a function we can call in multiple places.


<div class="break"></div>


## Key Steps

1. I went through an added in-code documentation to all of the methods and tasks.
2. I grouped all of my methods together under an easily scannable heading, and did the same thing with my feature test, inits, and event listeners.
3. My event listeners were already in named functions, but if they weren't, I would have done that now.
4. I moved my repeated code inside the `clickHandler()` and `if` statement into a function I can call in both places.
5. I also moved our check for a video ID in the URL on page load to a named function.

## Adding a function to play videos

The code to load a video was repeated inside my `clickHandler()` and the `if` statement that runs on page load.

I moved this code into a new function called `playVideo()`. I need to pass in the toggling link as an argument. I also added argument to control autoplay (which we use on click but not on page load), and an argument for the event so we can prevent the default click event behavior.

Additionally, if a video exists, I return it's ID so we can use it to update the URL.

```javascript
/**
 * Play the selected video
 * @param  {node}    toggle   The link that toggled the video
 * @param  {boolean} autoplay If true, autoplay the video
 * @param  {event}   event    The click event [optional]
 * @return {number}      The video ID
 */
var playVideo = function (toggle, autoplay, event) {

	// Get the video player
	var playerSelector = toggle.hasAttribute( 'data-player' ) ? toggle.getAttribute( 'data-player' ) : '.video';
	var player = document.querySelector( playerSelector + ' iframe' );
	if ( !player ) return;

	// Prevent the default link action
	if ( event ) {
		event.preventDefault();
	}

	// Get the video ID
	var id = /youtu.be/.test(toggle.href) ? toggle.href.split('youtu.be/')[1] : getQueryString( 'v', toggle.href );

	// Play video
	var play = autoplay ? 'autoplay=1' : '';
	player.src = 'https://www.youtube.com/embed/' + id + '?rel=0&' + play;

	// Deactivate currently active link
	var activeSelector = toggle.hasAttribute( 'data-player' ) ? '[data-player="' + toggle.getAttribute( 'data-player' ) + '"]' : '';
	var active = document.querySelector( activeSelector + '.play.active' );
	if ( active ) {
		active.classList.remove( 'active' );
	}

	// Activate current video
	toggle.classList.add( 'active' );

	// Return the ID of the video
	return id;

};
```

Now, I can call this in both spots in my code.

```javascript
var clickHandler = function (event) {

	// Check to see if a play link was clicked
	var toggle = getClosest(event.target, '.play');
	if ( !toggle ) return;

	// Play the video
	var video = playVideo(toggle, true, event);

	// Update the URL
	if ( video ) {
		window.location.hash = '#' + video;
	}

};

// ...

if ( window.location.hash ) {

	// Get all of the play links
	var links = document.querySelectorAll( '.play' );

	// Check to see if the hash matches
	for (var i = 0; i < links.length; i++) {

		// Check if hash matches video ID
		var id = /youtu.be/.test(links[i].href) ? links[i].href.split('youtu.be/')[1] : getQueryString( 'v', links[i].href );
		if ( window.location.hash.replace('#', '') !== id ) continue;

		// Play video
		playVideo(links[i]);
	}

}
```

## Moving our check for a video ID in the URL

To keep our `Inits and Event Listeners` section more organized, I moved the check for a video ID in the URL to a function.

```javascript
/**
 * If a video ID is included in the URL, load that video
 */
var playVideoOnload = function () {

	if ( window.location.hash ) {

		// Get all of the play links
		var links = document.querySelectorAll( '.play' );

		// Check to see if the hash matches
		for (var i = 0; i < links.length; i++) {

			// Check if hash matches video ID
			var id = /youtu.be/.test(links[i].href) ? links[i].href.split('youtu.be/')[1] : getQueryString( 'v', links[i].href );
			if ( window.location.hash.replace('#', '') !== id ) continue;

			// Play video
			playVideo(links[i]);
		}
	}

};
```

I call this function immediately when the script loads.

```javascript
//
// Inits and Event Listeners
//

// Feature Test
var supports = 'querySelector' in document && 'addEventListener' in window;
if ( !supports ) return;

// Load a video if video ID is included in the URL
playVideoOnload();

// Listen for click events
document.addEventListener('click', clickHandler, false);
```

And with that, this project is done.