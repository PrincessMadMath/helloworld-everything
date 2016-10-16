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
 
/****** Register some tasks to expose to the cli ******/

// 3-1: We can call this task writing: gulp build
gulp.task('build',  scripts);

// 3-2: We can call using: gulp watch
gulp.task(watch);
 
// 4: The default task, we can call using: gulp
gulp.task('default', gulp.series('build', watch));
 
 
/****** Define our tasks using plain functions ******/
 
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
```cli
npm install [pluginName] --save-dev
```
4. Add it in the gulpfile.js
```js
var relevantName = require("pluginName");
```


### 2. Node Glob: why and How

As you know by now, gulp is all about taking files and do some work with it. So, it is important to have an easy way to specified which files we want to operate on.
Here come glob, a matching pattern make for files and easier to use than regex.

(todo complete)

### 3. Basic task

### 4. Task sequence

### 5. Simple task

### 6. Simple watch




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
* Copying css file from src/css -> build/css
* Copying images from src/img -> build/img
* On "gulp watch", watcher should copy files when one is updated or a newer one is add 
* On "gulp build", should clean build/ and copy js, css and images

[My solution here (todo)](https://github.com/PrincessMadMath/hwe_gulp-introduction/tree/1_Copying)


### 2. Minify everything!

Ok now you are a pro of moving files and watch some. Now we want to minify everything so it can be production ready. 
Your job (if you accept it) is to meet the following criteria:
* Uglify then concat all javascript files
* Minify css
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


[My solution](#)


### 4. Live-reload

We have almost a complete automatic process, we just want to stop hitting refresh when we make change. We want gulp to update that automatically for us.
We are in luck there is a useful module compatible with gulp for that: [Browsersync](https://www.browsersync.io/docs/gulp).
I found it harder to implement this one, so I will go step by step.



## Gulp starter pack

Knowing all we know now, I decide to look into bit more into good practices with gulp so I can create a starter-pack that can boost the start of futur static web-site.

What other tools we could want:  
* Source-mapping
* Linting
(todo)


## Annex A: Questions I asked myself.

### How does it work behing the scene? 

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


## For me later

[Impact of minification](http://www.yottaa.com/company/blog/application-optimization/how-does-reducing-javascript-requests-minifying-javascript/)