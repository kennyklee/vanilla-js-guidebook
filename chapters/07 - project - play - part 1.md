
# Play: The Basics

Let's get started building our YouTube video player.

For this phase of the project, let's setup a simple webpage with a list of video links. When a link is clicked, we want to update the YouTube video being played at the top of the page.

## The Template

Under `Play` in the `projects` directory, there's a subdirectory called `0. Starting Template`.

This has a simple page with some starting HTML, and empty `scripts.js` and `styles.css` files for you.

```html
<!-- Our video will get played here -->
<div class="video" id="video"></div>

<!-- The list of videos -->
<p><strong>Videos</strong></p>
<ul>
	<li>
		<a href="#">Video 1</a>
	</li>
	<li>
		<a href="#">Video 1</a>
	</li>
	<li>
		<a href="#">Video 1</a>
	</li>
	<li>
		<a href="#">Video 1</a>
	</li>
	<li>
		<a href="#">Video 1</a>
	</li>
</ul>
```

## Some useful things to know

You don't have to create a custom video player. You can just use the YouTube iFrame embed code.

```html
<iframe width="560" height="315" src="https://www.youtube.com/embed/{VIDEO ID}?rel=0" frameborder="0" allowfullscreen></iframe>
```

`{VIDEO ID}` gets replaced with the ID for the video. In YouTube URLs, this is the string of characters after `?v=`, or after the `/` on their `https://youtu.be/` sharing links.

## Let's get started

1. Plan out the key steps for your script.
2. Get some actual videos from YouTube that you'd like to use for your player.
3. Start writing your JavaScript.

If you get stuck, I've got some tips and hints to help get you started on the next page.


<div class="break"></div>


## Hints and Tips

### If you're not sure what the key steps are...

This is what I came up with:

1. When the user clicks a video player link, prevent the default link action.
2. Get the ID for the video from that link. (*Not sure how? See the next section.*)
3. Update the embedded video player with the new video's ID.


### If you're not sure how to get the player ID...

There are a few techniques you can use to store this information in the link.

1. You can use a data attribute^[[https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_data_attributes](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_data_attributes)] to store the video ID on the link.
	```javascript
	<a data-video-id="123456" href="#">Video 1</a>
	```

2. You can grab the ID from the URL itself.
	- Using the normal URL, the video ID is just a query string, and *Ditching jQuery Cheat Sheet* includes a method for getting a query string value from a URL.
	- Using the `https://youtu.be` sharing link, you'll need to get the string from the end of the URL. A search on MDN should help you get started.


### If you're not sure how to update the video player with the new video...

There are two approach you can use.

1. Completely replace the embed code within the video player container with an updated one using `innerHTML` (the native JS equivalent of jQuery's `.html()`) method.
2. Get the embedded video player from inside the container and update it's `src` with the new video ID.

Once you're happy with how things are going, skip to the next page and I'll walk you through how I approached this project.


<div class="break"></div>


## The Key Steps

Just to recap, here's what I identified as the key steps:

1. When the user clicks a video player link, prevent the default link action.
2. Get the ID for the video from that link.
3. Update the embedded video player with the new video's ID.

## Setting up the HTML

I added a unique class, `.play`, to my video links. I also added links directly to those videos on YouTube as `href` values, so that if the script failed, users would get redirected to YouTube to watch.

```html
<a class="play" href="https://www.youtube.com/watch?v=QnM2tVZr7Fs">Who got to the North Pole first?</a>
```

I also added a default video so that the player isn't empty when someone first visits the page.

```html
<div class="video" id="video">
	<iframe width="560" height="315" src="https://www.youtube.com/embed/QnM2tVZr7Fs?rel=0" frameborder="0" allowfullscreen></iframe>
</div>
```

## Listening for Clicks

I set up an event listener for clicks.

```javascript
var clickHandler = function (event) {
	// Do stuff...
};

document.addEventListener('click', clickHandler, false);
``

Then, I used the `getClosest()` method to check if the clicked element was a video link.

```javascript
var clickHandler = function (event) {

	// Check to see if a play link was clicked
	var toggle = getClosest(event.target, '.play');
	if ( !toggle ) return;

};
```

If the clicked element is a video link, I grab the embedded video player iframe and prevent the default link action.

```javascript
var clickHandler = function (event) {

	// Check to see if a play link was clicked
	var toggle = getClosest(event.target, '.play');
	if ( !toggle ) return;

	// Get the video player
	var player = document.querySelector( '.video iframe' );
	if ( !player ) return;

	// Prevent the default link action
	event.preventDefault();

};
```

## Getting the Video ID

Next, I grab the video ID from the YouTube URL.

To do this, I first check if the URL is a `youtu.be` share link using `.test()` for regex matching. (Todd Motto has a great article on this topic that you should definitely read.^[[https://toddmotto.com/understanding-regular-expression-matching-with-test-match-exec-search-and-split/](https://toddmotto.com/understanding-regular-expression-matching-with-test-match-exec-search-and-split/)])

If it is, I use the `.split()` API to grab the string at the end. `.split()` generates an array. We want the second item, (with a key of `[1]` in the array). You'll notice I left off the `https://`. Since there's a chance someone could paste in a non-SSL version of the URL, I left it off.

If it's just a standard YouTube URL, I use the `getQueryString()` method (from the *Ditching jQuery Cheat Sheet*) to grab the value of `v` from the URL.

```javascript
var clickHandler = function (event) {

	// ...

	// Get the video ID
	var id = /youtu.be/.test(toggle.href) ? toggle.href.split('youtu.be/')[1] : getQueryString( 'v', toggle.href );

};
```

In the snippet above, I've written my variable on a single line instead of using an if/then statement. This is known as a tenary operator.^[[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)]

The stuff before the `?` is your if statement. The part immediately after it but before the `:` is what to do if the if statement is `true`. The part after the `:` is what the variable is set to if the if statement is `false`.

Feel free to use if/else statements if you prefer them. I like the brevity of tenary operators when possible.

## Updating the Player

Finally, I update the video player `src` with the new video ID.

```javascript
var clickHandler = function (event) {

	// ...

	// Get the video ID
	var id = /youtu.be/.test(toggle.href) ? toggle.href.split('youtu.be/')[1] : getQueryString( 'v', toggle.href );

	// Play video
	player.src = 'https://www.youtube.com/embed/' + id + '?rel=0&autoplay=1';

};
```

I decided to add `autoplay=1` to end of the embed `src`. This tells YouTube to autoplay the video.

Autoplay is typically terrible for users and you should avoid using it. But in this case, if someone clicks a link to play the video, the expectation is that video starts playing immediately, so it makes sense ot use it.

And at just 3kb, you've got a working vanilla JavaScript YouTube video player.