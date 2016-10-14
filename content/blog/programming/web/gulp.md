+++
tags = ["tools", "web", "automation"]
categories = ["programming", "web"]
date = "2016-10-14T11:29:44-04:00"
title = "Hello gulp"

+++

## Overview

In the series of creating a starter-pack for a static web site.

## Article plan

1. [Goal of this post](#goal-of-this-post)  
2. [Why use gulp](#why-use-gulp )


### Goal of this post

Here a list of what I hope you will be able to do after reading this article:

* Explain why task tunner are useful
* Understand core concept of Gulp 4.^
* Basic use of gulp for a project
* How to go further

### Why use gulp 

#### Problem it try to solve


#### Alternative


### Important concept

#### Set-up (todo)

Just an other javascript-package

1. Be sure to have the package.json init in the folder
2. Install gulp-cli globally  
npm install gulpjs/gulp-cli -g
3. Install gulp
4. Create gulpfile.js

Here what the stucture folder should look like: 

![Initial directory structure: src, gulpfile.js, package.json](/img/blog/hello-gulp/initial_directory.PNG)

#### Concep

Let's look at the sample given by the [official documentation](https://www.npmjs.com/package/gulp-4.0.build)

```js
var gulp = require('gulp');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var imagemin = require('gulp-imagemin');
var del = require('del');

/* Defines globs to target specic files type */
var paths = {
  scripts: ['src/js/**/*.js', '!src/external/**/*.js'],
  images: 'src/img/**/*'
};
 
/* Register some tasks to expose to the cli */
gulp.task('build', gulp.series(
                      clean,
                      gulp.parallel(scripts, images)
                    ));
gulp.task(clean);
gulp.task(watch);
 
// The default task (called when you run `gulp` from cli) 
// Will build then watch for changes
gulp.task('default', gulp.series('build', watch));
 
 
/* Define our tasks using plain functions */
 
// Will delete the directory name build
function clean() {
  // Return a promise: (gulp required that the function return a promise, a stream or alternatively take a call back and call it)
  return del(['build']);
}
 
// Take all images find in paths.images, minify them and save each of them in "build/img" (initial one doesn't change)
function images() {
  return gulp.src(paths.images)
    // imagemin is the module required at the beginning of the files, some module can take option
    .pipe(imagemin({optimizationLevel: 5}))
    .pipe(gulp.dest('build/img'));
}
 
// Minify all JavaScript (except vendor scripts) and save the concatanate result in "build/js"
function scripts() {
  return gulp.src(paths.scripts)
    .pipe(uglify())
    .pipe(concat('all.min.js'))
    .pipe(gulp.dest('build/js'));
}
 
// Rerun the task when a file changes 
function watch() {
  gulp.watch(paths.scripts, scripts);
  gulp.watch(paths.images, images);
}

```


## Application exemple


## Gulp starter pack


## Annex A: Questions I asked myself.

### How does it work behing the scene? 

### What are the alternative of gulp?

First you can look at other task runner: 

* Grunt
-> getting their powerfulness from all the available plugins


Then you can ask yourself if we can do thing differently: here come bundlers.

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
[Official documentation](https://github.com/gulpjs/gulp/blob/master/docs/README.md)