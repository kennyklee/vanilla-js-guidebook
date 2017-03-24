
# Query Strings

`getQueryString` is a helper method I wrote to get the value of a query string from a URL. Pass in the key to get the value of. You can optionally pass in a URL as a second argument. The function will use the window URL by default.

```javascript
/**
 * Get the value of a query string from a URL
 * @param  {String} field The field to get the value of
 * @param  {String} url   The URL to get the value from [optional]
 * @return {String}       The value
 */
var getQueryString = function ( field, url ) {
    var href = url ? url : window.location.href;
    var reg = new RegExp( '[?&]' + field + '=([^&#]*)', 'i' );
    var string = reg.exec(href);
    return string ? string[1] : null;
};

// Examples
// http://example.com?sandwich=turkey&snack=cookies
var thisOne = getQueryString( 'sandwich' ); // returns 'turkey'
var thatOne = getQueryString( 'snack' ); // returns 'cookies'
var anotherOne = getQueryString( 'dessert' ); // returns null
var passedInURL = getQueryString( 'drink', 'http://another-example.com?drink=soda' ); // returns 'soda'
```

## Browser Compatibility

Works in all modern browsers, and at least IE6.