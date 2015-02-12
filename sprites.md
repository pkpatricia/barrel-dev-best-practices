# Image Spriting
### Dope af image assets for dope af people.

This walkthrough is assuming that you're using SVGs for your sprites, and only use PNGs as fallbacks for < IE8.

## Reqs
 - Grunt
 - [svgstore](https://github.com/FWeinb/grunt-svgstore)
  - handles SVG spriting needs
 - [spritesmith](https://github.com/Ensighten/grunt-spritesmith)
  - handles PNG spriting needs

## Setup
Create directories for you individual SVGs (and PNG, if you're into that sort of thing). Make sure when you're exporting to these folders you use consistent nomenclature, it makes using the icons in projects easier.

**My Gruntfile.js**
``` javascript
concat: {
      build: {
        src: ['js/inc/*.js'],
        dest: 'js/build.js',
      },
      deploy: {
        src: ['js/*.js'],
        dest: 'scripts.js',
      }
    },

    sass: {
      options: {
        style: 'normal'
      },
      dist: {
        files: {
          'scss/build.css': 'scss/style.scss'
        }
      }
    },

    autoprefixer: {
      options: {
        browsers: ['last 3 versions', 'ie 8', 'ie 9']
      },
      dist: {
        files: {
          'style.css': 'scss/build.css'
        }
      }
    },

    svgstore: {
      options: {
        prefix : '', // This will prefix each ID
        inheritviewbox: true,
        svg: { // will add and overide the the default xmlns="http://www.w3.org/2000/svg" attribute to the resulting SVG
          viewBox : '0 0 100 100',
          xmlns: 'http://www.w3.org/2000/svg',
          style: 'display:none'
        }
      },
      default: {
        files: {
          'assets/images/svg_sprite.svg' : ['assets/images/svg/*.svg'],
        }
      }
    },

    sprite:{
      all: {
        src: 'assets/images/png/*.png',
        dest: 'assets/images/png_sprite.png',
        destCss: 'scss/base/_svg-sprite.scss',
        cssFormat: 'scss'
      }
    },

    watch: {
      css: {
        files: 'scss/**/*.scss',
        tasks: ['sass', 'autoprefixer']
      },
      js: {
        files: ['js/*.js', 'js/**/*.js', '!js/build.js'],
        tasks: ['concat']
      },
      svg: {
        files: 'assets/images/svg/*.svg',
        tasks: 'svgstore'
      },
      png: {
        files: 'assets/images/png/*.png',
        tasks: 'sprite'
      }
    }
```

So the important part is this bit:
``` javascript
svgstore: {
      options: {
        prefix : '', // prefix each icon however you would like
        inheritviewbox: true,
        svg: { // add whatever attributes you need to your SVG sprite
          viewBox : '0 0 100 100',
          xmlns: 'http://www.w3.org/2000/svg',
          style: 'display:none' // just hides the sprite until you use it
        }
      },
      default: {
        files: {
          'assets/images/svg_sprite.svg' : ['assets/images/svg/*.svg'],
        }
      }
    },

    sprite:{
      all: {
        src: 'assets/images/png/*.png',
        dest: 'assets/images/png_sprite.png',
        destCss: 'scss/base/_svg-sprite.scss', // by default the plugin will infer an output of SCSS
        cssFormat: 'scss' // output format, if you need to specify, can be almost anything
      }
    },
```
## SVG Spriting
Svgstore is takes your SVG asses and creates an inline SVG, with each individual file becoming a `<symbol>` element with an `id="#idHere"` within the main object. You can prefix that # with whatever you want, but I'm not doing it here because I was naming my artboards directly in Illustrator so that they output icon-*.svg. Viewbox is standardizing the frame of reference, but each `<symbol>` will have it's own. Declaring `display:none` just hides the entire object, because we don't need it to show where we import it, we want to later reference the data within it.

My rendered SVG looks like this:
```
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg" style="display:none"><symbol viewBox="0 0 19.3 37.1" id="icon_facebook"><title>icon_facebook</title> <path d="M19.3,6.1h-3.5c-2.8,0-3.3,1.3-3.3,3.2v4.2h6.6l-0.9,6.6h-5.7v17H5.7v-17H0v-6.6h5.7V8.6c0-5.7,3.5-8.8,8.5-8.8
	c2.4,0,4.5,0.2,5.1,0.3V6.1z"/> </symbol><symbol viewBox="0 0 37.1 37.1" id="icon_instagram"><title>icon_instagram</title> <path style="fill:#FF1F3C;" d="M37.1,32.4c0,2.6-2.2,4.8-4.8,4.8H4.8C2.2,37.1,0,35,0,32.4V4.8C0,2.2,2.2,0,4.8,0h27.6
	c2.6,0,4.8,2.2,4.8,4.8V32.4z M32.9,15.7h-3.3c0.3,1,0.5,2.1,0.5,3.2c0,6.2-5.2,11.2-11.6,11.2c-6.4,0-11.5-5-11.5-11.2
	c0-1.1,0.2-2.2,0.5-3.2H4.1v15.7c0,0.8,0.7,1.5,1.5,1.5h25.9c0.8,0,1.5-0.7,1.5-1.5V15.7z M18.6,11.3c-4.1,0-7.4,3.2-7.4,7.2
	s3.3,7.2,7.4,7.2c4.1,0,7.5-3.2,7.5-7.2S22.7,11.3,18.6,11.3z M32.9,5.8c0-0.9-0.8-1.7-1.7-1.7h-4.2c-0.9,0-1.7,0.7-1.7,1.7v4
	c0,0.9,0.8,1.7,1.7,1.7h4.2c0.9,0,1.7-0.8,1.7-1.7V5.8z"/> </symbol><symbol viewBox="0 0 45.6 37.1" id="icon_twitter"><title>icon_twitter</title> <path d="M40.9,9.2c0,0.4,0,0.8,0,1.2c0,12.3-9.4,26.6-26.6,26.6C9,37,4.1,35.5,0,32.8c0.8,0.1,1.5,0.1,2.3,0.1c4.4,0,8.4-1.5,11.6-4
	c-4.1-0.1-7.5-2.8-8.7-6.5c0.6,0.1,1.2,0.1,1.8,0.1c0.8,0,1.7-0.1,2.5-0.3c-4.3-0.9-7.5-4.6-7.5-9.2c0,0,0-0.1,0-0.1
	c1.2,0.7,2.7,1.1,4.2,1.2c-2.5-1.7-4.2-4.5-4.2-7.8c0-1.7,0.5-3.3,1.3-4.7c4.6,5.7,11.5,9.4,19.3,9.8c-0.1-0.7-0.2-1.4-0.2-2.1
	c0-5.1,4.2-9.3,9.3-9.3c2.7,0,5.1,1.1,6.8,2.9c2.1-0.4,4.1-1.2,5.9-2.3c-0.7,2.2-2.2,4-4.1,5.1c1.9-0.2,3.7-0.7,5.4-1.4
	C44.3,6.2,42.7,7.9,40.9,9.2z"/> </symbol></svg>
```
Ain't she a 'beaut. Note that the outer `<svg>` is `display:none` and that each `<sybmol>` has an ID associated with it.

We need to include this file at the top of our document so that we can reference the `<symbols>` to make our icons later on.
``` html
<!-- load sprite -->
<?php include_once("assets/images/svg_sprite.svg"); ?>
```

You reference each icon by first using the SVG tag and matching it's frame of reference to the one you declared in your Gruntfile.
``` html
<svg viewBox="0 0 100 100">

</svg>
```
Then, we need to reference the specific symbol.
``` html
<svg viewBox="0 0 100 100">
  <use xlink:href="#icon_twitter"></use>
</svg>
```
By default **this will fill to fit its container**, except for in IE where it's contrained to 100px/100px by the viewBox declariong. Idk why yet. At any rate, you can just add a class to the `<svg>` tag wherever you use them that will style the height/width of the contained SVG file.
``` html
<svg viewBox="0 0 100 100" class="icon svg">
  <use xlink:href="#icon_twitter"></use>
</svg>
```

## PNG Spriting (the fallback to SVG)

Max has more experience with Spritesmith, but what's it's doing is creating a sprite *as well as* a stylesheet, which is neat. I have it exporting SCSS, which makes these lovely mixins which make creating a class for an icon sooper easy and exposes some cool variables like each icon's width and height.
``` css
.icon-twitter-png {
  @include sprite($icon-twitter);
}
.icon-twitter-png {
  @include sprite-2x($icon-twitter);
  /* this mixin doesn't come standard, but you could easily make one
  for those users who like to run IE8 on a retina display and
  watch the world burn. */
}
```

So we only need these PNGs < IE8 (and if someone is better with Javascript and IE8 compatibility, we could kill the HTTP request here too), so I'm hiding the elements with `display:none`. I'm leaving the SVGs right now (1) because I couldn't figure out how to hide them < IE8 (we'll need to remove the include tag/HTTP request too) and (2) because the elements don't show up anyway and therefore don't take up any space. It's a temporary fix, it would be great to figure this out.

So I'm doing this with a little script, and packaging it and the `<?php include_once("assets/images/svg_sprite.svg"); ?>` from above into one php file that I can just include after my opening `<body>` tag in my header template. That way it's out of my way and it doesn't look ugly.
```html
<!-- load sprite -->
<?php include_once("assets/images/svg_sprite.svg"); ?>

<!-- check for SVG support and show PNGs if none -->
<script type="text/javascript">
window.onload = function(){
    if(!document.createElement('svg').getAttributeNS){
        var pngs = document.querySelectorAll('.png');

        for (var i = 0; i < pngs.length; i ++) {
            pngs[i].style.display = 'block';
        }
    }
}
</script>
```

## Doing It
So the total package is this:
``` html
<div class="icon-cont">
  <svg viewBox="0 0 100 100" class="icon svg">
    <use xlink:href="#icon_facebook"></use>
  </svg>
  <!--This element is 'display:none' using the '.icon.png' selector-->
  <i class="icon-facebook-png icon png"></i>
</div>
```
I have a (hardly a demo) [demo](http://sandbox.ericbailey.co) for your viewing pleasure. Look at it on IE8, the SVGs are gone and the PNGs replace them. Vice versa on every other browser. Those with a keen eye will notice that the sizing is inconsitent between the SVG version and the PNG version. I know this can be fixed with icon-specific CSS styles, which is meh, but I'd like to experiment with some other options built into the to spriting plugins to see if this can be minimized.

## Next Steps
 - find a good way to keep consistent scaling between PNG and SVG icons
 - properly manage the showing/hiding of each asset
 - kill the HTTP requests not needed
 - create Moustache templates that contain each icon (SVG and PNG) so as not to pollute the code
  - these could potentially control other attributes, like **sizing**.
 - rewrite this entire document moar better