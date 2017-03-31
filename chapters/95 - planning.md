
# Planning out your script

This may sound ridiculous, but I actually plan out my scripts on paper before I ever open a text editor. Like, real paper. In a physical notebook. With a pen.

Code is really just a series of small steps, run one after another, that tell computers (or in our case, browsers) exactly what to do.

I find it helpful to identify what each of those steps should be ahead of time, which makes it easier for me to think through which APIs and methods I need to use, which elements on the page I need to target, and so on.

## An example

Here's an actual example from when I created my Right Height plugin.^[[https://github.com/cferdinandi/right-height](https://github.com/cferdinandi/right-height)]

Right Height will take a set of content areas that are different heights and make them all the same height. When those content areas are all stacked one-on-top-of-the-other (say, in a single-column mobile view), it leaves them at their natural height.

First, I sketched out what I was trying to accomplish on paper.

![Sketching out rightHeight.js](assets/img/right-height.jpg)

Next, I identified and wrote out each of the steps.

1. Get all of the content areas in a group.
2. Determine if the content areas are side-by-side or stacked one-on-top-of-the-other.
	- If they're side-by-side...
		1. Get the height of the tallest content area.
		2. Set each content area to the height of the tallest one.
	- If they're one-on-top-of-the-other...
		1. Remove any height setting and let the content flow naturally.
3. If the browser is resized, do it all over again.

And here's how that translated those steps into specific tasks for the script.

1. Get all of the content areas in a group.
	a. Wrap content in a parent element with a selector (like `.right-height` or `[data-right-height]`).
	b. Use `.querySelectorAll()` to get all content groups.
	c. Loop through each group, and use `.querySelectorAll()` to get all content areas within it.
2. Determine if the content areas are side-by-side or stacked one-on-top-of-the-other.
	- Get the distance from the top of the first two content areas to the top of the page. If they're the same, the content areas are side-by-side. If they're different, they're stacked.
	- If they're side-by-side...
		1. Get the height of the tallest content area.
			a. Set a variable for `height` to `0`.
			b. Loop through each content area and measure its height. If it's greater than the current `height` variable value, reset `height` to that content area's height.
		2. Set each content area to the height of the tallest one.
			a. Loop through each content area again and give it a `style.height` equal to the `height` variable value.
	- If they're one-on-top-of-the-other...
		1. Remove any height setting and let the content flow naturally.
			a. Loop through each content area and set the `style.height` to nothing.
3. If the browser is resized, do it all over again.
	a. Add a event listener for browser resizing.

Obviously, there's a bit more to writing code than just outlining the steps. But this outline gives me a huge headstart on actually writing the script and helps keep me focused on the bigger tasks I need to get done.

Give it a try!