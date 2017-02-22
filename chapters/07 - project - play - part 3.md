
# Play: Linking to Specific Videos

Now, let's add the ability to link to specific videos in your player.

Imagine liking a video and wanting to share it or save it for later, but having to go to the starting page and click on the specific video each time. That's annoying.

We want to provide a unique URL that links directly to each video.

As always, hints and tips on the next page.


<div class="break"></div>


## Hints and Tips

### If you're not sure what the key steps are...

1. When the user clicks a video player link, update the URL with a unique video identifier that won't take you to a different page.
2. When someone visits the page, if the URL contains the video ID:
	a. Load that video in the player.
	b. Set the toggle link for that video to active.
	c. Deactivate that default video's toggle link.

### If you're not sure how to update the URL...

There are two options for storing the ID in the URL:

1. A query string (`?video={VIDEO ID}`).
2. A hash value (`#{VIDEO ID}`).

To actually change the URL, you should look into using `window.location`^[[https://developer.mozilla.org/en-US/docs/Web/API/Window/location](https://developer.mozilla.org/en-US/docs/Web/API/Window/location)] or `history.pushState()`^[[https://developer.mozilla.org/en-US/docs/Web/API/History_API#Adding_and_modifying_history_entries](https://developer.mozilla.org/en-US/docs/Web/API/History_API#Adding_and_modifying_history_entries)].

Be aware that some techniques will trigger a page reload, which, while functional, will cause the page to jump, which we don't want.

### If you're not sure how to get the link for the target video...

You can use `.querySelector()` for this. The `href*=` targets a link whose `href` contains the string you use.

```javascript
document.querySelector( '[a href*="' + {VIDEO ID} + '"].play' );
```


<div class="break"></div>


## The Key Steps

To recap, here are the key steps I identified:

1. When the user clicks a video player link, update the URL with a unique video identifier that won't take you to a different page.
2. When someone visits the page, if the URL contains the video ID:
	a. Load that video in the player.
	b. Set the toggle link for that video to active.
	c. Deactivate that default video's toggle link.

## Updating the URL

To store the video ID in our URL without pointing someone to a different page, we have two options:

1. A query string (`?video={VIDEO ID}`).
2. A hash value (`#{VIDEO ID}`).

There are several ways to update the URL with JavaScript.

`window.location`^[[https://developer.mozilla.org/en-US/docs/Web/API/Window/location](https://developer.mozilla.org/en-US/docs/Web/API/Window/location)] updates the URL, but triggers a page reload.

`history.pushState()`^[[https://developer.mozilla.org/en-US/docs/Web/API/History_API#Adding_and_modifying_history_entries](https://developer.mozilla.org/en-US/docs/Web/API/History_API#Adding_and_modifying_history_entries)] changes the browser history without a refresh, but is less supported, harder to implement, and can break forward/backward functionality. It's also generally not a great idea to recreate browser functionality with JavaScript. We want to do the least amount of work to get the biggest impact.

Fortunately, there's a way we can use `window.location` *without* triggering a page reload.

### `window.location` and the hash value

While using `window.location` normally triggers a page reload, updating the `hash` with it does not.

```javascript
window.location.hash = '#{VIDEO ID}';
```

This also provides us with an easy way to change the video when the visitor navigates with the forward and backward buttons. I added this to the `clickHandler`.

```javascript
var clickHandler = function (event) {

	// ...

	// Get the video ID
	var id = /youtu.be/.test(toggle.href) ? toggle.href.split('youtu.be/')[1] : getQueryString( 'v', toggle.href );

	// ...

	// Update the URL
	window.location.hash = '#' + id;

};
```

## Playing a linked video on page load

For this part, I checked to see if a URL `hash` exists when the page loads.

```javascript
if ( window.location.hash ) {
	// Do stuff...
}
```

If it does, I want to validate that the `hash` is a video ID.

To do that, I grab all of our video toggle links. Then, I loop through each one and check if the `href` for that link includes the `hash` value.

```javascript
if ( window.location.hash ) {

	// Get all video toggle links
	var links = document.querySelectorAll( '.play' );

	for (var i = 0; i < links.length; i++) {

		// Check if hash matches video ID
		var id = /youtu.be/.test(links[i].href) ? links[i].href.split('youtu.be/')[1] : getQueryString( 'v', links[i].href );
		if ( window.location.hash.replace('#', '') !== id ) continue;

		// Otherwise, we have match!

	}

}
```

You can see I'm using the same tenary operator as before to get the video ID from the link. If there's no match, the page will just load as normal.

If we do have a match, we can grab the video player and load the video. This time, we're not include the `autoplay` query string key. Since the page is loading for the first time, autoplay would be a negative user experience.

```javascript
if ( window.location.hash ) {

	// Get all video toggle links
	var links = document.querySelectorAll( '.play' );

	for (var i = 0; i < links.length; i++) {

		// Check if hash matches video ID
		var id = /youtu.be/.test(links[i].href) ? links[i].href.split('youtu.be/')[1] : getQueryString( 'v', links[i].href );
		if ( window.location.hash.replace('#', '') !== id ) continue;

		// Get the video player
		var player = document.querySelector( '.video iframe' );
		if ( !player ) return;

		// Play video
		player.src = 'https://www.youtube.com/embed/' + window.location.hash.replace('#', '') + '?rel=0';

	}

}
```

## Change the active link

Once we have a match and update the video, we want to set that video's toggle link to active and deactive the default link.

To do that, we'll grab the `.play` link that currently has the `.active` class and remove it. Then, we'll add the class to the link that matched.

```javascript
if ( window.location.hash ) {

	// Get all video toggle links
	var links = document.querySelectorAll( '.play' );

	for (var i = 0; i < links.length; i++) {

		// ...

		// Deactivate currently active link
		var active = document.querySelector( '.play.active' );
		if ( active ) {
			active.classList.remove( 'active' );
		}

		// Activate current video
		links[i].classList.add( 'active' );

	}

}
```

Now, visitors can link directly to their favorite video.