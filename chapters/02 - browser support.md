
# Cutting the Mustard

The web is for everyone, but support is not the same as optimization.^[[http://bradfrostweb.com/blog/mobile/support-vs-optimization/](http://bradfrostweb.com/blog/mobile/support-vs-optimization/)]

Rather than trying to provide the same level of functionality for older browsers, I use progressive enhancement to serve a basic experience to all browsers (even Netscape and IE 5). Newer browsers that support modern APIs and techniques get the enhanced experience.

**To be clear, I’m not advocating dropping support for older and less capable browsers.**

They still have access to all of the content. They just don’t always get the same layout or extra features.

## A simple feature detection technique

I used a feature detection technique that the BBC calls “cutting the mustard.”^[[http://responsivenews.co.uk/post/18948466399/cutting-the-mustard](http://responsivenews.co.uk/post/18948466399/cutting-the-mustard)]

A simple browser test determines whether or not a browser supports modern JavaScript APIs. If it does, it gets the enhanced experience. If not, it gets a more basic one.

```javascript
var supports = 'querySelector' in document && 'addEventListener' in window;
if ( !supports ) return;
```

The `supports` variable targets two modern APIs we use to "cut the mustard." The `!supports` if statement stops running the script of the browser doesn’t support the appropriate APIs.

## What browsers are supported?

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

## Hiding content after the JS is loaded

In my scripts, after the mustard test is run, I include this line of code which adds a class to the `<html>` element after the script is loaded.

```javascript
document.documentElement.className += ' js-MyPlugin';
```

In my CSS, I can prefix styles with `.js-MyPlugin` to ensure that JS-specific styles are only applied in supported browsers after the required files have downloaded. Of course, I change `MyPlugin` to some name relevant to my script.

For example, if I was building an accordion script, I would make sure the script had loaded before hiding any content, like so:

```css
.js-accordion .collapse {
	display: none;
}
```

This approach does result in some FOUT^[[http://en.wikipedia.org/wiki/Flash_of_unstyled_content](http://en.wikipedia.org/wiki/Flash_of_unstyled_content)], but it’s worth it to ensure that users can always access content.