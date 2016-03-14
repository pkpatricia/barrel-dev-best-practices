#### Barrel Development Best Practices
# JavaScript Best Practices

### Goals and Questions You Should Ask Yourself
1. Maintainability 
  - is this going to make sense in 3 months?
  - what can I document now, to save time later?
  - how can I simplify this?
2. Readablity
  - *contributes to \#1*
  - are my teammates going to understand this?
  - what can I do to help my partner use this to their advantage?
3. Flexibility
  - can I use this on other projects?
  - can I use this elsewhere within your current project?
  - what if this needs to change later?
4. Speed
  - can I make this just as readable/maintainable in vanilla js?
  - could this have performance impacts down the line?
  - can I make this asynchronous?

* * *

## General Outline 
- Import modules via NPM
  - maintain a clean `package.json`
  - use Browserify to bundle modules
  - if a library is not on NPM, use Browserify Shim or similar to transform into a common-js module
  - declare all dependencies at the top of each file, via `require()`
- Build using Gulp
  - see Barrel Base for boilerplate
- Use vanilla javscript where possible
  - if using jQuery, include as a `browser` dependency in your `package.json` and include it via `var $ = require('jquery')` in your `main.js` (et al) file.
- Modules that require javascript should have their own JS partial, which are instantiated via our `data-module-init` pattern on page load.
  - see Barrel Base for examples
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
- Use named functions always, it helps with debugging
  ```javascript
  // this is an anonymous function
  var log = function(msg){
    console.log(msg);
  }

  // this is a named function
  function log(msg){
    console.log(msg);
  }
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
