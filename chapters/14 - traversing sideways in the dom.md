
# Traversing Sideways in the DOM

`getSiblings` is a helper method I wrote that gets the siblings of an element in the DOM. For example: if you had a list item (`<li>`) and wanted to grab all of the other items in the list.

```javascript
/**
 * Get all siblings of an element
 * @param  {Node}  elem The element
 * @return {Array}      The siblings
 */
var getSiblings = function ( elem ) {
    var siblings = [];
    var sibling = elem.parentNode.firstChild;
    for ( ; sibling; sibling = sibling.nextSibling ) {
        if ( sibling.nodeType === 1 && sibling !== elem ) {
            siblings.push( sibling );
        }
    }
    return siblings;
};

// Example
var elem = document.querySelector( '#some-element' );
var siblings = getSiblings( elem );
```

## Browser Compatibility

Works in all modern browsers, and IE6 and above.