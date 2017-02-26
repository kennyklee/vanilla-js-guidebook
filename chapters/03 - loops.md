
# Loops

## jQuery

In jQuery, you can iterate of arrays, objects, and node lists with the `.each()` method.

```javascript
// Arrays
var sandwiches = [
	'tuna',
	'ham',
	'turkey',
	'pb&j'
];

$.each(sandwiches, function( index, sandwich ) {
	console.log( index ) // index
	console.log( sandwich ) // value
});


// Objects
var lunch = {
	sandwich: 'ham',
	snack: 'chips',
	drink: 'soda',
	desert: 'cookie',
	guests: 3,
	alcohol: false,
};

$.each(lunch, function( key, item ) {
	console.log( key ) // key
	console.log( item ) // item
});
```


## Arrays and Node Lists

In vanilla JavaScript, you can use `for` to loop through array and node list items.

- In the first part of the loop, before the first semicolon, we set a counter variable (typically `i`, but it can be anything) to `0`.
- The second part, between the two semicolons, is the test we check against after each iteration of the loop. In this case, we want to make sure the counter value is less than the total number of items in our array.
- Finally, after the second semicolon, we specify what to run after each loop. In this case, we're adding `1` to the value of `i`.

We can then use `i` to grab the current item in the loop from our array.

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
```

Any variables you set in the `for` part of the loop are global in scope, so if you tried to include a second loop with `var i` you would get an error. You can use a different variable, or define `var i` and set it's value in the loop.

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


## Objects

You can also use a `for` loop for objects, though the structure is just a little different. The first part, `item`, is a variable that gets assigned to the object key on each loop. The second part (in this case, `lunch`), is the object to loop over.

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

for ( var item in lunch ) {
	if ( Object.prototype.hasOwnProperty.call( lunch, item ) ) {
		console.log( item ); // key
		console.log( lunch[item] ); // value
	}
}
```


## forEach Helper Method

If you use loops a lot, you may want to use Todd Motto's helpful `forEach()` method.

It checks to see if you've passed in an array or object and uses the correct `for` loop automatically. It also gives you a more jQuery-like syntax.

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


## The Lab: Loops

For this lab:

1. Get the all of the `<divs>` that have a `[data-sandwich]` value of `ham`.
2. Loop through each one and log it in the console.