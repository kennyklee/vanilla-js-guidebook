
# Merging Arrays and Objects

## Add items to an array

Use `push()` to add items to an array.

```javascript
var sandwiches = ['turkey', 'tuna', 'blt'];
sandwiches.push('chicken', 'pb&j');
// sandwiches: ['turkey', 'tuna', 'blt', 'chicken', 'pb&j']
```

### Browser Compatibility

Works in all modern browsers, and IE6 and above.


## Merge two or more arrays together

Use `Array.prototype.push.apply()` to merge two or more arrays together. Merges all subsequent arrays into the first.

```javascript
var sandwiches1 = ['turkey', 'tuna', 'blt'];
var sandwiches2 = ['chicken', 'pb&j'];
Array.prototype.push.apply(sandwiches1, sandwiches2);
// sandwiches1: ['turkey', 'tuna', 'blt', 'chicken', 'pb&j']
// sandwiches2: ['chicken', 'pb&j']
```

### Browser Compatibility

Works in all modern browsers, and at least IE6.


## Add items to an object

Use the dot notation (`obj.something`) or bracket notation (`obj['something']`) to add key/value pairs to an object.

```javascript
var lunch = {
    sandwich: 'turkey',
    chips: 'cape cod',
    drink: 'soda'
}

// Add items to the object
lunch.alcohol = false;
lunch["dessert"] = 'cookies';

// return: {sandwich: "turkey", chips: "cape cod", drink: "soda", alcohol: false, dessert: "cookies"}
```

### Browser Compatibility

Works in all modern browsers, and at least IE6.


## Merge two or more objects together

`extend` is a helper method I wrote to merge two or more objects together. It works a lot like jQuery's `.extend()` function, except that it returns a new object, preserving all of the original objects and their properties. For deep (or recursive) merges, pass in `true` as the first argument. Otherwise, just pass in your objects.

```javascript
/**
 * Merge two or more objects. Returns a new object.
 * Set the first argument to `true` for a deep or recursive merge
 * @param {Boolean}  deep     If true, do a deep (or recursive) merge [optional]
 * @param {Object}   objects  The objects to merge together
 * @returns {Object}          Merged values of defaults and options
 */
var extend = function () {

    // Variables
    var extended = {};
    var deep = false;
    var i = 0;
    var length = arguments.length;

    // Check if a deep merge
    if ( Object.prototype.toString.call( arguments[0] ) === '[object Boolean]' ) {
        deep = arguments[0];
        i++;
    }

    // Merge the object into the extended object
    var merge = function ( obj ) {
        for ( var prop in obj ) {
            if ( Object.prototype.hasOwnProperty.call( obj, prop ) ) {
                // If deep merge and property is an object, merge properties
                if ( deep && Object.prototype.toString.call(obj[prop]) === '[object Object]' ) {
                    extended[prop] = extend( true, extended[prop], obj[prop] );
                } else {
                    extended[prop] = obj[prop];
                }
            }
        }
    };

    // Loop through each object and conduct a merge
    for ( ; i < length; i++ ) {
        var obj = arguments[i];
        merge(obj);
    }

    return extended;

};

// Example objects
var object1 = {
    apple: 0,
    banana: { weight: 52, price: 100 },
    cherry: 97
};
var object2 = {
    banana: { price: 200 },
    durian: 100
};
var object3 = {
    apple: 'yum',
    pie: 3.214,
    applePie: true
}

// Create a new object by combining two or more objects
var newObjectShallow = extend( object1, object2, object3 );
var newObjectDeep = extend( true, object1, object2, object3 );
```

### Browser Compatibility

Works in all modern browsers, and at least IE6.