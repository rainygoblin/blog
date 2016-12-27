---
title: Hello Hammerjs
date: 2016-12-27 22:46:19
tags: [Hammerjs, TypeScript]
---

Refer [Hammer.js](https://www.npmjs.com/package/hammerjs)

``` bash
// get a reference to an element
var stage = document.getElementById('stage');

// create a manager for that element
var mc = new Hammer.Manager(stage);

// create a recognizer
var Rotate = new Hammer.Rotate();

// add the recognizer
mc.add(Rotate);

// subscribe to events
mc.on('rotate', function(e) {
    // do something cool
    var rotation = Math.round(e.rotation);    
    stage.style.transform = 'rotate('+rotation+'deg)';
});
```
An advanced demo is available here: http://codepen.io/runspired/full/ZQBGWd/


TypeScript [@types/hammerjs](https://www.npmjs.com/package/@types/hammerjs)


## Installation
``` bash
npm install --save @types/hammerjs
```
