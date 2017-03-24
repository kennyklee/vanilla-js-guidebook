
# Loops

Loop through arrays, objects, and node lists.

## Arrays and Node Lists

In vanilla JavaScript, you can use `for` to loop through array and node list items.

```javascript
var sandwiches = [
	'tuna',
	'ham',
	'turkey',
	'pb&j'
];

for ( var i = 0; i < sandwiches.length; i++ ) {
	console.log(i) // index
	console.log( sandwiches[i] ) // value
}

// returns 0, tuna, 1, ham, 2, turkey, 3, pb&j
```

- In the first part of the loop, before the first semicolon, we set a counter variable (typically `i`, but it can be anything) to `0`.
- The second part, between the two semicolons, is the test we check against after each iteration of the loop. In this case, we want to make sure the counter value is less than the total number of items in our array. We do this by checking the `.length` of our array.
- Finally, after the second semicolon, we specify what to run after each loop. In this case, we're adding `1` to the value of `i` with `i++`.

We can then use `i` to grab the current item in the loop from our array.

### Multiple loops on a page

Variables you set in the `for` part of the loop are not scoped to the loop, so if you tried to include a second loop with `var i` you would get an error. You can use a different variable, or define `var i` outside of the loop and set it's value in the loop.

```javascript
for ( var n = 0; n < sandwiches.length; n++ ) {
	// Do stuff...
}

// Or...
var i;
for ( i = 0; i < sandwiches.length; i++ ) {
	// Do stuff...
}
```

### Skip and End

You can skip to the next item in a loop using `continue`, or end the loop altogether with `break`.

```javascript
for ( var n = 0; n < sandwiches.length; n++ ) {

	// Skip to the next in the loop
	if ( sandwiches[n] === 'ham' ) continue;

	// End the loop
	if ( sandwiches[n] === 'turkey' ) break;

	console.log( sandwiches[n] );

}
```

### Browser Compatibility

Supported in all modern browsers, and at least back to IE6.


## Objects

You can also use a `for` loop for objects, though the structure is just a little different. The first part, `key`, is a variable that gets assigned to the object key on each loop. The second part (in this case, `lunch`), is the object to loop over.

We also want to check that the property belongs to this object, and isn't inherited from further up the object chain (for nested or *deep* objects).

```javascript
var lunch = {
	sandwich: 'ham',
	snack: 'chips',
	drink: 'soda',
	desert: 'cookie',
	guests: 3,
	alcohol: false,
};

for ( var key in lunch ) {
	if ( Object.prototype.hasOwnProperty.call( lunch, key ) ) {
		console.log( key ); // key
		console.log( lunch[key] ); // value
	}
}

// returns sandwich, ham, snack, chips, drink, soda, desert, cookie, guests, 3, alcohol, false
```

### Browser Compatibility

Supported in all modern browsers, and IE6 and above.


## forEach Helper Method

If you use loops a lot, you may want to use Todd Motto's helpful `forEach()` method^[[https://github.com/toddmotto/foreach](https://github.com/toddmotto/foreach)].

It checks to see whether you've passed in an array or object and uses the correct `for` loop automatically. It also gives you a more jQuery-like syntax.

```javascript
/*! foreach.js v1.1.0 | (c) 2014 @toddmotto | https://github.com/toddmotto/foreach */
var forEach = function (collection, callback, scope) {
	if (Object.prototype.toString.call(collection) === '[object Object]') {
		for (var prop in collection) {
			if (Object.prototype.hasOwnProperty.call(collection, prop)) {
				callback.call(scope, collection[prop], prop, collection);
			}
		}
	} else {
		for (var i = 0, len = collection.length; i < len; i++) {
			callback.call(scope, collection[i], i, collection);
		}
	}
};

// Arrays
forEach(sandwiches, function (sandwich, index) {
	console.log( sandwich );
	console.log( index );
});

// Objects
forEach(lunch, function (item, key) {

	// Skips to the next item.
	// No way to terminate the loop
	if ( item === 'soda' ) return;

	console.log( item );
	console.log( key );
});
```

It's worth mentioning that because the helper uses a function, you can only skip to the next item using `return`. There's no way to terminate the loop entirely.

### Browser Compatibility

Supported in all modern browsers, and IE6 and above.