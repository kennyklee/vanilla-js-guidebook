
# Traversing Up the DOM

## parentNode

Use `parentNode` to get the parent of an element.

```javascript
var elem = document.querySelector( '#some-elem' );
var parent = elem.parentNode;
```

You can also string them together to go several levels up.

```javascript
var levelUpParent = elem.parentNode.parentNode;
```

### Browser Compatibility

Works in all modern browsers, and at least back to IE6.


## getClosest()

`getClosest()` is a helper method I wrote to get the closest parent up the DOM tree that matches against a selector. It's a vanilla JavaScript equivalent to jQuery's `.closest()` method.

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
var elem = document.querySelector( '#some-elem' );
var closestSandwich = getClosest( elem, '[data-sandwich]' );
```

***Note:*** *This method can also be used in event listeners to determine if the `event.target` is inside of a particular element or not (for example, did a click happen inside of a dropdown menu?).*

### Browser Compatibility

Works in all modern browsers, and IE9 and above.


## getParents()

`getParents()` is a helper method I wrote that returns an array of parent elements, optionally matching against a selector. It's a vanilla JavaScript equivalent to jQuery's `.parents()` method.

It starts with the element you've passed in itself, so pass in `elem.parentNode` to skip to the first parent element instead.

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
var elem = document.querySelector( '#some-elem' );
var parents = getParents( elem.parentNode );
var parentsWithWrapper = getParents( elem.parentNode, '.wrapper' );
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above.


## getParentsUntil()

`getParentsUntil()` is a helper method I wrote that returns an array of parent elements until a matching parent is found, optionally matching against a selector. It's a vanilla JavaScript equivalent to jQuery's `.parentsUntil()` method.

It starts with the element you've passed in itself, so pass in `elem.parentNode` to skip to the first parent element instead.

```javascript
/**
 * Get all of an element's parent elements up the DOM tree until a matching parent is found
 * @param  {Node}   elem     The element
 * @param  {String} parent   The selector for the parent to stop at
 * @param  {String} selector The selector to filter against [optionals]
 * @return {Array}           The parent elements
 */
var getParentsUntil = function ( elem, parent, selector ) {

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

        if ( parent ) {
            if ( elem.matches( parent ) ) break;
        }

        if ( selector ) {
            if ( elem.matches( selector ) ) {
                parents.push( elem );
            }
            break;
        }

        parents.push( elem );

    }

    return parents;

};

// Examples
var elem = document.querySelector( '#some-element' );
var parentsUntil = getParentsUntil( elem, '.some-class' );
var parentsUntilByFilter = getParentsUntil( elem, '.some-class', '[data-something]' );
var allParentsUntil = getParentsUntil( elem );
var allParentsExcludingElem = getParentsUntil( elem.parentNode );
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above.