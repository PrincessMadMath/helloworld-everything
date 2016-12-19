---
title : "Gulp starter pack"
tags : ["tools", "web", "automation"] 
categories : ["programming", "web"]
lastmod: 2016-11-17 
date : "2016-10-14" 
---

Simple starter pack for a static web site using the task-runner gulp (with focus on good practice). Use lots of sweet gulp plugin to make the development easier.

<!--more--> 



# Overview

In the series of creating a starter pack for a static web site.


<!-- toc -->

## Goal of this post

Here a list of what I hope you will be able to do after reading this article:

* Explain why task runners are useful
* Understand core concept of Gulp 4.^
* Understand the starter project using gulp

## Problem Gulp is tacking

# Important concept

Word to understand

* cli: 
* command line interface

Set-up 

The easiest set-up instruction is found on the [official documentation](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md)

But because I used Gulp 4 and it is not released you need to replace

~~~bash
npm install --save-dev gulp 
~~~

by

~~~bash
npm install gulpjs/gulp.git#4.0  --save-dev 
~~~

Otherwise a good introduction between the difference of gulp 3 and gulp 4, can be found in this [guide](https://www.joezimjs.com/javascript/complete-guide-upgrading-gulp-4/)

[Source code](https://github.com/PrincessMadMath/hwe_gulp-introduction/tree/1_Copying)

Let's take this following example and analyze it lines by lines:

gulpfile.js:

~~~javascript 

/* 1: Import required modules for our task: all gulp-plugin necessary will be added here */ 
var gulp = require('gulp');

/* 2: Defines globs to target specific files type */ 
var paths = {   scripts: ['src/js/**/*.js', '!src/external/**/*.js'], };

/****** Register some tasks to expose to the cli or other tasks ******/

// 3-1: We can call this task writing:
gulp build gulp.task('build',  scripts);

// 3-2: We can call using: 
gulp watch gulp.task(watch);

// 3-3: The default task, we can call using: 
gulp gulp.task('default', gulp.series('build', watch));

/******  Define our tasks using plain functions (available in Gulp 4.0) ******/

// 5: Copying all JavaScript (except vendor scripts) into "build/js" 
function scripts() {   
    return gulp.src(paths.scripts)     .pipe(gulp.dest('build/js'));
}

// 6: Rerun the task when a file changes  
function watch() {   
    gulp.watch(paths.scripts, scripts);
}
~~~


## 1. Import

Working with gulp implies using many plugins: one to minify, another to compile sass and so on. To be able to use them we need to import them in the gulpfile.

Here it my current workflow to use a new plugin.  
1. Know what I want
2. Find a plugin that does what I want (using the [official plugin registry](http://gulpjs.com/plugins/) or any search engine)
3. Install the plugins to the dev dependencies (production doesn't need to perform gulp task)

~~~bash
$ npm install [pluginName] --save-dev 
~~~

4. Add it in the gulpfile.js

~~~javascript
var relevantName = require("pluginName");
~~~

## 2. Node Glob: why and How

As you know by now, gulp is all about taking files and do some work with it. So, it is important to have an easy way to specify which files we want to operate on.
Here come glob, a matching pattern makes for files and easier to use than regex.

When globbing we pass an array of patterns, which you should know:

* Name wildcard (*): i.e : "*.scss" any file ending with .scss
* Folder wildcard (**): i.e : "src/**/*.css": File in any subfolder inside src ending with .css
* Exclusion (!):  Could target a repo ("!vendor/") or a specific file ("!.config")
* Combine pattern (+(|)): i.e: Could be use to find any style file ("*.+(scss|css))

Glob used in the starter project: 

~~~javascript   
html: ['**/*.html'],   
sass: ['scss/**/*.scss'],   
css : ['css/**/*.css'],   
js: ['js/**/*.js'],   
images: ['img/**/*'], 
~~~

## 3. Tasks

> FMORR: So now I know that Gulp is a task runner. Can you tell me more about those task!

### 3.1 Running Tasks

> FMORR: So tell me, how can I execute? 

There is multiple way to execute a task. First they should be available in the gulpfile.js (the name of the file is important and should be in the repo where you use the CLI).

As we saw in the example above there are 3 ways to declare a task:

~~~javascript
/****** Register some tasks to expose to the cli ******/

// 1: Explicitly declare the name
gulp.task('build',  scripts);

// 2: Implicitly use the function name 
gulp.task(watch);

// 3: "default" is a reserve keyword to declare the default task 
gulp.task('default', gulp.series('build', watch));
~~~

Running #1:  

~~~bash
 gulp build 
 ~~~

Running #2:  

~~~bash
gulp watch 
~~~

Running #3: 

~~~bash
gulp 
~~~

> FMORR: What about using the implicit naming and a task sequence?

Well, you can't and anyway it would result in a mess!

> FMORR: How can I see which task is available

* Read the gulpfile.js and find the task declaration
* If using gulp 4.0: you can use the following build-in command to list all the tasks and their composition (the blue-green name is task that you can call) ~~~Io $ gulp --tasks ~~~

### 3.2 Tasks sequence

> FMORR: I like having decoupled code. Is it possible to create small independent function and group them together?

Well, yes, it would be a poor task runner otherwise! Gulp 4.0 allows to create sequence of tasks and you can define which task execute in series and which one in parallel.

~~~javascript 
// We use gulp.series: because we don't want to start the build before the pre-build is completed 
gulp.task('compile', gulp.series('build:assets', 'build'));

// We use a composition of series and parallel because we first want to delete the assets folder, 
// then we can create the new one. And every asset type can be built independently 
gulp.task('build:assets', gulp.series(   
    cleanAsset,    
    gulp.parallel(copyHtml, copyScript, copyCss, copyImages, compileSass) ));

// Same explication as above 
gulp.task('build', gulp.series(   
    cleanBuild,   
    gulp.parallel(buildHtml, buildScript, buildCss, buildImages) ));
~~~

> FMORR: What happens if an error occurs in a task in series

The task stop and the none of the following is executed.

### 3.3 Task implementation

The normal implementation of a task consists of selecting some files using globs, do some work with them, and write the resulting files at the target location.

## 4. Watch

A watcher is something that will execute tasks when certain event occurs and will continue to do so unless the watcher is stopped (ctrl+c) or an error occurs.

# Gulp starter pack 

Knowing all we know now, I decide to look into bit more into good practices with gulp so I can create a starter pack that can boost the start of your/my future static web site.

[Source Code](https://github.com/PrincessMadMath/helloworld-gulp-starter)

## Plugins

Task we want to perform
* Linting: ensure that everybody uses the same convention
** How enforce before committing/pushing?
* Minify and concat files 
** Have better performance
* Autoprefixer  
** Make cleaner style sheet: does not need to add vendor prefix

## Config file explication

In this [article](https://www.freshconsulting.com/how-to-organize-your-gulp-js-development-builds-for-multiple-environments/), they talk about using a config files to remove the responsibility of specifying globs, plugin options,... in the tasks files. It also allows to see everything in one place, easier modification and to reuse tasks for different environments. 

config.js: 

~~~javascript
/*************** Load utility plugin to make the config files work ***************/

// Contain function to merge object
var _ = require('lodash');
// Be able to get CLI command parameters (for --env)
var gutil = require('gulp-util')

/*************** Path ***************/

// All tasks reference path here: make it easier to modify them
var paths = {
    src: "src/",
    // Warning: changing this name require to add it the .gitignore  
    temp: ".temp/",
    // Changing the dist folder name also required change in .gitignore and the npm deploy scripts
    dist: "build/",
    glob: {
        html: ['**/*.html'],
        sass: ['scss/**/*.scss'],
        css: ['css/**/*.css'],
        js: ['js/**/*.js'],
        images: ['img/**/*'],
        assets: ['assets/**/*']
    },
    dest: {
        html: '',
        css: 'css/',
        sass: 'scss/',
        js: 'js/',
        images: 'img/',
        assets: 'assets/'
    },
}

// Function to prefix a parent directory to each elements in an array of globs: 
//    -> return an array of globs 
// i.e: I want each css files in the src directory: prefixGlob(paths.src, paths.glob.css) 
// Todo: this should but not calculated at runtime...
function prefixGlob(prefix, glob) {
    return glob.map(function (el) {
        return prefix + el;
    })
}

/*************** Little explication ***************

For the next part, it will be split in the different available environment.

The default value will be used for properties that were not overridden. 

**************************************************/


/*************** Constants ***************/

// Currently I don't use constants but just to know it is possible 
var constants = {
    default: {
        placeHolderConstant: ''
    },
    development: {
        placeHolderConstant: 'dev'
    },
    production: {
        placeHolderConstant: 'prod'
    }
};



/*************** Plugin toggling ***************/

// Declare which plugins to run for each environment // The cleanest way is to set to false in default and enable them after
var run = {
    default: {
        js: {
            uglify: false
        },
        css: {
            cssnano: false,
            autoprefixer: true,
        },
        html: {
            useref: false
        }
    },

    development: {
        js: {
            uglify: false
        },
        css: {
            cssnano: false
        }
    },

    production: {
        js: {
            uglify: true
        },
        css: {
            cssnano: true
        },
        html: {
            useref: true
        }
    }
};


/*************** Plugin options ***************/

// Declare pluggins option
var plugin = {
    default: {
        js: {
            uglify: {
                mangle: true
            }
        },
        css: {
            autoprefixer: {
                browsers: ['last 2 versions'],
            }
        },
        lint: {
            eslint: {
                configFile: ".eslintrc",
                fix: true
            }
        }
    },

    development: {
        js: {
            uglify: {

                mangle: false
            }
        },
        lint: {
            eslint: {
                configFile: ".dev.eslintrc",
            }
        }
    },

    production: {
        js: {
            uglify: {
                mangle: true
            }
        }
    }
};


/*************** Exports ***************/

// How to set : --env=[name]
var env = gutil.env.env || 'development';


// Merge: : set the options depending of the environment and using the default value if not specified
var runOpts = _.merge({}, run.default, run[env]);
var pluginOpts = _.merge({}, plugin.default, plugin[env]);
var constantsOpts = _.merge({}, constants.default, constants[env]);

// Make object available to use
module.exports.constants = constantsOpts;
module.exports.run = runOpts;
module.exports.plugin = pluginOpts;

module.exports.paths = paths;
module.exports.prefixGlob = prefixGlob;

~~~

## Task file explication

It was often recommended to split the tasks in multiple files to again, make things cleaner.  

In my case I split in the following files:
* build.js: Tasks to build the project: make-it browser-ready
* watcher.js: Tasks to start a auto-reload server: to use when developing
* lint.js: Tasks to lint files

gulpfile.js:  

~~~javascript 

var gulp = require('gulp');

// Plugin to support splitting gulp task in multiple files 
var requireDir = require('require-dir');

// If we want to use tasks declare in other files: this plugging enable forward-reference 
var fwdRef = require('undertaker-forward-reference');

// Start registry supporting forward-reference
gulp.registry(fwdRef());

// Load different tasks found in the files
requireDir('./gulp');
~~~

build.js

* Todo: add schema of build pipeline (vertical (step-by-step) vs horizontal (update))

~~~javascript 

var gulp = require('gulp');
var config = require('./config');

/*** html pluggins ***/
var useref = require("gulp-useref");

/*** js plugins ***/
var uglify = require('gulp-uglify');

/*** style plugins ***/
var sass = require('gulp-sass');
var cssnano = require('gulp-cssnano');
var autoprefixer = require('gulp-autoprefixer');


/*** image plugins ***/
var imagemin = require('gulp-imagemin');

/*** util plugins ***/
var del = require('del');
var gulpif = require('gulp-if');



/*************** Task definition ***************/

gulp.task('build:check', gulp.parallel('lint:js', 'lint:style'));

// From superset files (sass, typescript (not implemented)) to files usable by the browser (css, plain js,...)
gulp.task('build:compile', gulp.series(
    cleanCompile,
    gulp.parallel(copyHtml, copyScript, copyCss, copyImages, compileSass)
));

// Different operation to do on basic files: minify, autoprefixer,...
gulp.task('build:build', gulp.series(
    cleanBuild,
    gulp.parallel(buildHtml, buildScript, buildCss, buildImages, copyAssets)
));

// Any task that need to be perform on "final" files
gulp.task('build:post', postBuildConcat);

/*** Tasks to build step-by-step ***/
gulp.task('build', gulp.series('build:check', 'build:compile', 'build:build', 'build:post'));


gulp.task('clean', gulp.parallel(cleanCompile, cleanBuild));


/*** Tasks to update by type ***/
gulp.task('update:html', gulp.series(copyHtml, buildHtml, postBuildConcat));
gulp.task('update:css', gulp.series(copyCss, buildCss, postBuildConcat));
gulp.task('update:sass', gulp.series(compileSass, buildCss, postBuildConcat));
gulp.task('update:js', gulp.series(copyScript, buildScript, postBuildConcat));
gulp.task('update:image', gulp.series(copyImages, buildImages));
gulp.task('update:assets', copyAssets);


/*************** Utils function ***************/

function cleanCompile() {
    return del([config.paths.temp]);
}

function cleanBuild() {
    return del([config.paths.dist]);
}

/*************** Compile function ***************/

function copyHtml() {
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.html);
    var dest = config.paths.temp + config.paths.dest.html;

    return gulp.src(glob)
        .pipe(gulp.dest(dest));
}

function copyScript() {
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.js);
    var dest = config.paths.temp + config.paths.dest.js;

    return gulp.src(glob)
        .pipe(gulp.dest(dest));
}

function copyCss() {
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.css);
    var dest = config.paths.temp + config.paths.dest.css;

    return gulp.src(glob)
        .pipe(gulp.dest(dest));
}

function copyImages() {
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.images);
    var dest = config.paths.temp + config.paths.dest.images;

    return gulp.src(glob)
        .pipe(gulp.dest(dest));
}

function compileSass() {
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.sass);
    var dest = config.paths.temp + config.paths.dest.css;

    return gulp.src(glob)
        .pipe(sass())
        .pipe(gulp.dest(dest));
}


/*************** Build functions (make ready for production) ***************/

function buildHtml() {
    var glob = config.prefixGlob(config.paths.temp, config.paths.glob.html);
    var dest = config.paths.dist + config.paths.dest.html;

    return gulp.src(glob)
        .pipe(gulp.dest(dest));
}

// Only take from .temp and minify
function buildCss() {
    var glob = config.prefixGlob(config.paths.temp, config.paths.glob.css);
    var dest = config.paths.dist + config.paths.dest.css;

    return gulp.src(glob)
        .pipe(gulpif(config.run.css.autoprefixer, autoprefixer(config.plugin.css.autoprefixer)))
        .pipe(gulpif(config.run.css.cssnano, cssnano()))
        .pipe(gulp.dest(dest));
}

function buildImages() {
    var glob = config.prefixGlob(config.paths.temp, config.paths.glob.images);
    var dest = config.paths.dist + config.paths.dest.images;

    return gulp.src(glob)
        .pipe(imagemin({
            optimizationLevel: 5
        }))
        .pipe(gulp.dest(dest));
}

function buildScript() {
    var glob = config.prefixGlob(config.paths.temp, config.paths.glob.js);
    var dest = config.paths.dist + config.paths.dest.js;

    return gulp.src(glob)
        .pipe(gulpif(config.run.js.uglify, uglify(config.plugin.js.uglify)))
        .pipe(gulp.dest(dest));
}

function copyAssets() {
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.assets);
    var dest = config.paths.dist + config.paths.dest.assets;

    return gulp.src(glob)
        .pipe(gulp.dest(dest));
}


/*************** Post-build task (final file modification) ***************/

// Because we concat files based on the declaration in the .html, we need to
// run the concat everytime one of the target files is update 
// (or else it is not in the concat version)
function postBuildConcat() {
    var glob = config.prefixGlob(config.paths.dist, config.paths.glob.html);
    var dest = config.paths.dist + config.paths.dest.html;

    return gulp.src(glob)
        .pipe(gulpif(config.run.html.useref, useref()))
        .pipe(gulp.dest(dest));
}

~~~

watcher.js:  

~~~javascript 
var gulp = require('gulp');
var config = require('./config');

// Plugin to enable live-reload (https://www.browsersync.io/docs/gulp)
var browserSync = require('browser-sync').create();


/*************** Task definition ***************/
gulp.task('server', gulp.series('build', gulp.parallel('lint:watch', watchBuild, watchServer, startServer)));


/*************** Watcher functions (change -> build) ***************/

// When a modification is done in the src folder, update the build
function watchBuild() {
    var html_glob = config.prefixGlob(config.paths.src, config.paths.glob.html);
    var js_glob = config.prefixGlob(config.paths.src, config.paths.glob.js);
    var image_glob = config.prefixGlob(config.paths.src, config.paths.glob.images);
    var sass_glob = config.prefixGlob(config.paths.src, config.paths.glob.sass);
    var css_glob = config.prefixGlob(config.paths.src, config.paths.glob.css);
    var assets_glob = config.prefixGlob(config.paths.src, config.paths.glob.assets);

    gulp.watch(html_glob, gulp.series('update:html'));
    gulp.watch(js_glob, gulp.series('update:js'));
    gulp.watch(image_glob, gulp.series('update:image'));
    gulp.watch(sass_glob, gulp.series('update:sass'));
    gulp.watch(css_glob, gulp.series('update:css'));
    gulp.watch(assets_glob, gulp.series('update:assets'));
}

// When change occurs in /build we want to notify the server (reload or inject)
function watchServer() {
    var html_glob = config.prefixGlob(config.paths.dist, config.paths.glob.html);
    var js_glob = config.prefixGlob(config.paths.dist, config.paths.glob.js);
    var image_glob = config.prefixGlob(config.paths.dist, config.paths.glob.images);
    var css_glob = config.prefixGlob(config.paths.dist, config.paths.glob.css);
    var assets_glob = config.prefixGlob(config.paths.dist, config.paths.glob.assets);

    gulp.watch(html_glob, reload);
    gulp.watch(js_glob, reload);
    gulp.watch(image_glob, reload);
    gulp.watch(css_glob, injectCss);
    gulp.watch(assets_glob, reload);
}

/*************** Browsersync helper function ***************/

function startServer() {
    browserSync.init({
        server: {
            baseDir: config.paths.dist
        }
    });
}

// Inject css files (instead of reloading)
function injectCss() {
    var css_glob = config.prefixGlob(config.paths.dist, config.paths.glob.css);

    return gulp.src(css_glob)
        .pipe(browserSync.stream());
}

function reload(done) {
    browserSync.reload();
    done();
}

~~~

# Annex A: Questions I asked myself.

## How does it work behind the scenes? 

Each plugin manipulate virtual file that are streamed with node's stream (all manipulation is done in memoery: there is no intermediate files). You can use streamed files (access by chunks) or a buffer (if the whole file is needed before starting task).

I created 2 minimal plugin that lower every character. Minimal because no check nor error handling.

Using buffer: 

~~~javascript 
var through = require('through2');

function transformText(file) {
var content = file.contents.toString('utf8');
return content.toLowerCase();
}

// plugin level function: will deals which each files function toLowerPlugin(){

function handleFile(file, enc, callback){

//  if (file.isBuffer())          var output = transformText(file);

file.contents = new Buffer(output, 'utf8');

// Notify stream engine that we are done with this file         callback(null, file);
}

return  through.obj(handleFile);
}

// Make it available to use module.exports = toLowerPlugin;
~~~

Using stream:

They also recommend supporting streaming, but most of the example I found doesn't support them. Maybe I will give an example later.

[Good documentation](https://medium.com/@webprolific/getting-gulpy-a2010c13d3d5#.dosj4tpx9)  

## What are the alternatives of gulp?

First you can look at other task runners: 

* Grunt -> getting their powerfulness from all the available plugins, seem more complicated to setup/use than gulp

Then you can ask yourself if we can do things differently: here come bundlers.

## I saw there is multiple way we can use watchers, can you tell me more about them.

(todo)

## What about npm, packages and modules?

The purpose of npm is to shared small block of code solving a problem (called package).

> FMORR: OK but what consists of a package?_ 

Well, it is simple a directory where you find a package.json file.

> Ohhh and what can it do?_

It can add new available command for the command line (like gulp) or export method that you can use in you js file.  

> FMORR: OK last question! What is the difference between a module and a package?_

Everything within require() is a module. A module is a single js file exposing multiple function that we can use (look for the **exports** keyword). A package can contain multiple modules and depend on other packages and contains an entry point (often index.js) that is use in the require().

# Annex B: Sources

[Source code of Gulp](https://github.com/gulpjs/gulp)  
[Official code sample](https://www.npmjs.com/package/gulp-4.0.build)  
[Official documentation](https://github.com/gulpjs/gulp/blob/master/docs/README.md)  


