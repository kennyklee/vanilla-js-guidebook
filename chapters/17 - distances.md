
# Distances

## Get the currently scrolled distance from the top of the page

Use `pageYOffset` to get the distance the user has scrolled from the top of the page.

```javascript
var distance = window.pageYOffset;
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above.


## Get an element's distance from the top of the page

`getOffsetTop` is a helper method I wrote to get an element's distance from the top of the document.

```javascript
/**
 * Get an element's distance from the top of the Document.
 * @private
 * @param  {Node} elem The element
 * @return {Number}    Distance from the top in pixels
 */
var getOffsetTop = function ( elem ) {
	var location = 0;
	if (elem.offsetParent) {
		do {
			location += elem.offsetTop;
			elem = elem.offsetParent;
		} while (elem);
	}
	return location >= 0 ? location : 0;
};

// Example
var elem = document.querySelector( '#some-element' );
var distance = getOffsetTop( elem );
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above.