### Barrel Development Best Practices

JavaScript Best Practices
-------------------------

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