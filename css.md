### Barrel Development Best Practices

CSS Best Practices
------------------
- [General](#general)
- [Animation](#animation)
- [Preprocessing](#preprocessing)

### General

* Use hyphens for class names, and ProudCamelCase for element IDs.

    ```html
    <section id="IntroSection" class="bg-blue section-fixed"></section>
    ```
    
* Use classes defined in CSS to change appearance whenever possible. Only set styles in JavaScript if the values need to be dynamic.

* Avoid unnecessary use of `*` and other expensive CSS selectors.

* Instead of relying on `!important`, use CSS specificity to override styles when necessary.

  *Don't:*
  ```
  #element {
    color: black;
  }
  .blue {
    color: blue !important;
  }
  ```
  
  *Do:*
  ```
  #element {
    color: black;
  }
  #element.blue {
    color: blue;
  }
  ```

### Animation
* Use transitions for simple changes from one style to another. Use keyframe animations for more complex scripted actions.

* For best performance, animate CSS transform properties instead of position, margin, height/width, etc. whenever possible.

### Preprocessing
A preprocessor can be used to simplify style sheet authoring by extending the basic CSS syntax.
* Organize CSS into separate files that group related styles.

* Define commonly used values as variables.

* The primary stylesheet should include minimal styles; mostly imports of variables, mixins, and partial stylesheets.

* Avoid unnecessarily long selectors by keeping nesting at 1 or 2 levels.

  *Don't:*
  ```
  html {
    body {
      color: white;
      background: blue;

      #element {
        background: white;
        color: black;
        
        p {
          font-size: 2em;
        }
      }
    }
  }
  ```
  
  *Do:*
  ```
  body {
    color: white;
    background: blue;
  }
  
  #element {
    background: white;
    color: black;
      
    p {
      font-size: 2em;
    }
  }
  ```
