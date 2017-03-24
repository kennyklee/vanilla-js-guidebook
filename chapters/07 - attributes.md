
# Attributes

Get and set attributes for an element.

## getAttribute(), setAttribute(), and hasAttribute()

The `getAttribute()`, `setAttribute()`, and `hasAttribute()` methods let you get, set, and check for the existance of attributes (including data attributes) on an element.

```javascript
var elem = document.querySelector( '#some-elem' );

// Get the value of an attribute
var sandwich = elem.getAttribute( 'data-sandwich' );

// Set an attribute value
elem.setAttribute( 'data-sandwich', 'turkey' );

// Check if an element has an attribute
if ( elem.hasAttribute( 'data-sandwich' ) ) {
	// do something...
}
```

These methods can also be used to manipulate other types of attributes (things like `id`, `tabindex`, `name`, and so on), but these are better done by calling the attribute on the element directly (see below).

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.


## Element Attributes

You can get and set attributes directly on an element. View a full list of HTML attributes on the Mozilla Developer Network.^[[https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)]

```javascript
var elem = document.querySelector( '#some-elem' );

// Get attributes
var id = elem.id;
var name = elem.name;
var tabindex = elem.tabindex;

// Set attributes
elem.id = 'new-id';
elem.title = 'The title for this thing is awesome!';
elem.tabindex = '-1';
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.