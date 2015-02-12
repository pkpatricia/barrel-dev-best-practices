### Barrel Development Best Practices

CSS Best Practices
------------------
- [General](#general)
- [Animation](#animation-more)
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

### Animation ([more](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/))
* Use transitions for simple changes from one style to another. Use keyframe animations for more complex scripted actions.
* Modern browsers can animate four things really cheaply: position, scale, rotation and opacity. If you animate anything else, it’s at your own risk, and the chances are you’re not going to hit a silky smooth 60fps.
* Use the "Continuous Page Repainting" Tool in Chrome to monitor complex or large quantities of animated elements.
* Avoid overusing `translateZ(0)` or `translate3d(0,0,0)` in a single page thereby creating too many composite layers.
* In Blink and WebKit browsers a new layer is created for any element which has a CSS transition or animation on opacity. Therefore, excessive opacity transitions could result in the same issue of forcing a separate composite layer.
* For best performance, animate CSS transform properties instead of position, margin, height/width, etc. whenever possible.

### Preprocessing
A preprocessor can be used to simplify style sheet authoring by extending the basic CSS syntax. At Barrel we prefer to use Sassy (`*.scss`) for preprocessing. You may use less (`*.less`) or another flavor of sass (`*.sass`), but you must seek the approval from a Lead Developer and include this in your development approach when scoping a project. 

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
