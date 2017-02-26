
# String Transforms

In this section, we're going to look at a handful of helpful APIs for modifying strings. Most of them have no jQuery equivalent.


## trim()

`.trim()` is used to remove whitespace from the beginning and end of a string. This is the only API that has a jQuery equivalent.

```javascript
var text = '  This sentence has some whitespace at the beginning and end of it.  ';

// jQuery
var $trimmed = $.trim( text );


// Vanilla JS
var trimmed = text.trim();
```


## toLowerCase()

Transform all text in a string to lowercase.

```javascript
var text = 'This sentence has some MIXED CASE LeTTeRs in it.';
var lower = text.toLowerCase();
```


## toUpperCase()

Transform all text in a string to uppercase.

```javascript
var text = 'This sentence has some MIXED CASE LeTTeRs in it.';
var upper = text.toUpperCase();
```


## parseInt()

Convert a string into an integer (number). That second argument, `10`, is called the `radix`. This is the base number used in mathematical systems. For our use, it should always be `10`.

```javascript
var text = '42px';
var integer = parseInt( text, 10 );
```


## replace()

Replace a portion of text in a string with something else.

```javascript
var text = 'I love Cape Cod potato chips!';
var lays = text.replace( 'Cape Cod', 'Lays' );
var soda = text.replace( 'Cape Cod potato chips', 'soda' );
var extend = text.replace( 'Cape Cod', 'Cape Cod salt and vinegar' );
```


## slice()

Get a portion of a string starting (and optionally ending) at a particular character.

The first argument is where to start. Use `0` to include the first character.

The second argument is where to end (and is optional). If negative, it will start at the end of the string and work backwards.

```javascript
var text = 'Cape Cod potato chips';
var startAtFive = text.slice( 5 );
var startAndEnd = text.slice( 5, 8 );
var sliceFromTheEnd = text.slice( 0, -6 );
```


## split()

Convert a string into an array by splitting it after a specific character (or characters).

The first argument, the `delimiter`, the character or characters to split by. As an optional second argument, you can stop splitting your string after a certain number of delimiter matches have been found.

```javascript
var text = 'Soda, turkey sandwiches, potato chips, chocolate chip cookies';
var menu = text.split( ', ' );
var limitedMenu = text.split( ', ', 2 );
```