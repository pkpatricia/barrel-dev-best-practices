# Overall Goals
1. high maintainability
2. strict syntax
  - allows for linting
  - self documenting
3. fast 
4. well commented

> "Obviousness comes from conforming to people’s existing mental models." - [Julie Zhou](https://medium.com/the-year-of-the-looking-glass/good-design-a89c15136ba6#.9h8jfoyr5)

Read: *If it makes sense to you, but doesn't conform to someone else's mental model, it doesn't make sense to them.*
Conversly: *If you think your way is better, you'll need to convert someone's entire way of thinking about it in order to convince them.*

## TOC
1. [Naming](#naming)

### Structure
TODO

Where to put variables?

###SASS
Use `.scss`, but don't be afraid to use `.css`, \*gasp\*.

### Naming
Barrel uses [BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/). The accronym stands for classifications of selectors: block, elements, and modifiers. You already use these, but may not have thought about them as distinct. BEM just aims to codify them into a more readable, transparent convention.

####B is for Block
Blocks are the root element of your module or component. Elements that make up that component are then scoped to the root using BEM syntax.

Examples of blocks:
```css
.hero {
  ... 
}
.page-header {
  ...  
}
```

**Do:**
- keep the block selector short, and no more than two words

**Don't:**
- use IDs, shouldn't need them


####E is for Element
Elements are children of Blocks. They are usually the smallest parts of a Block scoped piece of DOM/CSS, and are preferential to selecting elements by HTML tag. Be careful not to confuse elements with other Blocks. BEM selectors should usually never be "deeper" than two levels i.e. a block followed by an element.

Examples of elements:
```css
.hero__bg-overlay {
  ...  
}
.page-header__category {
  ...  
}

/* you probably don't need to do this */
.hero__background__overlay {
  ...  
}
```

**Do:**
- use CSS selectors instead of defining classes based on HTML elements
- try to keep selectors to only two "levels" deep
  - however, the syntax allows for it, so use with discretion, they get annoying to type out

####M is for Modifier
Modifiers are the variants of a Block or Element within a Block. They can change any attribute on the selector they're applied to, but use caution when using Modifiers as a descender class. It can get tricky depending on where the definitions are located within your partial structure. Keep all modifiers of a class within it's parent definition.

Examples of modifiers:
```css
.hero__bg-overlay--white {
  ...  
}
.page-header__category--underline {
  ...  
}
```

Modifiers can be tricky as selectors, especially when the Modifier class is applied in a different partial than where it was defined. In the example below, if the dev were to change the class from `.hero__background--light` to `.hero__background--white`, the usage within `buttons.scss` would break, and it may not be immediately obvious why.
```css
/* hero.scss */
.hero__background--light {
  ...
  .hero__title {
     color: white; 
  }
}

/* buttons.scss */
.button {
  ...
  .hero__background--light & {
    background-color: white;  
  }
}

/* this is preferable, in this case */
.hero__background--light {
  .button {
    background-color: white;  
  }  
}
```

###Nesting
Nesting should be kept to a minimum. "As much as is necessary, but as litle as possible" is a good rule of thumb. It creates (oftentimes) unecessary specificity, which leads to the need to override styles. 

For example:
```html
<!-- hero.hbs -->
<div class="hero">
  <button class="button"></button>
</div>

<!-- contact.hbs -->
<div class="hero">
  <button class="download-button button"></button>
</div>
```
```css
/* hero.scss */
.hero {
  .button {
    margin-top: 10px;  
  }  
}

/* contact.scss */
.download-button {
  margin-top: 0; // won't work  
}
```

The problem is exacerbated when you don't follow a syntax like BEM. For example:
```css
/* about.scss */
.about-page {
  .image {
    ...implementation specific styles here...  
  }
}

/* hero.scss */
.hero {
  .image {
    ...implementation specific styles here...  
  }
}
```
In the above, if `.hero` lives within `.about-page`, `.hero .image` will inherit the same styles as those from `.about-page`. This *compounds* the properties, and makes it harder to maintain. A better alternative would be to use a Modifier class (along with BEM, of course), or separate the *base* styles from the *implementation specific* and group them into a class, which can be applied to both elements.

###Mixins & Extends
Both are powerful, but can be very expensive. Ground rules:
- never extend a class within a nested selector structure
- never extend a class that contains another `@extend`
- never extend a class that contains *implementation specific* properties
- never include a mixin that contains its own selectors
- be careful when you include a mixin that contains *implementation specific* properties

**In general:**
Mixins duplicate code, so be careful what you're duplicating. Extends *hoist* selectors, so pay attention how long you selectors get, and understand what causes them.

###Stateful Classes
Introduced by SMACSS, stateful classes tell a developer that a component is in a current *state*, or that a script is acting upon the component. Stateful classes *are not global*, meaning they do not carry styles on their own. Join stateful classes to an existing selector using "active" prefixes `.is-`, `.has-`, `.was-`, etc. 

```css
.info__tab {
  color: gray;
  &.is-active {
    color: white; 
  }
}
```

###Javascript Hooks
Never select an element in javascript by a standard class, especially one that has properties defined on it. Always use `.js-` prefixes so that other developers know what classes are being used in CSS, and which are used in JS.

###Breakpoints
Breakpoints should be kept in their own partial for ease of use. They should be defined based on the content, not devices, and you [should use EMs](http://blog.cloudfour.com/the-ems-have-it-proportional-media-queries-ftw/) where possible. With SASS it's easy to convert [pixels to EMs.](https://github.com/estrattonbailey/svbstrate/blob/master/src/base/_breakpoints.scss)

### Animation ([more](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/))
* Use transitions for simple changes from one style to another. Use keyframe animations for more complex scripted actions.
* Modern browsers can animate four things really cheaply: position, scale, rotation and opacity. If you animate anything else, it’s at your own risk, and the chances are you’re not going to hit a silky smooth 60fps.
* Use the "Continuous Page Repainting" Tool in Chrome to monitor complex or large quantities of animated elements.
* Avoid overusing `translateZ(0)` or `translate3d(0,0,0)` in a single page thereby creating too many composite layers.
* In Blink and WebKit browsers a new layer is created for any element which has a CSS transition or animation on opacity. Therefore, excessive opacity transitions could result in the same issue of forcing a separate composite layer.
* For best performance, animate CSS transform properties instead of position, margin, height/width, etc. whenever possible.

###Misc
- avoid `*` selectors, they're slow and can be dangerous if applied incorrectly
- Instead of relying on `!important`, use CSS specificity to override styles when necessary.

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
