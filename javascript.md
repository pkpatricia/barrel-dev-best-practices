### Barrel Development Best Practices

JavaScript Best Practices
-------------------------

- [General](#general)
- [jQuery](#jquery)

### General

*	For each website or application, encapsulate all variables and functions as properties and methods of a global object. This improves ease of readability and maintenance, while also protecting variable scope and preventing global namespace pollution.

	**Code Example**
	
	```javascript
	var site = {
		myVariable: false,
		myMethod: function(){
			var self = this;
		}
	};
	```
	
### jQuery

*	Whenever a jQuery object of a DOM element is used more than once, it should be cached as a variable to avoid having to call the jQuery method multiple times.

	**Code Example**
	
	```javascript
	var $slider = $('#Slider');
	
	$slider.addClass('loaded');
	```