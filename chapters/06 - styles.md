
# Styles

Get and set styles (ie. CSS) for an element.

Vanilla JavaScript uses camel cased versions of the attributes you would use in CSS. The Mozilla Developer Network provides a comprehensive list of available attributes and their JavaScript counterparts.^[[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference)]

## Inline Styles

Use `.style` to get and set inline styles for an element.

```javascript
var elem = document.querySelector( '#some-elem' );

// Get a style
// If this style is not set as an inline style directly on the element, it returns an empty string
var bgColor = elem.style.backgroundColor;

// Set a style
elem.style.backgroundColor = 'purple';
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.


## Computed Styles

Use `window.getComputedStyle()` gets the actual computed style of an element. This factors in browser default stylesheets as well as external styles you've specified.

```javascript
var elem = document.querySelector( '#some-elem' );
var bgColor = window.getComputedStyle( elem ).backgroundColor;
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above.