
# Climbing Down the DOM

## querySelector() and querySelectorAll()

You can use jQuery's `.find()` method to find elements that match a selector within another element.

```javascript
var $elem = $( '.wrapper' );
var divs = $elem.find( 'div' );
```

The `querySelector()` and `querySelectorAll()` APIs aren't limited to just running on the `document`. They can be run on any element, and provide the same functionality as jQuery's `.find()`.

```javascript
var elem = document.querySelector( '.wrapper' );
var snack = elem.querySelector( '[data-snack]' );
var divs = elem.querySelectorAll( 'div' );
```


## ChildNodes

While `querySelector()` and `querySelectorAll()` search through all levels within a nested DOM/HTML structure, you may want to just get immediate decedants of a particular element.

jQuery's `.children()` method does this.

```javascript
var $elem = $( '#d' );
var decendants = $wrapper.children();
```

The native `childNodes` API does the same thing.

```javascript
var elem = document.querySelector( '#d' );
var decendants = wrapper.childNodes;
```