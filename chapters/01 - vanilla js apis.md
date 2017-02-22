
# Vanilla JavaScript equivalents of common jQuery APIs

I want to show you how easy it is to ditch jQuery by sharing some vanilla JavaScript equivalents of common jQuery APIs. Unless otherwise noted, these provide support for IE9 and above.

You can find a more comprehensive list in the *Ditching jQuery Cheat Sheet* that came with this book.

## ECMAScript 5

ECMAScript 5, more commonly known as ES5, is a more modern interation of JavaScript that added a ton of useful APIs that hadn't previously existed.

Many modern web and ECMAScript 5 APIs wouldn't exist at all if jQuery hadn't grown so popular and pushed demand for them. It's made working on the web much easier. Thanks jQuery!

At the time of writing, ES6 is the latest iteration, ES7 (now called ES2016) is in the works, and new versions are coming every year. Browser support for ES6 and beyond is inconsistent, however, and the resulting code looks very different from traditional JavaScript and jQuery.

As a result, we'll be focusing on ES5 APIs, which are wildly supported and work perfectly for what we're trying to do.

## Selectors

In jQuery, you get elements in the DOM using `$()`. This returns an array of matching elements.

```javascript
var elemsByClass = $( '.some-class' );
var elemsById = $( '#some-id' );
var elemsByData = $( '[data-example]' );
```

HTML5 provides two APIs to select elements in the DOM. `document.querySelector()` gets the first matching element, while `document.querySelectorAll()` returns a node list of all matching elements.

Supported in IE8, but only for CSS 2.1 selectors.

```javascript
// Get the first matching element
var firstClass = document.querySelector( '.some-class' );
var firstId = document.querySelector( '#some-id' );
var firstData = document.querySelector( '[data-example]' );

// Get all matching elements
var allClasses = document.querySelectorAll( '.some-class' );
var allData = document.querySelectorAll( '[data-example]' );
```

## Looping through objects

In jQuery, you can iterate of arrays, objects, and node lists with the `.each()` method.

```javascript
$.each([ 52, 97 ], function( index, value ) {
	console.log(index) // index
	console.log(value) // value
});
```

In vanilla JavaScript, you use `for` loops to do the same thing. These are supported all the way back to IE6.

```javascript
// Arrays and node lists
var elems = document.querySelectorAll( '.some-class' );
for ( var i = 0, len = elems.length; i < len; i++ ) {
    console.log(i) // index
    console.log(elems[i]) // object
}

// Objects
var obj = {
    apple: 'yum',
    pie: 3.214,
    applePie: true
};
for ( var prop in obj ) {
    if ( Object.prototype.hasOwnProperty.call( obj, prop ) ) {
        console.log( prop ); // key
        console.log( obj[prop] ); // value
    }
}
```

***Note:*** *Todd Motto has created a simple helper method^[[https://github.com/toddmotto/foreach](https://github.com/toddmotto/foreach)] that’s useful if you frequently loop over objects.*

## Class manipulation

jQuery provides a set of methods for adding, removing, and checking for classes.

```javascript
var elem = $( '#some-element' );

// Add class
elem.addClass( 'some-class' );

// Remove class
elem.removeClass( 'some-other-class' );

// Add or remove class
elem.toggleClass( 'some-other-class' );

// Check for class
if ( elem.hasClass( 'some-third-class' ) ) {
	console.log( 'yep!' );
}
```

Vanille JavaScript makes it really easy to do this same thing with the classList API. Support starts with IE10, but a polyfill provides support back to IE8.^[[https://github.com/eligrey/classList.js/](https://github.com/eligrey/classList.js/)] You should always use it.

```javascript
var elem = document.querySelector( '#some-element' );

// Add class
elem.classList.add( 'some-class' );

// Remove class
elem.classList.remove( 'some-other-class' );

// Add or remove class
elem.classList.toggle( 'some-other-class' );

// Check for class
if ( elem.classList.contains( 'some-third-class' ) ) {
    console.log( 'yep!' );
}
```

## Manipulate styles

jQuery uses the `.css()` method to get and add styles to an element.

```javascript
var elem = $( '#some-element' );

// Get a CSS attribute
var color = elem.css( 'color' );

// Set a CSS attribute
elem.css( 'color', 'rebeccapurple' );
```

In vanilla JavaScript, you can do the same thing with the `style` attribute. This is supported all the way back to IE6.

```javascript
var elem = document.querySelector( '#some-element' );

// Get a CSS attribute
var color = elem.style.color;

// Set a CSS attribute
elem.style.color = 'rebeccapurple';

// Get a CSS attribute
var height = elem.style.minHeight;

// Set a CSS attribute
elem.style.minHeight = '200px';
```

***Note:*** *Not sure what the right property name is? I Google these all the time!*

## Manipulate attributes

jQuery uses the `.attr()` and `.data()` methods to add, remove, and check for attributes.

```javascript
var elem = $( '.some-element' );

// Get data attribute
var att = elem.data( 'example' );

// Set data attribute
elem.data( 'example', 'Hello world' );

// Get an ID
var id = elem.attr( 'id' );

// Set an ID
elem.attr( 'id', 'some-element' );
```

In vanilla JavaScript, you can use `getAttribute`, `setAttribute`, and `hasAttribute` to get and set all sorts of attributes—--not just data attributes. However, there’s usually an API you can call directly on the element, too.

```javascript
var elem = document.querySelector( '#some-element' );

// Get data attribute
var att = elem.getAttribute( 'data-example' );

// Set data attribute
elem.setAttribute( 'data-example', 'Hello world' );

// Check data attribute
if ( elem.hasAttribute( 'data-example' ) ) {
	console.log( 'yep!' );
}

// Set an ID
elem.setAttribute( 'id', 'new-id' );
elem.id = 'new-id';

// Set width
elem.setAttribute( 'width', '200px' );
elem.width = '200px';

// Get title
var title = elem.getAttribute( 'title' );
var titleToo = elem.title;
```

## Event Listeners

jQuery uses the `.on()` method to listen for events, and also provides specifically named event listeners like `.click()` and `.hover()`.

```javascript
$( '#some-element' ).on( 'click', function( event ) {
    // Do stuff
});
```

In vanilla Javascript, you can do the same thing with `addEventListener`. Listen for clicks, hovers, and more.^[[https://developer.mozilla.org/en-US/docs/Web/Events](https://developer.mozilla.org/en-US/docs/Web/Events)]

```javascript
var elem = document.querySelector( '.some-class' );
elem.addEventListener( 'click', function( event ) {
    // Do stuff
}, false);
```

Unlike jQuery, each event requires its own listener, but you can assign a function to a variable to keep your code more DRY.

```javascript
var elem = document.querySelector( '.some-class' );
var someFunction = function ( event ) {
    // Do stuff
};
elem.addEventListener( 'click', someFunction, false );
elem.addEventListener( 'mouseover', someFunction, false );
```

And if you need to pass multiple variables into a function assigned to a variable, use the `.bind()` method. The first variable is the one assigned to this, and event is automatically passed in as the last variable.

```javascript
var elem = document.querySelector( '.some-class' );
var someFunction = function ( var1, var2, var3, event ) {
    // Do stuff
}
elem.addEventListener('click', someFunction.bind( null, var1, var2, var3 ), false);
elem.addEventListener('mouseover', someFunction.bind( null, var1, var2, var3 ), false);
```

***Note:*** *`.bind()` was a late addition to ECMAScript 5, and some otherwise compliant browsers don’t support it. You should include the polyfill^[[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Polyfill](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Polyfill)] if you use it.*

With named functions, you can also remove event listeners.

```javascript
elem.removeEventListener( 'click', someFunction, false );
elem.removeEventListener( 'mouseover', someFunction, false );
```

If you need to apply the same event listener on multiple elements, you *can* loop through each element and add a listener.

A better and more performant approach, though, is to listen for events on the entire document and filter just the elements you need. This is know as *event bubbling*.

```javascript
/**
 * Function to filter what's clicked and run your functions
 * @param  {Event} event The event
 */
var eventHandler = function ( event ) {

    // Get the clicked element
    var toggle = event.target;

    // If clicked element is the one you're looking for, run your methods
    if ( toggle.hasAttribute( 'data-example' ) || toggle.classList.contains( 'sample-class' ) ) {
        event.preventDefault(); // Prevent default click event
        someMethod( the, arguments, to, pass, into, method );
    }

};

// Listen for all click events on the document
document.addEventListener( 'click', eventHandler, false );
```

## Waiting until the DOM is ready

jQuery uses the `.ready()` method to wait until the DOM is ready before running code.

```javascript
$( document ).ready(function() {
    // Do stuff
});
```

Here's a super lightweight vanilla JavaScript equivalent.

While modern browsers support the `DOMContentReady` event listener, the code won’t execute if it’s called after the DOM is loaded (the event it’s listening for has already happened). The `ready()` method provided below executes your scripts immediately if the DOM is ready, and waits until it is if it’s not.

Under `readyState`, `interactive` runs once the document is done but before all images and stylesheets have been downloaded. `complete` runs after that stuff is downloaded, too. I’ve included both for completeness.

```javascript
/**
 * Run event after DOM is ready
 * @param  {Function} fn Callback function
 */
var ready = function ( fn ) {

    // Sanity check
    if ( typeof fn !== 'function' ) return;

    // If document is already loaded, run method
    if ( document.readyState === 'interactive' || document.readyState === 'complete' ) {
        return fn();
    }

    // Otherwise, wait until document is loaded
    document.addEventListener( 'DOMContentLoaded', fn, false );

};

// Example
ready(function() {
    // Do stuff...
});
```

***Note:*** *If you're loading your scripts in the footer (which you should be for performance reasons), the `ready` method isn't really needed. It's just a habit from the "load everything in the header" days.*

## HTML content

In jQuery, you use the `.html()` method.

```javascript
var elem = $( '#some-element' );

// Get HTML
var html = elem.html();

// Set HTML
elem.html( 'Hello world!' );
```

In vanilla JavaScript, you can use `innerHTML` to do the same thing. This is actually what jQuery uses under the hood anyways.

```javascript
var elem = document.querySelector( '#some-element' );

// Get HTML
var html = elem.innerHTML;

// Set HTML
elem.innerHTML = 'Hello world!';
```

## More APIs

You can find a more comprehensive list in the *Ditching jQuery Cheat Sheet* that came with this book, but at some point, you'll need to do something that's not included there.

My first go-to site for anything JavaScript related is the Mozilla Developer Network^[[https://developer.mozilla.org/](https://developer.mozilla.org/)], which is essentially a user guide for the web.

They provide documentation on tons of web and JS APIs, with examples, browser compatibility information, and polyfills when needed. Just add `mdn` to your Google searches.

If that fails, I turn to Stack Overflow^[[http://stackoverflow.com/](http://stackoverflow.com/)]. Make sure to add `vanilla js` to your searches. Typing `without jQuery` returns a ton of jQuery-based responses instead. Also be aware that answers can vary in quality, so don't just take the first answer you get at face value.