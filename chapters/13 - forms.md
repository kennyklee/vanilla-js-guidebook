
# Forms

There are some helpful native JavaScript and browser APIs that make working with forms a bit easier.

## forms

Get all forms on a page.

```javascript
var forms = document.forms;
```


## elements

Get all elements in a form.

```javascript
var form = document.querySelector( 'form' );
var elements = form.elements;
```


## Form Element Attributes

You can check the value of form element attributes by calling them directly on the element.

```javascript
var input = document.querySelector( '#a' );
var type = input.type;

var checkbox = document.querySelector( '#c' );
var name = checkbox.name;

var radio = document.querySelector( '[name="radiogroup"]:checked' );
var value = radio.value;
```

You can also change and set attributes using the same approach.

```javascript
var input = document.querySelector( '#a' );
input.type = 'email';

var checkbox = document.querySelector( '#c' );
checkbox.name = 'thisForm';

var textarea = document.querySelector( '#e' );
textarea.value = 'Hello, world!';
```


## Boolean Element Attributes

Certain attributes, like whether or not an input is `disabled`, `readonly`, or `checked`, use simple `true`/`false` boolean values.

```javascript
var input1 = document.querySelector( '#a' );
var isDisabled = input1.disabled;

var input2 = document.querySelector( '#b' );
input2.readOnly = false;
input2.required = true;

var checkbox = document.querySelector( '#c' );
isChecked = checkbox.checked;

var radio = document.querySelector( '#d1' );
radio.checked = true;
```


## The Lab: Forms

For this lab:

1. Get all form elements.
2. If the form is a text input, change the type to `password`.
3. if the form is a checkbox, check it.
4. If it's a radio button, check if it's the selected option. If it is, log the value in the console.
5. If it's a textarea, give it a value.