
# Selectors

Get elements in the DOM.

## querySelectorAll()

Use `document.querySelectorAll()` to find all matching elements on a page. You can use any valid CSS selector.

```javascript
// Get all elements with the .bg-red class
var elemsRed = document.querySelectorAll( '.bg-red' );

// Get all elements with the [data-snack] attribute
var elemsSnacks = document.querySelectorAll( '[data-snack]' );
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above. Can also be used in IE8 with CSS2.1 selectors (no CSS3 support).


## querySelector()

Use `document.querySelector()` to find the first matching element on a page.

```javascript
// The first div
var elem = document.querySelector( 'div' );

// The first div with the .bg-red class
var elemRed = document.querySelector( '.bg-red' );

// The first div with a data attribute of snack equal to carrots
var elemCarrots = document.querySelector( '[data-snack="carrots"]' );

// An element that doesn't exist
var elemNone = document.querySelector( '.bg-orange' );
```

If an element isn't found, `querySelector()` returns `null`. If you try to do something with the nonexistant element, an error will get thrown. You should check that a matching element was found before using it.

```javascript
// Verify element exists before doing anything with it
if ( elemNone ) {
	// Do something...
}
```

If you find yourself doing this a lot, here's a helper method you can use that will fail gracefully by returning a dummy element rather than cause an error.

```javascript
var getElem = function ( selector ) {
	return document.querySelector( selector ) || document.createElement( '_' );
};
getElem( '.does-not-exist' ).id = 'why-bother';
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above. Can also be used in IE8 with CSS2.1 selectors (no CSS3 support).


## getElementById()

Use `getElementById()` to get an element by its ID.

```javascript
var elem = getElementById( '#some-selector' );
```

### Browser Compatibility

Works in all modern browsers, and at least IE6.


## getElementsByClassName()

Use `getElementsByClassName()` to find all elements on a page that have a specific class or classes.

***Note:*** *This returns a live HTMLCollection of elements. If an element is added or removed from the DOM after you set your variable, the list is automatically updated to reflect the current DOM.*

```javascript
// Get elements with a class
var elemsByClass = document.getElementsByClassName( 'some-class' );

// Get elements that have multiple classes
var elemsWithMultipleClasses = document.getElementsByClassName( 'some-class another-class' );
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above.


## getElementsByTagName()

Use `getElementsByTagName()` to get all elements that have a specific tag name.

***Note:*** *This returns a live HTMLCollection of elements. If an element is added or removed from the DOM after you set your variable, the list is automatically updated to reflect the current DOM.*

```javascript
// Get all divs
var divs = document.getElementsByTagName( 'div' );

// Get all links
var links = document.getElementsByTagName( 'a' );
```

### Browser Compatibility

Works in all modern browsers, and at least IE6.


## matches()

Use `matches()` to check if an element would be selected by a particular selector or set of selectors. Returns `true` if the element is a  match, and `false` when it's not. This function is analogous to jQuery's `.is()` method.

```javascript
var elem = document.querySelector( '#some-elem' );
if ( elem.matches( '.some-class' ) ) {
	console.log( 'It matches!' );
} else {
	console.log( 'Not a match... =(' );
}
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above.

**But...** several browser makes implemented it with nonstandard, prefixed naming. If you want to use it, you should include this polyfill^[[https://developer.mozilla.org/en-US/docs/Web/API/Element/matches#Polyfill](https://developer.mozilla.org/en-US/docs/Web/API/Element/matches#Polyfill)] to ensure consistent behavior across browsers.

```javascript
if (!Element.prototype.matches) {
	Element.prototype.matches =
		Element.prototype.matchesSelector ||
		Element.prototype.mozMatchesSelector ||
		Element.prototype.msMatchesSelector ||
		Element.prototype.oMatchesSelector ||
		Element.prototype.webkitMatchesSelector ||
		function(s) {
			var matches = (this.document || this.ownerDocument).querySelectorAll(s),
				i = matches.length;
			while (--i >= 0 && matches.item(i) !== this) {}
			return i > -1;
		};
}
```


## A word about selector performance

Selectors that target a specific element type, like `getElementById()` and `getElementsByClassName()`, are more than twice as fast^[[https://jsperf.com/getelementbyid-vs-queryselector/25](https://jsperf.com/getelementbyid-vs-queryselector/25)] as `querySelector()` and `querySelectorAll()`.

So, that's bad, right? I honestly don't think it matters.

`getElementById()` can run about 15 million operations a second, compared to *just* 7 million per second for `querySelector()` in the latest version of Chrome. But that also means that `querySelector()` runs 7,000 operations a millisecond. A millisecond. Let that sink in.

That's absurdly fast. Period.

Yes, `getElementById()` and `getElementsByClassName()` are faster. But the flexibility and consistency of `querySelector()` and `querySelectorAll()` make them the obvious muscle-memory choice for my projects.

They're not slow. They're just not *as* fast.