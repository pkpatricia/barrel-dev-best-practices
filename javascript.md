#### Barrel Development Best Practices
# JavaScript Best Practices

## General Outline 
- Common JS modules via NPM
  - maintain a clean `package.json`
  - use Browserify to bundle modules
  - if a library is not on NPM, use Browserify Shim or similar to transform into a CJS module
  - declare all dependencies at the top of each file, via `require()`
- Build using Gulp
  - see Barrel Base for boilerplate
- Use vanilla javscript where possible
  - if using jQuery, include as a `browser` dependency in your `package.json` and include it via `var $ = require('jquery')` in your `main.js` (et al) file.
- At the time of this writing, ES5 is still generally preferred to ES6 + Babel
- Add `<script>` tags before closing `body` tag, unless they need to be loaded immediately.

## Styleguide
- Use semi-colons for consistency
  ```javascript
  function log(){
    // like this
    console.log(str);

    // not like this
    console.log(str)
  }
  ```
- Trailing commas + list un-assigned variables first
  ```javascript
  var unassigned, 
      assigned = 'This variable is assigned';
  ```
- When writing ternary, use parentheses to mark the left-hand side argument
  ```javascript
  var duration = (speed === 'fast') ? 200 : 800;
  ```

## Workflow
- Bundle using Browserify
- Minify using Uglify

## Things to Keep In Mind

#### You don't need to declare everything within `$.ready()`.
Only declare what initiates your application within the `.ready()`. Keep everything else scoped and modularlized as needed.

```javascript
function App(){
  ...
}

// Or $(document).ready();
jQuery(function($){
  new App();
});
```

#### Avoid unecessary `if ... else` statements.
While they are very explicit, they add unecessary visual clutter in large files. Use ternary for simple value checking. Where multiple levels are needed, `if ... else` is still preferable. 

## Tips & Tricks

#### Not everything needs to use the Constructor Pattern 
Class based (contructor based) javascript is great, and mimics the OOP principles of other languages. Use it if you need to generate many of something based on a base configuration, so that each of your returned objects has access to a shared pool of `prototype` methods.

But, using the Constructor Pattern isn't *technically* necessary if all you want to use it for is the ability to share properties between methods via the scoped value of `this`. For these instances, use of plain functions or Module Patterns (object literals) is perfectly fine, and usually more intuitive.

```javascript
// You don't need this...
function Menu(el){
  this.menu = document.querySelector(el);
  this.toggle = this.menu.querySelector('.js-menu'); 

  this.toggle.addEventListener('click', this.clicker().bind(this));
}
Menu.prototype.clicker = function(){
  this.menu.className += ' is-visible';
}

// ...when you can write it like this.
function Menu(el){
  var menu = document.querySelector(el),
      toggle = this.menu.querySelector('.js-menu'); 

  toggle.addEventListener('click', function(){
      
  })
}

















### Barrel Development Best Practices

# JavaScript Best Practices

- [General](#general)
- [jQuery](#jquery)

## General

*	**Encapsulate functions and variables as object methods and properties**

	For each website or application, encapsulate all global variables and functions as properties and methods of a single object literal. This improves ease of readability and maintenance, while also protecting variable scope and preventing global namespace pollution.
	
	```javascript
	var site = {
		myVariable: false,
		myMethod: function(){

		}
	};
	```
	
*	**Variable declaration**

	When declaring temporary variables that do not need to part of the parent object (i.e. something that is only ever referenced within a single function or method), declare all variables at the start of the function as a grouped comma-delineated list. This again improves readability and helps when keeping track of variable scope.
	
	```javascript
	...
	getRandom: function(){
		var min = 1,
			max = 10,
			rand = Math.random() * (max - min) + min,
			output = Math.floor(rand);
			
		return output;
	},
	...
	```
	
*	**Use ternary operation to declare conditional variables**

	To avoid unnecessary "if ... else" statements, make use of the ternary operation to declare variables whose value depends on a condition. The condition should be wrapped in parenthes.
	
	```javascript
	// The following statement:
	if(speed == 'fast'){
		var dur = 200;
	} else {
		var dur = 800;
	};
	// Can be re-written as:
	var dur = (speed == 'fast') ? 200 : 800;
	```
	
*	**Placement of JavaScript assets in HTML documents**

	Because the downloading of scripts blocks page rendering, the only JavaScript that should be loaded from the &lt;head&gt; of the document should contain time-sensitive functionality that must be evaluated before the document loads - for example image, video or font loading handlers, and feature detection. If possible, such functionality should be dependency-free and written in vanilla JS so that no other dependencies need to be loaded in the &lt;head&gt;.
		
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
	
## jQuery

*	**Using "Document Ready"**

	The document ready event should be bound only to what is required. Wrapping all javascript within a document ready function will harm performance as code will have to be evaluated and executed at the same time.
	
	The following pattern should be used instead:
	
	```javascript
	
	var site = {
		init: function(){
			// initialize 
		}
	};
	
	$(function(){
		site.init() // On doc ready, initalize script
	});
	
	```
	
	If conditional loading is required, the following pattern should be used to minimize the amount of code to be executed:
	
	```javascript
	
	var site = {
		global: {
			init: function(){
				...
			}
		},
		home: {
			init: function(){
				...
			}
		},
		blog: {
			init: function(){
				...
			}
		},	
		gallery: {
			init: function(){
				...
			}
		},
	};
	
	$(function(){
		site.global.init();
		
		if($('#Home').length){
			site.home.init();
		};
		
		if($('#Blog').length){
			site.blog.init();
		};
		
		if($('#Gallery').length){
			site.gallery.init();
		};
	});
	
	```

*	**Cache jQuery Objects**

	Instantiating jQuery objects is expensive and should be done sparingly. Whenever the jQuery object of a DOM node is required more than once, it should be cached as a variable to avoid having to call the jQuery method multiple times. 

	As per convention, any variable containing a jQuery object should be prefixed with the '$' symbol.
	
	```javascript
	var $slider = $('#Slider');
	
	$slider.addClass('loaded');
	```
*	**Reference DOM nodes by ID where possible**

	When instantiating a jQuery object on a single DOM node, use an ID as a selector rather than a class name. This will map directly to document.getElementById() and is therefore much quicker than traversing the entire document for all occurrences of a class name.
	
	Likewise, when instantiating a jQuery object on a group of DOM nodes, always descend from an ID using .find() to narrow the set of DOM nodes.
	
	```javascript
	
	var $hero = $('#Hero');
	
	...
	
	var $slides = $hero.find('.slide');
	
	
	```
	
*	**Use "vanilla" JS where possible**

	Using jQuery methods and objects during time-sensitive operations such as animation, loading, or AJAX requests, should be avoided if the same functionality can be easily achieved through vanilla javascript alone.
	
	If you are already referencing a single DOM node using a jQuery object, the DOM node object can be retrieved as follows:
	
	```javascript	
	var $hero = $('#Hero'); // create a jQuery object of DOM node with ID 'Hero' 
	
	...
	
	var hero = $hero[0]; // This returns the equivalent to document.getElementById('Hero');
	
	```
	**Some common examples**
	
	```javascript
	
	//jQuery
	
	$element.hide();
	
	$element.show();
	
	//Vanilla JS
	
	$element[0].style.display = 'none';
	
	$element[0].style.display = 'block';
	
	```
	
	```javascript
	
	//jQuery
	
	$elements.each(function(){
		var element = this, // DOM node
			$element = $(this); // jQuery Object
		
		...
	});
	
	//Vanilla JS
	
	for(var i = 0; i < $elements.length; i++){
		var element = $elements[i], // DOM node
			$element = $element.eq(i); // jQuery Object
		
		...
	};
	
	```
	
	```javascript
	
	//jQuery
	
	var $newSlide = $('<div class="slide"></div>');
	
	$hero.append($newSlide);
	
	//Vanilla JS
	
	var newSlide = document.createElement('div');
	
	newSlide.className = 'slide';
	
	$hero[0].appendChild(newSlide);
	
	```
	
	```javascript
	
	//jQuery
	
	var $link = $('#MyLink'),
		href = $link.attr('href');
	
	var $input = $('#MyInput'),
		value = $input.val();
		
	var $element = $('#MyElement'),
		customData = $element.attr('data-custom') // or .data('custom')
	
	
	//Vanilla JS
	
	var link = document.getElementById('MyLink'),
		href = link.href;
	
	var input = document.getElementById('MyInput'),
		value = input.value;
		
	var element = document.getElementById('MyElement'),
		customData = element.dataset.custom;
		
	```
