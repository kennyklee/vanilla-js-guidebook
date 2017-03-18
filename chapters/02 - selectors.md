
# Selectors


## jQuery

In jQuery, you get elements in the DOM using `$()`. This returns an array of matching elements.

To get the first matching element on a page, you would grab the first item in the array.

```javascript
// All divs
var $elems = $( 'div' );

// The first div
var $firstElem = $elems[0];
```


## querySelector()

Use `document.querySelector()` to find the first matching element on a page:

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


## querySelectorAll()

Use `document.querySelectorAll()` to find all matching elements on a page:

```javascript
// Get all elements with the .bg-red class
var elemsRed = document.querySelectorAll( '.bg-red' );

// Get all elements with the [data-snack] attribute
var elemsSnacks = document.querySelectorAll( '[data-snack]' );
```