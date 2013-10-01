### Barrel Development Best Practices

JavaScript Best Practices
-------------------------

Last edited by **Patrick Kunka**  

*	For each website or application, encapsulate all variables and functions as properties and methods of a global object. This improves ease of readability and maintenance and protects variable scope.

	##### Code Example
	
		var site = {
			myVariable: false,
			myMethod: function(){
				var self = this;
			}
		};