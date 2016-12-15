---
title: Hello Ionic
date: 2016-12-15 17:42:50
tags: [Ionic 2, Cordova, AngularJS 2]
---

## Installing Ionic
[Ionic](http://ionicframework.com/) 2 apps are created and developed primarily through the Ionic command line utility (the “CLI”), and use Cordova to build and deploy as a native app. This means we need to install a few utilities to get developing.

### Ionic CLI and Cordova
To create Ionic 2 projects, you’ll need to install the latest version of the CLI and Cordova. Before you do that, you’ll need a recent version of Node.js. Download the installer for Node.js 6 or greater and then proceed to install the Ionic CLI and Cordova for native app development:

``` bash
$ npm install -g ionic cordova
```

You may need to add “sudo” in front of these commands to install the utilities globally

Once that’s done, create your first Ionic app:

``` bash
$ ionic start cutePuppyPics --v2
```

Omit –v2 if you’d like to use Ionic 1. To run your app, cd into the directory that was created and then run the ionic serve command to test your app right in the browser!

``` bash
$ cd cutePuppyPics
$ ionic serve
```