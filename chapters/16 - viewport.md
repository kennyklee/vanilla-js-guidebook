
# The Viewport

## Get the viewport height

There are two methods to get the viewport height: `window.innerHeight` and `document.documentElement.clientHeight`. The former is more accurate. The latter has better browser support.

To get the best of both worlds, try `innerHeight` first, and fallback to `clientHeight` if not supported.

```javascript
var viewportHeight = window.innerHeight || document.documentElement.clientHeight;
```

### Browser Compatibility

`innerHeight` works in all modern browsers, and IE9 and above. `clientHeight` works in all modern browsers, and IE6 and above.


## Get the viewport width

There are two methods to get the viewport width: `window.innerWidth` and `document.documentElement.clientWidth`. The former is more accurate. The latter has better browser support.

To get the best of both worlds, try `innerWidth` first, and fallback to `clientWidth` if not supported.

```javascript
var viewportWidth = window.innerWidth || document.documentElement.clientWidth;
```

### Browser Compatibility

`innerWidth` works in all modern browsers, and IE9 and above. `clientWidth` works in all modern browsers, and IE6 and above.


## Check if an element is in the viewport or not

`isInViewport` is a helper method I wrote to check if an element is in the viewport or not. It returns `true` if the element is in the viewport, and `false` if it's not.

```javascript
/**
 * Determine if an element is in the viewport
 * @param  {Node}    elem The element
 * @return {Boolean}      Returns true if element is in the viewport
 */
var isInViewport = function ( elem ) {
    var distance = elem.getBoundingClientRect();
    return (
        distance.top >= 0 &&
        distance.left >= 0 &&
        distance.bottom <= (window.innerHeight || document.documentElement.clientHeight) &&
        distance.right <= (window.innerWidth || document.documentElement.clientWidth)
    );
};

// Example
var elem = document.querySelector( '#some-element' );
isInViewport( elem ); // Boolean: returns true/false
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above.