
# HTML

Use `.innerHTML` to get and set HTML content.

```javascript
var elem = document.querySelector( '#some-elem' );

// Get HTML content
var html = elem.innerHTML;

// Set HTML content
elem.innerHTML = 'We can dynamically change the HTML. We can even include HTML elements like <a href="#">this link</a>.';

// Add HTML to the end of an element's existing content
elem.innerHTML += ' Let\'s add this after what is already there.';

// Add HTML to the beginning of an element's existing content
elem.innerHTML = 'We can add this to the beginning. ' + elem.innerHTML;
```

## Browser Compatibility

Works in all modern browsers, and IE9 and above. **IE9 Exception:** Tables and selects require IE10 and above.^[[http://quirksmode.org/dom/html/](http://quirksmode.org/dom/html/)]