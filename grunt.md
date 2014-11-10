# Grunt & Optimization

Grunt is a tool for automating development tasks. You run it from the command line in terminal, using the Node.js engine and the Node Package Manager. It can be configured in a variety of ways to suit different types of projects. This document includes some recommended Grunt packages and sample configuration for common tasks. 

*While there are new task runners such as [gulp](https://github.com/gulpjs/gulp), you should seek approval from a Lead Developer before embarking on this journey since many of our projects continue to be developed either by third parties or new developers who likely will not be familiar with an entirely different workflow.*

- [Example Gruntfile](http://gruntjs.com/getting-started#an-example-gruntfile)

## File structure
Generally you will be working with source files which are being compiled into distribution-ready assets by your Grunt workflow. It’s common to store source files in a directory called “src” or “app”, and to output the finalized site files to a “dist” directory.

## Basic config
You can set up your project by running `npm init` or manually creating a [package.json](http://gruntjs.com/getting-started#package.json) file in your project’s root directory. This file keeps a manifest of the dependencies required to compile your project. To get started with Grunt you’ll need to first install grunt-cli globally (`sudo npm install grunt-cli`), then install the latest version of Grunt for your current project (`npm install grunt --save-dev`).

### Tasks
For the sake of consistency, it will be a Barrel Best Practice to create two tasks, one for building the theme without any server or watch-related functionality and another to do any such tasks requiring the use of a server. These tasks should be called `build` and `server` respectively. Additionally a task called `default` should be created as an alias to run the `build` task. You may include as many additional tasks as needed for your project.

## Packages

### [grunt-usemin](https://github.com/yeoman/grunt-usemin) & [grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify)
Uglify concatenates and minifies JavaScript files. If you're developing a static site, you can use grunt-usemin to configure this process using special comments when you insert the scripts in your HTML. There are two separate tasks in Grunt: useminPrepare, for scanning your HTML files and passing the configuration to Uglify; and usemin, for replacing the references to the source files with the minified paths. For some projects (such as Wordpress sites) you may need to define the JS source files directly in the Uglify config block.

In HTML:

    <!-- build:js dist/scripts/main.min.js -->
    <script src="src/scripts/lib/underscore.js"></script>
    <script src="src/scripts/lib/backbone.js"></script>
    <script src="src/scripts/plugins/mixitup.js"></script>
    <script src="src/scripts/main.js"></script>
    <!-- endbuild -->

In Gruntfile.js:

    useminPrepare: {
      html: 'src/index.html',
      options: {
        flow: {
          steps: { 'js': ['uglifyjs'] },
          post: {}
        },
        dest: 'dist'
      }
    },
    usemin: {
      html: 'dist/**/*.html',
      options: {
        assetDirs: ['dist/assets']
      }
    },
    uglify: {
      options: {
        sourceMap: 'dist/scripts/main.js.min.map',
        sourceMappingURL: 'main.js.min.map'
      }
    }

### [grunt-contrib-less](https://github.com/gruntjs/grunt-contrib-less)
Task for compiling LESS files into CSS. Can also include Source Maps for easier inspection.

    less: {
      dev: {
        options: {
          sourceMap: true
        },
        files: {
          'styles/css/main.min.css': ['styles/less/main.less']
        }
      },
      dist: {
        options: {
          compress: true
        },
        files: {
          'styles/css/main.min.css': ['styles/less/main.less']
        }
      }
    }

### [grunt-contrib-sass](https://github.com/gruntjs/grunt-contrib-sass)
Task for compiling Sass files into CSS. Can include bourbon stylesheet as includes. Can include source maps for better debug and insection in Chrome. Also be sure to checkout [grunt-sass](https://github.com/sindresorhus/grunt-sass), which uses a C++ to compile SASS resulting in a much speedier compile time. This task is experimental and may not include all the latest or most stable SASS compilation functionality.

	sass: {
		options: {
			includePaths: [
				'modules', 'node_modules/node-bourbon/assets/stylesheets'
			],
			sourceMap: false,
			outputStyle: 'compressed'
		},
		build: {
			files: {
				'css/theme.min.css': 'css/scss/theme.scss'
			}
		}
	}


### [grunt-contrib-watch](https://github.com/gruntjs/grunt-contrib-watch)
This task will continuously monitor a directory for changes, and run Grunt tasks based on the files that were updated. Often used to continually compile preprocessors during development. Watch also includes LiveReload. If you insert the LiveReload script into your project, use the browser extension, or use a node server with live reload middleware, you can see changes in your site immediately after you update the files.

### [grunt-contrib-connect](https://github.com/gruntjs/grunt-contrib-connect)
Connect is a static web server for Node.js. When using with Grunt, you can mount one or more folders from your project as its own site on your development machine. You can also enable middleware such as connect-livereload to automatically insert scripts or otherwise process your files before being served.

In Gruntfile.js (outside config):

    var liveReload = require('connect-livereload');
    var mountFolder = function(connect, dir) {
      return connect.static(require('path').resolve(dir));
    };

In Gruntfile.js (config):

    connect: {
      options: {
        port: 9000,
        hostname: '0.0.0.0'
      },
      dev: {
        options: {
          middleware: function(connect) {
            return [
              liveReload(),
              mountFolder(connect, '.tmp'),
              mountFolder(connect, 'src')
            ];
          }
        }
      }
    }

### [grunt-contrib-copy](https://github.com/gruntjs/grunt-contrib-copy) & [grunt-contrib-clean](https://github.com/gruntjs/grunt-contrib-clean)
A couple utility tasks for managing files in your build process. Copy allows you to copy files from one directory to another (useful for copying static assets from src to dist, for example) and clean will delete a set of files so you can start clean for each build.

### [grunt-autoprefixer](https://github.com/nDmitry/grunt-autoprefixer)
This task refactors a css file to include all relevant browser-specific css prefixes. It allows you to target all or specific browsers, including IE css hacks.

	autoprefixer: {
		build: {
			src: 'css/theme.min.css',
			dest: 'css/theme.min.css'
		}
	}


## Other recommended packages
- [Assemble](https://github.com/assemble/assemble)
- [grunt-contrib-imagemin](https://github.com/gruntjs/grunt-contrib-imagemin)
- [responsive_images](https://github.com/andismith/grunt-responsive-images)
- [gh-pages](https://github.com/tschaub/grunt-gh-pages)
