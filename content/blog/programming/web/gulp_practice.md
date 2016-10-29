+++
tags = ["tools", "web", "automation"]
categories = ["programming", "web", "practice"]
date = "2016-10-17T18:11:44-04:00"
title = "Simple Gulp Practice"

+++

## Overview

Simple step-by-step to automate the process of a static web page with gulp.



### Goal of this post

Often after reading a let's get started about a tech, I find my self not sure where to start applying what I learned.
So here is something to get your teeth start on!


#### Set-up (todo)

Just an other javascript-package

1. Download this [source code]()
2. Install gulp-cli globally  
npm install gulpjs/gulp-cli -g
3. In the repository install all npm module  
npm install 


## Learning-project

We (you and I) will build a simple project to show some useful capacity of gulp and its pluggins.
For some parts I will write every steps and other parts I will give directive for you to try. 
At the end of everystep, I will provided a link to the code source of how I implemented it.   

What the project will do:  
* Compile sass into css and minify them
* Minify and concat javascript file
* Live-reload: when we will change a html, css or js files the browser will automaticly reload and show the changes

### 1. Simply copying file

Using your knowledge to setup and and how implement simple task and watch, implements a gulpfile able to:  
* Copying javascript from src/js -> build/js 
* Copying images from src/img -> build/img
* On "gulp watch", watcher should copy files when one is updated or a newer one is add 
* On "gulp build", should clean build/ then copy js and images

[My solution here (todo)](https://github.com/PrincessMadMath/hwe_gulp-introduction/tree/1_Copying)


### 2. Minify everything!

Ok now you are a pro of moving files and watch some. Now we want to minify everything so it can be production ready. 
Your job (if you accept it) is to meet the following criteria:
* Uglify then concat all javascript files
* Compress imagemin

(todo: change)
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

### 3. Small improvement and changes

You may have notice that we do a lots of unecessary work about images: 1 image change/was added -> re-optimize everything!  
We can use the gulp-cache pluggin to solve this problem: now check this [pluggin](https://www.npmjs.com/package/gulp-cached/) and implement it!

Also you may have heard about sass, scrap the css and now use sass!


[My solution](#) (update)


### 4. Live-reload

We have almost a complete automatic process, we just want to stop hitting refresh when we make change. We want gulp to update that automatically for us.
We are in luck there is a useful module compatible with gulp for that: [Browsersync](https://www.browsersync.io/docs/gulp).
I found it harder to implement this one, so I will go step by step.
