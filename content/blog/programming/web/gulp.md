+++
tags = ["tools", "web", "automation"]
categories = ["programming", "web"]
date = "2016-10-14T11:29:44-04:00"
title = "Hello gulp"

+++

# Overview

In the series of creating a starter-pack for a static web site.


## Goal of this post

Here a list of what I hope you will be able to do after reading this article:

* Explain why task tunner are useful
* Understand core concept of Gulp 4.^
* Undestand the starter project using gulp


## Problem Gulp is tacking


# Important concept

#### Word to understand

* cli: 
  * command line interface

#### Set-up (todo)

Just an other javascript-package

1. Be sure to have the package.json init in the folder
2. Install gulp-cli globally  
npm install gulpjs/gulp-cli -g
3. Install gulp
4. Create gulpfile.js

Here what the stucture folder should look like: 

![Initial directory structure: src, gulpfile.js, package.json](/img/blog/hello-gulp/initial_directory.PNG)

```c
|- src/
    |- css/
|- gulpfile.js
|- package.json

```

#### Concep

[Source code](https://github.com/PrincessMadMath/hwe_gulp-introduction/tree/1_Copying)

gulpfile.js:

```js
/* 1: Import required module for our task: all gulp-plugin necessary will be added here */
var gulp = require('gulp');

/* 2: Defines globs to target specic files type */
var paths = {
  scripts: ['src/js/**/*.js', '!src/external/**/*.js'],
};
 
/****** Register some tasks to expose to the cli or other task ******/

// 3-1: We can call this task writing: gulp build
gulp.task('build',  scripts);

// 3-2: We can call using: gulp watch
gulp.task(watch);
 
// 3-3: The default task, we can call using: gulp
gulp.task('default', gulp.series('build', watch));
 
 
/****** Define our tasks using plain functions (available in Gulp 4.0) ******/
 
// 5: Copying all JavaScript (except vendor scripts) into "build/js"
function scripts() {
  return gulp.src(paths.scripts)
    .pipe(gulp.dest('build/js'));
}
 
// 6: Re-run the task when a file changes 
function watch() {
  gulp.watch(paths.scripts, scripts);
}
```

### 1. Import

Working with gulp imply using many plugins: one to minify, an other to compile sass and so on. To be able to use them we need to import them in the gulpfile.

Here it my current workflow to use a new plugin.  
1. Know what I want
2. Find a plugin that does what I want (using the [official pluggins registry](http://gulpjs.com/plugins/) or any search engine)
3. Install the pluggins to the dev dependencies (production don't need to perform gulp task)

```
npm install [pluginName] --save-dev
```

4. Add it in the gulpfile.js

```js
var relevantName = require("pluginName");
```


### 2. Node Glob: why and How

As you know by now, gulp is all about taking files and do some work with it. So, it is important to have an easy way to specified which files we want to operate on.
Here come glob, a matching pattern make for files and easier to use than regex.

When globbing we pass an array of pattern, which you should know:

* Name wildcard (*): i.e : "*.scss" any file ending with .scss
* Folder wildcard (**): i.e : "src/**/*.css": File in any subfolder inside src ending with .css
* Exclusion (!):  Could target a repo ("!vendor/") or a specific file ("!.config")
* Combine pattern (+(|)): i.e: Could be use to find any style file ("*.+(scss|css))

Glob used in the starter project: 

```js
  html: ['**/*.html'],
  sass: ['scss/**/*.scss'],
  css : ['css/**/*.css'],
  js: ['js/**/*.js'],
  images: ['img/**/*'],
```


### 3. Tasks

- FMORR: So now I know that Gulp is a task runner. Can you tell me more about those task!

#### 3.1 Running Tasks

- FMORR: So tell me, how can I execute 

Their is multiple way to execute a task. First they should be available in the gulpfile.js (the name of the file is important and should be in the repo where you use the CLI).

As we saw in the exemple above their is 3 way to declare a task:

```js
/****** Register some tasks to expose to the cli ******/

// 1: Explicitly declare the name ()
gulp.task('build',  scripts);

// 2: Implicitly use the function name
gulp.task(watch);
 
// 3: "default" is a reserve keyworkd to declare the default task
gulp.task('default', gulp.series('build', watch));
```

Running #1: 
```
gulp build
```

Running #2: 
```
gulp watch
```

Running #3: 
```
gulp
```

- FMORR: What about using the implicit naming and a task sequence...

Well you can't and anyway it would result in a mess!


- FMORR: How can I see which task is available

* Read the gulpfile.js and find the task declaration
* If using gulp 4.0: you can use the following build-in command to list all the tasks and their composition (the blue-green name is task that you can call)
```
gulp --tasks
```


#### 3.2 Tasks sequence

- FMORR: I like having decouple code. Is it possible to create small independant function and group them together.

Well yes, it would be a poor task runner otherwise! Gulp 4.0 allows to create sequence of task and you can define which task execute in series and which one in parallel.

```js
// We use gulp.series: because we don't want to start the build before the pre-build is completed
gulp.task('compile', gulp.series('build:assets', 'build'));

// We use a composition of series and parallel because we first want to delete the assets folder,
// then we can create the new one. And every assets type can be build independently
gulp.task('build:assets', gulp.series(
  cleanAsset, 
  gulp.parallel(copyHtml, copyScript, copyCss, copyImages, compileSass)
));

// Same explication as above
gulp.task('build', gulp.series(
  cleanBuild,
  gulp.parallel(buildHtml, buildScript, buildCss, buildImages)
));
```

- FMORR: What happen in an error occurs in a task in series

The task stop and the none of the following is execute.

#### 3.3 Task implementation

The normal implementation of a task consist to selecting some files using globs, do some work with them, and write the resulting files at the target location.


### 4. Watch

A watch, is something that will execute tasks when certain event occurs and will continue to do so unless the watcher is stop or an error occurs.

## Gulp starter pack 

Knowing all we know now, I decide to look into bit more into good practices with gulp so I can create a starter-pack that can boost the start of futur static web-site.

[Source Code](https://github.com/PrincessMadMath/helloworld-gulp-starter)

### Pluggins

Task we want to perform
* Linting: ensure that everybody use the same convention
** How enforce before comitting/pushing?
* Minify and concat files
** Have better performance
* Auto-prefixer
** Make cleaner style sheet: does not need to add vendor-prefix


### Config file explication

In this [article](https://www.freshconsulting.com/how-to-organize-your-gulp-js-development-builds-for-multiple-environments/), they talk about using a config files to remove
the responsability of specifiyng globs, plugins options,... in the tasks files. It also allow to see everything in one place, easier modification and to re-use tasks for
different environnements. 


config.js: 

```js
/***************  Load utility plugin to make the config files work ***************/

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
    // Warning: changing this name require to add it the .gitignore
    dist: "build/",
    // Glob to target specific files type (used in combinaison of prefix path)
    glob: {
        html: ['**/*.html'],
        sass: ['scss/**/*.scss'],
        css : ['css/**/*.css'],
        js: ['js/**/*.js'],
        images: ['img/**/*'],
    },
    // 
    dest: {
        html: '',
        css : 'css/',
        sass: 'scss/',
        js: 'js/',
        images: 'img/',
    },
}

// Function to prefix a parent directory to each elements in an array of globs:
//    -> return an array of globs
// i.e: I want each css files in the src directory: prefixGlob(paths.src, paths.glob.css)
function prefixGlob(prefix, glob){
    return glob.map(function(el) { 
        return prefix + el; 
    })
}



/*************** Little explication ***************

For the nexts part, it will be split in the different available environnement.

The default value will be used for property that were not overriden.

**************************************************/


/*************** Constants ***************/


// Currently I don't use constant but just to know it is possible
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

// Declare which plugins to run for each environnment
// The cleanest way is to set to false in default and enable them after
var run = {
    default: {
        js: {
            uglify: false
        },
        css: {
            cssnano: false,
            autoprefixer : true,
        },
        html: {
            useref : false
        }
    },

    development: {
    },

    production: {
        js: {
            uglify: true
        },
        css: {
            cssnano: true
        },
        html: {
            useref : true
        }
    }
};


/*************** Plugin options ***************/

// Declare pluggins option 
var plugin = {
    default: {
        js: {
            uglify: {
                mangle: false
            }
        },
        css: {
            autoprefixer : {
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


// Merge: set the options depending of the environnement and using the default value if not specified
var runOpts = _.merge( {}, run.default, run[ env ] );
var pluginOpts = _.merge( {}, plugin.default, plugin[ env ] );
var constantsOpts = _.merge( {}, constants.default, constants[ env ]);

// Make object available to use
module.exports.constants = constantsOpts;
module.exports.run = runOpts;
module.exports.plugin = pluginOpts;

module.exports.paths = paths;
module.exports.prefixGlob = prefixGlob;

```

### Task files explication

It was often recommended to split the tasks in multiple files to again, make things cleaner.  

In my case I split in the following files:
* build.js: Tasks to build the project: make-it browser-ready
* watcher.js: Tasks to start a auto-reload server: to use when developping
* lint.js: Tasks to lint files


gulpfile.js:  

```js
var gulp = require('gulp');

// Pluggin to support splitting gulp task in multiple files
var requireDir = require('require-dir');

// If we want to use tasks declare in other files: this plugging enable forward-reference
var fwdRef = require('undertaker-forward-reference');


// Start registry to support forward-reference
gulp.registry(fwdRef());

// Load differents taks found in the files
requireDir('./gulp');
```





#### build.js

* Todo: add schema of build pipeline (vertical (step-by-step) vs horizontal (update))

```js
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
  cleanAsset, 
  gulp.parallel(copyHtml, copyScript, copyCss, copyImages, compileSass)
));

// Different operation to do on basic files: minify, autoprefixer,...
gulp.task('build:build', gulp.series(
  cleanBuild,
  gulp.parallel(buildHtml, buildScript, buildCss, buildImages)
));

// Any task that need to be perform on "final" files
gulp.task('build:post', postBuildConcat);

/*** Tasks to build step-by-step ***/
gulp.task('build', gulp.series('build:check', 'build:compile', 'build:build', 'build:post'));


gulp.task('clean', gulp.parallel(cleanAsset, cleanBuild));


/*** Tasks to update assets type ***/
gulp.task('update:html', gulp.series(copyHtml, buildHtml, postBuildConcat));
gulp.task('update:css', gulp.series(copyCss, buildCss, postBuildConcat));
gulp.task('update:sass', gulp.series(compileSass, buildCss, postBuildConcat));
gulp.task('update:js', gulp.series(copyScript, buildScript, postBuildConcat));
gulp.task('update:image', gulp.series(copyImages, buildImages));


/*************** Utils function ***************/

function cleanAsset() {
    return del([config.paths.temp]);
}

function cleanBuild() {
    return del([config.paths.dist]);
}

/*************** Compile function ***************/

function copyHtml(){
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.html);
    var dest = config.paths.temp + config.paths.dest.html;

    return gulp.src(glob)
    .pipe(gulp.dest(dest));
}

function copyScript(){
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.js);
    var dest = config.paths.temp + config.paths.dest.js;

    return gulp.src(glob)
    .pipe(gulp.dest(dest));
}

function copyCss(){
    var glob = config.prefixGlob(config.paths.src,config.paths.glob.css);
    var dest = config.paths.temp + config.paths.dest.css;

    return gulp.src(glob)
    .pipe(gulp.dest(dest));
}

function copyImages(){
    var glob = config.prefixGlob(config.paths.src,config.paths.glob.images);
    var dest = config.paths.temp + config.paths.dest.images;

    return gulp.src(glob)
    .pipe(gulp.dest(dest));
}

function compileSass(){
    var glob = config.prefixGlob(config.paths.src,config.paths.glob.sass);
    var dest = config.paths.temp + config.paths.dest.css;

    return gulp.src(glob)
    .pipe(sass())
    .pipe(gulp.dest(dest));
}


/*************** Build functions (make ready for production) ***************/

function buildHtml(){
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
    .pipe(imagemin({optimizationLevel: 5}))
    .pipe(gulp.dest(dest));
}

function buildScript() {
    var glob = config.prefixGlob(config.paths.temp, config.paths.glob.js);
    var dest = config.paths.dist + config.paths.dest.js;

    return gulp.src(glob)
    .pipe(gulpif(config.run.js.uglify, uglify(config.plugin.js.uglify)))
    .pipe(gulp.dest(dest));
}


/*************** Post-build task (final file modification) ***************/

// Because we concat files based on the declaration in the .html, we need to
// run the concat everytime one of the target files is update 
// (or else it is not in the concat version)
function postBuildConcat(){
    var glob = config.prefixGlob(config.paths.dist, config.paths.glob.html);
    var dest = config.paths.dist + config.paths.dest.html;

    return gulp.src(glob)
    .pipe(gulpif(config.run.html.useref, useref()))
    .pipe(gulp.dest(dest));
}

```


watcher.js:  

```js
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

    gulp.watch(html_glob, gulp.series('update:html'));
    gulp.watch(js_glob, gulp.series('update:js'));
    gulp.watch(image_glob, gulp.series('update:image'));
    gulp.watch(sass_glob, gulp.series('update:sass'));
    gulp.watch(css_glob, gulp.series( 'update:css'));
}

// When change occurs in /build we want to notify the server (reload or inject)
function watchServer() {
    var html_glob = config.prefixGlob(config.paths.dist,config.paths.glob.html);
    var js_glob = config.prefixGlob(config.paths.dist,config.paths.glob.js);
    var image_glob = config.prefixGlob(config.paths.dist,config.paths.glob.images);
    var css_glob = config.prefixGlob(config.paths.dist,config.paths.glob.css);

    gulp.watch(html_glob, reload);
    gulp.watch(js_glob, reload);
    gulp.watch(image_glob, reload);
    gulp.watch(css_glob,  injectCss);
}

/*************** Browsersync helper function ***************/

function startServer(){
    browserSync.init({
        server: {
            baseDir: config.paths.dist
        }
    });
}

// Inject css files (instead of reloading)
function injectCss(){
    var css_glob = config.prefixGlob(config.paths.dist, config.paths.glob.css);

    return gulp.src(css_glob)
    .pipe(browserSync.stream());
}

function reload(done){
    browserSync.reload();
    done();
}

```

lint.js: 

```js
var gulp = require('gulp');
var config = require('./config');

/*** js lint ***/
var eslint = require('gulp-eslint');


/*** style tools ***/
var postcss = require('gulp-postcss');


/*** style lint ***/
var stylelint = require('stylelint')
var syntax_scss = require('postcss-scss');
var reporter = require('postcss-reporter');

var stylefmt = require('stylefmt');

var sorting = require('postcss-sorting');


/*** util ***/
// We also use lint for formatting, if we must only overwrite if a changed was
// made or else we have an infinite loop
var changed = require('gulp-changed');


gulp.task('lint', gulp.parallel(jsLint, cssLint, sassLint));
gulp.task('lint:js', jsLint);
gulp.task('lint:style', gulp.parallel(cssLint, sassLint));

gulp.task('lint:watch', gulp.series('lint', watchLint));



/*************** Function to lint js ***************/

function jsLint(){
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.js);
    var dest = config.paths.src + config.paths.dest.js;
    
    return gulp.src(glob)
        .pipe(eslint(config.plugin.lint.eslint))
        .pipe(eslint.format())
        .pipe(eslint.failAfterError())
        .pipe(changed(dest))
        .pipe(gulp.dest(dest));
}



/*************** Function to lint css and sass ***************/

// Additionnal linting that we want to do
//  sorting: will sort css attribute
//  stylefmt: will format css code
var processors = [
    stylefmt,
    sorting,
    stylelint({failAfterError: true}),
    reporter({ clearMessages: true }),
];

function cssLint(){
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.css);
    var dest = config.paths.src + config.paths.dest.css;

    return gulp.src(glob)
    .pipe(postcss(processors))
    .pipe(changed(dest))
    .pipe(gulp.dest(dest));
}


function sassLint(){
    var glob = config.prefixGlob(config.paths.src, config.paths.glob.sass);
    var dest = config.paths.src + config.paths.dest.sass;

    return gulp.src(glob)
    .pipe(postcss(processors, {
        syntax: syntax_scss
    }))
    .pipe(changed(dest))
    .pipe(gulp.dest(dest));
}


// When a modification is done in the src folder, update the build
function watchLint() {
    var js_glob = config.prefixGlob(config.paths.src, config.paths.glob.js);
    var sass_glob = config.prefixGlob(config.paths.src, config.paths.glob.sass);
    var css_glob = config.prefixGlob(config.paths.src, config.paths.glob.css);

    gulp.watch(js_glob, jsLint);
    gulp.watch(css_glob, cssLint);
    gulp.watch(sass_glob,  sassLint);
}
```



## Annex A: Questions I asked myself.

### How does it work behing the scene? 

Each pluggin manipulate virtual file that are streamed with node's stream (all manipulation is done in memoery: there is no intermediate files). You can used streamed file (access by chunk) or a buffer (if the whole file is needed before starting task).

I created 2 minimal plugin that lower every caracter. Minimal because no check nor error handling.


Using buffer: 

```js
var through = require('through2');

function transformText(file)
{
    var content = file.contents.toString('utf8');
    return content.toLowerCase();
}

// plugin level function: will deals which each files
function toLowerPlugin(){

    function handleFile(file, enc, callback){

        //  if (file.isBuffer()) 
        var output = transformText(file);

        file.contents = new Buffer(output, 'utf8');

        // Notify stream engine that we are done with this file
        callback(null, file);
    }

    return  through.obj(handleFile);
}

// Make it available to use
module.exports = toLowerPlugin;
```


Using stream:

They recommend also supporting streaming, but most of the exemple I found don't support them. Maybe I will give an exemple later


[Good documentation](https://medium.com/@webprolific/getting-gulpy-a2010c13d3d5#.dosj4tpx9)  


### What are the alternative of gulp?

First you can look at other task runner: 

* Grunt
-> getting their powerfulness from all the available plugins


Then you can ask yourself if we can do thing differently: here come bundlers.


### I saw there is multiple way we can use watcher, can you tell me more about them.

### What about npm, packages and modules?

The purpose of npm is to shared small block of code solving a problem (called package).

-_Ok but what consist of a package?_ 

Well it is simple a directory where you find a package.json file.

-_Ohhh and what can it do?_

It can add new available command for the command line (like gulp) or export method that you can use in you js file.  

-_Ok last question! What is the difference between a module and a package?_

Everything whithin require() is a module. A module is a single js file exposing multiple function that we can use (look for the **exports** keyword). A package can contains multiple modules dand depend on other packages and is contains an entry point (often index.js) that is use in the require().


## Annex B: Sources

[Source code of Gulp](https://github.com/gulpjs/gulp)
[Official code sample][official documentation](https://www.npmjs.com/package/gulp-4.0.build)
[Official documentation](https://github.com/gulpjs/gulp/blob/master/docs/README.md)


## What next

* [Impact of minification](http://www.yottaa.com/company/blog/application-optimization/how-does-reducing-javascript-requests-minifying-javascript/)
* Debug javascript
* Setup IDE to support linting
* CI/CD