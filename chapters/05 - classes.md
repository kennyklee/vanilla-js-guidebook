
# Classes

Vanilla JavaScript provides the `classList` API, which works very similar to jQuery's class manipulation APIs.

All modern browsers support it, but IE9 does not. And new versions of IE don't implement all of its features. A small polyfill from Eli Grey provides IE9+ support if needed. ([https://github.com/eligrey/classList.js/](https://github.com/eligrey/classList.js/))

## classList.add()

Add a class to an element.

```javascript
// jQUery
var $elem = $( '#a' );
$elem.addClass( 'jquery' );

// Vanilla JS
var elem = document.querySelector( '#a' );
elem.classList.add( 'vanilla-js' );
```


## classList.remove()

Remove a class from an element.

```javascript
// jQuery
var $elem = $( '#a' );
$elem.removeClass( 'bg-navy' );

// Vanilla JS
var elem = document.querySelector( '#c' );
elem.classList.remove( 'bg-red' );
```


## classList.toggle()

Add a class if the element does not currently have that class. Remove it if it does.

```javascript
// jQuery
var $elem = $( '#a' );
$elem.toggleClass( 'bg-navy' );

// Vanilla JS
var elem = document.querySelector( '#c' );
elem.classList.toggle( 'bg-green' );
```


## classList.contains()

Check if an element contains a class.

```javascript
// jQuery
var $elem = $( '#a' );
if ( $elem.hasClass( 'bg-navy' ) ) {
	// Do something...
}

// Vanilla JS
var elem = document.querySelector( '#a' );
if ( elem.classList.contains( 'bg-navy' ) ) {
	// Do something...
}
```


## className

You can use `className` to get all of the classes on an element as a string, add a class or classes, or completely replace or remove all classes.

```javascript
// Get all of the classes on an element
var elem = document.querySelector( 'div' );

// Add a class to an element
elem.className += ' vanilla-js';

// Replace all classes on an element
elem.className = 'new-class';
```


## The Lab: Classes

For this lab:

1. Get the all of the `<divs>` that have a `[data-sandwich]` value of `ham`.
2. Loop through each one.
3. If it has a class of `.bg-red`, remove it.
3. If it has a class of `.bg-blue`, change it to `.bg-red`.
5. If it has a class of `.bg-navy`, change it to `.bg-green`.