
# Traversing Down the DOM

## querySelector() and querySelectorAll()

The `querySelector()` and `querySelectorAll()` APIs aren't limited to just running on the `document`. They can be run on any element to search only for elements inside of it.

```javascript
var elem = document.querySelector( '#some-elem' );

// Find the first element inside `#some-elem` that has a `[data-snack]` attribute
var snack = elem.querySelector( '[data-snack]' );

// Get all divs inside `#some-elem`
var divs = elem.querySelectorAll( 'div' );
```

## Browser Compatibility

Same as `querySelectorAll` and `querySelector`.


## children

While `querySelector()` and `querySelectorAll()` search through all levels within a nested DOM/HTML structure, you may want to just get immediate decedants of a particular element. Use `children` for this.

```javascript
var elem = document.querySelector( '#some-elem' );
var decendants = wrapper.children;
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above.