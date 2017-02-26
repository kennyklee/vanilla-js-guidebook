
# HTML

## jQuery

jQuery uses the `.html()` method to get and set HTML.

```javascript
var $elem = $( '#a' );

// Get HTML
var html = $elem.html();

// Set HTML
$elem.html( 'We can dynamically change the HTML. We can even include HTML elements like <a href="#">this link</a>.' );
```


## Get HTML

In vanilla JavaScript you can get HTML for an element using the `innerHTML` API.

```javascript
var elem = document.querySelector( '#a' );
var html = elem.innerHTML;
```


## Set HTML

You can also use the `innerHTML` API to set HTML.

```
var elem = document.querySelector( '#a' );
elem.innerHTML = 'We can dynamically change the HTML. We can even include HTML elements like <a href="#">this link</a>.';
```


## The Lab: HTML

For this lab:

1. Get the first element with an attribute of `[data-snack]` equal to `chips`.
2. Add to the existing HTML with a short note about chips.