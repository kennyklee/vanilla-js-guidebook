
# Classes

Add, remove, toggle, and check for classes on an element.

Vanilla JavaScript provides the `classList` API, which works very similar to jQuery's class manipulation APIs.

All modern browsers support it, but IE9 does not. And new versions of IE don't implement all of its features. A small polyfill from Eli Grey provides IE9+ support if needed. ^[[https://github.com/eligrey/classList.js/](https://github.com/eligrey/classList.js/)]


## classList

`classList` is modelled after jQuery's class manipulation APIs.

```javascript
var elem = document.querySelector( '#some-elem' );

// Add a class
elem.classList.add( 'some-class' );

// Remove a class
elem.classList.remove( 'some-other-class' );

// Toggle a class
// (Add the class if it's not already on the element, remove it if it is.)
elem.classList.toggle( 'toggle-me' );

// Check if an element has a specfic class
elem.classList.contains( 'yet-another-class' );
```

### Browser Compatibility

Works in all modern browsers, and IE10 and above. A polyfill from Eli Grey^[[https://github.com/eligrey/classList.js/](https://github.com/eligrey/classList.js/)] extends support back to IE8.


## className

You can use `className` to get all of the classes on an element as a string, add a class or classes, or completely replace or remove all classes.

```javascript
var elem = document.querySelector( 'div' );

// Get all of the classes on an element
var elemClasses = elem.className;

// Add a class to an element
elem.className += ' vanilla-js';

// Completely replace all classes on an element
elem.className = 'new-class';
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.