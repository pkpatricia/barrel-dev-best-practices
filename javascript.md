### Barrel Development Best Practices

JavaScript Best Practices
-------------------------

*	For each website or application, encapsulate all variables and functions as properties and methods of a global object. This improves ease of readability and maintenance and protects variable scope, and prevents global namespace pollution.

	##### Code Example
	
		var site = {
			myVariable: false,
			myMethod: function(){
				var self = this;
			}
		};
		
- - -

Last edited by **Patrick Kunka**