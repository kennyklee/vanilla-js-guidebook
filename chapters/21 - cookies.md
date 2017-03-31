
# Cookies

How to get, set, and remove cookies.


## Setting a cookie

You can use `document.cookie` to set a cookie.

```javascript
document.cookie = 'sandwich=turkey; expires=Fri, 31 Dec 2024 23:59:59 GMT';
```

### Browser Compatibility

Works in all modern browsers, and at least IE6.


## Getting a cookie value

Getting a cookie value involves parsing a string, and can be a bit awkward to work with. `getCookie()`^[[https://gist.github.com/wpsmith/6cf23551dd140fb72ae7](https://gist.github.com/wpsmith/6cf23551dd140fb72ae7)] is a super lightweight helper method to make this easier.

```javascript
var getCookie = function (name) {
	var value = "; " + document.cookie;
	var parts = value.split("; " + name + "=");
	if (parts.length == 2) return parts.pop().split(";").shift();
};

// Example
var cookieVal = getCookie( 'sandwich' ); // returns "turkey"

### Browser Compatibility

Works in all modern browsers, and at least IE6.


## More complex cookies

If you're doing more complex work with cookies, I would strongly recommend the simple cookie library provided by MDN.^[[https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie/Simple_document.cookie_framework](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie/Simple_document.cookie_framework)]. It let's you easily set, get, and remove cookies.

```javascript
// Set a cookie
docCookies.setItem( 'sandwich', 'turkey with tomato and mayo', new Date(2020, 5, 12) );

// Get a cookie
var cookieVal = docCookies.getItem('sandwich');

// Remove a coookie
docCookies.removeItem( 'sandwich' );
```

### Browser Compatibility

Works in all modern browsers, and at least IE6.