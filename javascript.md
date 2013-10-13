### Barrel Development Best Practices

JavaScript Best Practices
-------------------------

- [General](#general)
- [jQuery](#jquery)

### General

*	**Encapsulate functions and variables as object methods and properties**

	For each website or application, encapsulate all global variables and functions as properties and methods of a single object literal. This improves ease of readability and maintenance, while also protecting variable scope and preventing global namespace pollution.
	
	Code written in this pattern lends itself to modularity and extensibility and is therefore especially useful for large applications.
	
	```javascript
	var site = {
		myVariable: false,
		myMethod: function(){
			var self = this;
		}
	};
	```
	
*	**Variable declaration**

	When declaring temporary variables that do not need to part of the parent object (i.e. something that is only ever referenced within a single function or method), declare all variables at the start of the function as a grouped comma-delineated list. This again improves readability and helps us to keep track of variable scope.
	
	```javascript
	...
	getRandom: function(){
		var self = this,
			min = 1,
			max = 10,
			rand = Math.random() * (max - min) + min,
			output = Math.floor(rand);
			
		return output;
	},
	...
	```
	
*	**Use ternary operation to declare conditional variables**

	To avoid unnecessary "if ... else" statements, make use of the ternary operation to declare variables whose value depends on a condition.
	
	```javascript
	// The following statement:
	if(speed == 'fast'){
		var dur = 200;
	} else {
		var dur = 800;
	};
	// Can be re-written as:
	var dur = speed == 'fast' ? 200 : 800;
	```
	
### jQuery

*	**Cache jQuery Objects**

	Creating jQuery objects is expensive and should be done sparingly. Whenever the jQuery object of a DOM node is required more than once, it should therefore be cached as a variable to avoid having to call the jQuery method multiple times. 

	As per convention, any variable containing a jQuery object should be prefixed with the '$' symbol.
	
	```javascript
	var $slider = $('#Slider');
	
	$slider.addClass('loaded');
	```