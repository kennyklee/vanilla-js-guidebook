
# Forms

There are some helpful element properties that make working with forms in JavaScript a bit easier.

## forms

Get all forms on a page.

```javascript
var forms = document.forms;
```

### Browser Compatibility

Works in all modern browsers, and at least back to IE6.


## elements

Get all elements in a form.

```javascript
var form = document.querySelector( 'form' );
var elements = form.elements;
```

### Browser Compatibility

Works in all modern browsers, and at least back to IE6.


## Form Element Attributes

You can check the value of form element attributes by calling them directly on the element.

```javascript
var input = document.querySelector( '#input-1' );
var type = input.type;

var checkbox = document.querySelector( '#checkbox' );
var name = checkbox.name;

var radio = document.querySelector( '[name="radiogroup"]:checked' );
var value = radio.value;
```

You can also change and set attributes using the same approach.

```javascript
var input = document.querySelector( '#input-1' );
input.type = 'email';

var checkbox = document.querySelector( '#checkbox' );
checkbox.name = 'thisForm';

var textarea = document.querySelector( '#textarea' );
textarea.value = 'Hello, world!';
```

### Browser Compatibility

Works in all modern browsers, and at least back to IE6.


## Boolean Element Attributes

Certain attributes, like whether or not an input is `disabled`, `readonly`, or `checked`, use simple `true`/`false` boolean values.

```javascript
var input1 = document.querySelector( '#input-1' );
var isDisabled = input1.disabled;

var input2 = document.querySelector( '#input-2' );
input2.readOnly = false;
input2.required = true;

var checkbox = document.querySelector( '#checkbox' );
isChecked = checkbox.checked;

var radio = document.querySelector( '#radio-1' );
radio.checked = true;
```

### Browser Compatibility

Works in all modern browsers, and at least back to IE6.