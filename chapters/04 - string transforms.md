
# String Transforms

Helpful functions for modifying strings.


## trim()

`.trim()` is used to remove whitespace from the beginning and end of a string.

```javascript
var text = '  This sentence has some whitespace at the beginning and end of it.  ';
var trimmed = text.trim();
// returns 'This sentence has some whitespace at the beginning and end of it.'
```

### Browser Compatibility

Works in all modern browsers, and IE9 and above. The following polyfill^[[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim#Polyfill](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim#Polyfill)] can be used to push support back to IE6.

```javascript
if (!String.prototype.trim) {
	String.prototype.trim = function () {
		return this.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '');
	};
}
```


## toLowerCase()

Transform all text in a string to lowercase.

```javascript
var text = 'This sentence has some MIXED CASE LeTTeRs in it.';
var lower = text.toLowerCase();
// returns 'this sentence has some mixed case letters in it.'
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.


## toUpperCase()

Transform all text in a string to uppercase.

```javascript
var text = 'This sentence has some MIXED CASE LeTTeRs in it.';
var upper = text.toUpperCase();
// returns 'THIS SENTENCE HAS SOME MIXED CASE LETTERS IN IT.'
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.


## parseInt()

Convert a string into an integer (a whole number). The second argument, `10`, is called the `radix`. This is the base number used in mathematical systems. For our use, it should always be `10`.

```javascript
var text = '42px';
var integer = parseInt( text, 10 );
// returns 42
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.


## parseFloat()

Convert a string into a point number (a number with decimal points).

```javascript
var text = '3.14someRandomStuff';
var pointNum = parseFloat( text );
// returns 3.14
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.


## replace()

Replace a portion of text in a string with something else.

```javascript
var text = 'I love Cape Cod potato chips!';
var lays = text.replace( 'Cape Cod', 'Lays' );
var soda = text.replace( 'Cape Cod potato chips', 'soda' );
var extend = text.replace( 'Cape Cod', 'Cape Cod salt and vinegar' );

// lays: 'I love Lays potato chips!'
// soda: 'I love soda!'
// extend: 'I love Cape Cod salt and vinegar potato chips!'
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.


## slice()

Get a portion of a string starting (and optionally ending) at a particular character.

The first argument is where to start. Use `0` to include the first character. The second argument is where to end (and is optional).

If either argument is a negative integer, it will start at the end of the string and work backwards.

```javascript
var text = 'Cape Cod potato chips';
var startAtFive = text.slice( 5 );
var startAndEnd = text.slice( 5, 8 );
var sliceFromTheEnd = text.slice( 0, -6 );

// startAtFive: 'Cod potato chips'
// startAndEnd: 'Code'
// sliceFromTheEnd: 'Cape Cod potato'
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.


## split()

Convert a string into an array by splitting it after a specific character (or characters).

The first argument, the `delimiter`, the character or characters to split by. As an optional second argument, you can stop splitting your string after a certain number of delimiter matches have been found.

```javascript
var text = 'Soda, turkey sandwiches, potato chips, chocolate chip cookies';
var menu = text.split( ', ' );
var limitedMenu = text.split( ', ', 2 );

// menu: ["Soda", "turkey sandwiches", "potato chips", "chocolate chip cookies"]
// limitedMenu: ["Soda", "turkey sandwiches"]
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.