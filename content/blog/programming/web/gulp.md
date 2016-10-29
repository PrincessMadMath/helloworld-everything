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

(todo complete)

### 3. Basic task

### 4. Task sequence

### 5. Simple task

### 6. Simple watch


## Gulp starter pack

Knowing all we know now, I decide to look into bit more into good practices with gulp so I can create a starter-pack that can boost the start of futur static web-site.

What other tools we could want:  
* Source-mapping
* Linting
* Auto-prefixer
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