
# Styles

## jQuery

jQuery uses the `css()` method to get and set styles on an element.

```javascript
var $elem = $( '#a' );

// Get styles
var bgColor = $elem.css( 'background-color' );

// Set styles
$elem.css( 'background-color', '#000000' );
```


## Get Styles

There are a couple different ways to get styles in vanilla JavaScript. The Mozilla Developer Network provides a comprehensive list of available attributes. ^[[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference)]

You can use `style.{css property name}`. This only works for inline styles, however. If you're trying to get a value specified in a stylesheet, you'll get `null` instead.

```javascript
var elem = document.querySelector( '#a' );
var bgColor = elem.style.backgroundColor;
```

`window.getComputedStyle()` gets the actual computed style of an element. This factors in browser default stylesheets as well as external styles you've specified.

```javascript
var elem = document.querySelector( '#a' );
var bgColor = window.getComputedStyle( elem ).backgroundColor;
```


## Set Styles

`style.{css property name}` can also be used to set an inline style on an element.

```javascript
var elem = document.querySelector( '#b' );
elem.style.backgroundColor = '#000000';
```