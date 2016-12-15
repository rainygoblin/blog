---
title: Hello Cordova
date: 2016-12-15 18:51:31
tags: [Cordova]
---

## Mobile apps with HTML, CSS & JS

## Target multiple platforms with one code base

## Free and open source

### 1. Installing Cordova
Cordova command-line runs on Node.js and is available on NPM. Follow platform specific guides to install additional platform dependencies. Open a command prompt or Terminal, and type npm install -g cordova.

``` bash
$ npm install -g cordova npm install -g cordova
```

### 2. Create a project
Create a blank Cordova project using the command-line tool. Navigate to the directory where you wish to create your project and type cordova create <path>.

For a complete set of options, type cordova help create.

``` bash
$ cordova create MyApp cordova create MyApp
```

### 3. Add a platform
After creating a Cordova project, navigate to the project directory. From the project directory, you need to add a platform for which you want to build your app.

To add a platform, type cordova platform add <platform name>.

For a complete list of platforms you can add, run cordova platform.

``` bash
$ cd MyApp

$ cordova platform add browser
```
cd MyApp cordova platform add browser

### 4. Run your app
From the command line, run cordova run <platform name>.

``` bash
$ cordova run browser
```