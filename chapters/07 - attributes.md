
# Attributes

## jQuery

jQuery uses the `data()` method to get and set attributes on an element.

```javascript
var $elem = $( '#a' );

// Get attribute
var dataSandwich = $elem.data( 'sandwich' );
var elemId = $elem.data( 'id' );

// Set attribute
$elem.data( 'sandwich', 'ham' );
$elem.data( 'id', 'abc' );
```


## getAttribute()

The `getAttribute()` API let's you get attributes from an element.

```javascript
var elem = document.querySelector( '#a' );

// Get the [data-sandwich] attribute value
var dataSandwich = elem.getAttribute( 'data-sandwich' );
```

It can be used to get attributes like ID, title, and so on as well, but this is better done using the attribute directly. You can view a full list of HTML attributes on the Mozilla Developer Network. ([https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes))

```javascript
// These do the same thing
var elemId1 = elem.getAttribute( 'id' );
var elemId2 = elem.id;
```


## setAttribute()

Use `setAttribute()` to set attributes on an element.

```javascript
var elem = document.querySelector( '#a' );
elem.setAttribute( 'data-sandwich', 'turkey' );
```

As with `getAttribute()`, you can use `setAttribute()` to set things like ID, title, tabindex and more, though this is better done using the attribute directly.

```javascript
// These do the same thing
elem.setAttribute( 'title', 'Nope, turkey!' );
elem.title = 'Nope, turkey!';
```


## hasAttribute()

Use the `hasAttribute()` API to check if an element has a specific attribute.

```javascript
var elem = document.querySelector( '#a' );
if ( elem.hasAttribute( 'data-snack' ) ) {
	// Do something...
}
```


## The Lab: Attributes

For this lab:

1. Get all `<divs>`.
2. Loop through each one.
3. If it has an attribute of `[data-sandwich]`, give it a class of `.bg-green`.
4. If the `[data-sandwich]` attribute is `ham`, remove all classes.