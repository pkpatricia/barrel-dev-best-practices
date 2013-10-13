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
	
*	**Placement of JavaScript assets in HTML documents**

	Because the downloading of scripts blocks page rendering, the only JavaScript that should be loaded from the &lt;head&gt; of the document should contain time-sensitive functionality that must be evaluated before the document loads - for example image, video or font loading handlers. If possible, such functionality should be dependency-free and written in vanilla JS so that no other dependencies need to be loaded in the &lt;head&gt;.
		
	All other JavaScript including dependencies, libraries and especially anything that is not executed until the "document ready" event, should be loaded from just before the closing &lt;/body&gt; tag of the document. Large libraries such as jQuery should generally be loaded from their own CDN if possible to distribute HTTP requests.
	
	To reduce HTTP requests, all javascript should be concatenated into as few files as possible. While a single file is preferable, maintainability of the project may be improved by utilizing 2 or 3 semantically named files.
	
	For example:
	
	```HTML
	<head>
		...
		<script src="js/load.js"></script> <!-- if loading scripts are necessary -->
	</head>
	<body>
		...
		<script src="http://code.jquery.com/jquery-1.10.1.min.js"></script> <!-- large libraries should be loaded from their own CDN -->
		<script src="js/dependencies.js"></script> <!-- smaller dependencies such as plugins should be concatenated into a single file -->
		<script src="js/main.js"></script> <!-- custom project-specific JS -->
	</body>
	```
	
### jQuery

*	**Cache jQuery Objects**

	Instantiating jQuery objects is expensive and should be done sparingly. Whenever the jQuery object of a DOM node is required more than once, it should therefore be cached as a variable to avoid having to call the jQuery method multiple times. 

	As per convention, any variable containing a jQuery object should be prefixed with the '$' symbol.
	
	```javascript
	var $slider = $('#Slider');
	
	$slider.addClass('loaded');
	```
*	**Reference DOM nodes by ID where possible**

	When instantiating a jQuery object on a single DOM node, use an ID as a selector rather than a class name. This will map directly to document.getElementById() and is therefore much quicker than traversing the entire document for all occurrences of a class name.
	
	Likewise, when instantiating a jQuery object on a group of DOM nodes, always descend from an ID using .find() if possible to narrow the set of DOM nodes.
	
	```javascript
	
	var $hero = $('#Hero');
	
	...
	
	var $slides = $hero.find('.slide');
	
	
	```
	
*	**Use "vanilla" JS where possible**

	Using jQuery methods and objects during time-sensitive operations such as animation, loading, or AJAX requests, should be avoided if the same functionality can be easily achieved through vanilla javascript alone.
	
	**Some common examples**
	
	If you are already referencing a single DOM node using a jQuery object, the DOM node object can be retrieved as follows:
	
	```javascript	
	var $hero = $('#Hero); // create a jQuery object of DOM node with ID 'Hero' 
	
	...
	
	var hero = $hero[0]; // This returns the equivalent to document.getElementById('Hero');
	
	hero.style.display = 'none'; // We can now perform javascript operations on the object.
	
	```
