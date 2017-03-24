
# Cross-Browser Compatibility

Vanilla JavaScript browser compatibility can be inconsistent and difficult to figure out. This is one of the big allures for libraries and frameworks.

In this section, I'm going to teach you a super-simple technique you can use to ensure a great experience for your users, regardless of their web browser.

## Support vs. Optimization

The web is for everyone, but support is not the same as optimization.^[[http://bradfrostweb.com/blog/mobile/support-vs-optimization/](http://bradfrostweb.com/blog/mobile/support-vs-optimization/)]

Rather than trying to provide the same level of functionality for older browsers, we can use progressive enhancement to serve a basic experience to all browsers (even Netscape and IE 5), while newer browsers that support modern APIs and techniques get the enhanced experience.

**To be clear, I’m not advocating dropping support for older and less capable browsers.**

They still have access to all of the content. They just don’t always get the same layout or extra features.


## Cutting the Mustard

"Cutting the Mustard" is a feature detection technique coined by the BBC.^[[http://responsivenews.co.uk/post/18948466399/cutting-the-mustard](http://responsivenews.co.uk/post/18948466399/cutting-the-mustard)]

A simple browser test determines whether or not a browser supports modern JavaScript methods and browser APIs. If it does, the browser gets the enhanced experience. If not, it gets a more basic one.

```javascript
var supports = 'querySelector' in document && 'addEventListener' in window;

if ( supports ) {
	// Codes goes here...
}

// or...

if ( !supports ) return;
```



### What browsers are supported?

To quote the BBC:

> - IE9+
> - Firefox 3.5+
> - Opera 9+ (and probably further back)
> - Safari 4+
> - Chrome 1+ (I think)
> - iPhone and iPad iOS1+
> - Android phone and tablets 2.1+
> - Blackberry OS6+
> - Windows 7.5+ (new Mango version)
> - Mobile Firefox (all the versions we tested)
> - Opera Mobile (all the versions we tested)

### If you're using `classList`

If you're using `classList`, you need to either include the polyfill, or check for `classList` support. Without the polyfill, your IE support starts at IE10 instead of IE9.

```javascript
var supports = 'querySelector' in document && 'addEventListener' in window && 'classList' in document.createElement('_');
```


## Don't hide content until JavaScript loads

If you have an accordion widget, you might use some CSS like this to hide the content:

```css
.accordion {
	display: none;
}
```

When JavaScript adds an `.active` class back on to that content, you show it again like this:

```css
.accordion.active {
	display: block;
}
```

The problem with this approach is that if the visitor's browser doesn't support your JavaScript APIs, or if the file fails to download, or if there's a bug and it break, they'll never be able to access that content.

### Add an activation class

In your scripts, include something like this as part of the initialization process:

```javascript
document.documentElement.className += ' js-accordion';
```

This adds the `.js-accordion` class to the `<html>` element. You can then hook into that class to conditionally hide my accordion content *after* you know my script has loaded and passed the mustard test.

```css
.js-accordion .accordion {
	display: none;
}
```

This ensures that all users can access that content, even if your script breaks.