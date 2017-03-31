
# DOM Injection

How to add and remove elements in the DOM.

## Injecting an element into the DOM

Injecting an element into the DOM requires us to combine a few JavaScript methods.

1. Get the element you want to add our new element before or after.
2. Create our new element using the `createElement()` method.
3. Add content to our element with `innerHTML`.
4. Add any other attributes we want to our element (an ID, classes, etc.).
5. Insert the element using the `insertBefore()` method.

```javascript
// Get the element you want to add your new element before or after
var target = document.querySelector( '#some-element' );

// Create the new element
// This can be any valid HTML element: p, article, span, etc...
var div = document.createElement( 'div' );

// Add content to the new element
div.innerHTML = 'Your content, markup, etc.';

// You could also add classes, IDs, and so on
// div is a fully manipulatable DOM Node

// Insert the element before our target element
// The first argument is our new element.
// The second argument is our target element.
target.parentNode.insertBefore( div, target );

// Insert the element after our target element
// Instead of passing in the target, you pass in target.nextSibling
target.parentNode.insertBefore( div, target.nextSibling );
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above. **IE9 Exception:** Adding content to tables and selects with `innerHTML` require IE10 and above.^[[http://quirksmode.org/dom/html/](http://quirksmode.org/dom/html/)]


## Removing an element from the DOM

You can easily hide an element in the DOM by setting it's `style.display` to `none`.

```javascript
var elem = document.querySelector('#some-element');
elem.style.display = 'none';
```

To *really* remove an element from the DOM, we can use the `removeChild()` method. This method is called against our target element's parent, which we can get with `parentNode`.

```javascript
var elem = document.querySelector('#some-element');
elem.parentNode.removeChild( elem );
```

### Browser Compatibility

Works in all modern browsers, and at least IE6.