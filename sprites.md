# Image Spriting
### Dope af image assets for dope af people.

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


