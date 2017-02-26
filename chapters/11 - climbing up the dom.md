
# Climbing Up the DOM

## parentNode

You can use `parentNode` to get the parent of an element. This is the same as jQuery's `.parent()` method.

```javascript
// jQuery
var $elem = $( '#d3' );
var parent = $elem.parent();


// Vanilla JS
var elem = document.querySelector( '#d3' );
var parent = elem.parentNode;
```

You can also string them together to go several levels up.

```javascript
var levelUpParent = elem.parentNode.parentNode;
```


## getClosest()

jQuery's `.closest()` method provides you with a way to get the closest parent up the DOM tree that matches against a selector.

```javascript
var $elem = $( '#d3' );
var closestSandwich = $elem.closest( '[data-sandwich]' );
```

While there's no native vanilla JavaScript API to handle this, a simple helper method achieves the same result.

```javascript
/**
 * Get the closest matching element up the DOM tree.
 * @private
 * @param  {Element} elem     Starting element
 * @param  {String}  selector Selector to match against
 * @return {Boolean|Element}  Returns null if not match found
 */
var getClosest = function ( elem, selector ) {

	// Element.matches() polyfill
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

	// Get closest match
	for ( ; elem && elem !== document; elem = elem.parentNode ) {
		if ( elem.matches( selector ) ) return elem;
	}

	return null;

};

// Example
var elem = document.querySelector( '#d3' );
var closestSandwich = getClosest( elem, '[data-sandwich]' );
```

***Note:*** *This method can also be used in event listeners to determine if the `event.target` is inside of a particular element or not (for example, did a click happen inside of a dropdown menu?).*


## getParents()

jQuery's `.parents()` method returns an array of parent elements, optionally matching against a selector.

```javascript
var $elem = $( '#d3' );
var parents = $elem.parents();
var parentsWithWrapper = $elem.parents( '.wrapper' );
```

The `getParents()` helper method provide below does the same thing. It starts with the element itself, so pass in `elem.parentNode` to skip to the first parent element instead.

```javascript
/**
 * Get all of an element's parent elements up the DOM tree
 * @param  {Node}   elem     The element
 * @param  {String} selector Selector to match against [optional]
 * @return {Array}           The parent elements
 */
var getParents = function ( elem, selector ) {

	// Element.matches() polyfill
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

	// Setup parents array
	var parents = [];

	// Get matching parent elements
	for ( ; elem && elem !== document; elem = elem.parentNode ) {

		// Add matching parents to array
		if ( selector ) {
			if ( elem.matches( selector ) ) {
				parents.push( elem );
			}
		} else {
			parents.push( elem );
		}

	}

	return parents;

};

// Example
var elem = document.querySelector( '#d3' );
var parents = getParents( elem.parentNode );
var parentsWithWrapper = getParents( elem.parentNode, '.wrapper' );
```


## The Lab: Climbing Up the DOM

For this lab:

1. Get the element with an ID of `#d3`.
2. Get all of the elements parents.
3. Add the `.bg-green` class to all of them until you get to the `document.body`.