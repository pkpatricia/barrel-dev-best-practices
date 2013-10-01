### Barrel Development Best Practices

JavaScript Best Practices
-------------------------

- [General](#general)
- [jQuery](#jquery)

### General

*	For each website or application, encapsulate all variables and functions as properties and methods of a global object literal. This improves ease of readability and maintenance, while also protecting variable scope and preventing global namespace pollution.

	**Code Example**
	
	```javascript
	var site = {
		myVariable: false,
		myMethod: function(){
			alert('hello world')
		}
	};
	```
	
*	When working inside methods of the global object literal, the keyword *this* can be used to reference the parent object. This should be cached as a variable to maintain its relationship to the parent object when the scope of *this* changes. As per convention, it should be named *self* or *that*.

	**Code Example**
	
	```javascript
	var site = {
		button: document.getElementById('Button'),
		buttonClicked: false,
		bindHandlers: function(){
			var self = this;
			
			self.button.onclick = function(){
				self.buttonClicked = true;
				
				// inside the function, "this" refers to the button and not the parent object, so we must refer to "self" instead.
			}
		}
	};
	
	document.addEventListener('DOMContentLoaded', function(){
		site.bindHandlers();
	};
	```
	
### jQuery

*	Whenever a jQuery object of a DOM element is used more than once, it should be cached as a variable to avoid having to call the jQuery method multiple times.

	**Code Example**
	
	```javascript
	var $slider = $('#Slider');
	
	$slider.addClass('loaded');
	```