---
title: jQuery document ready vs self calling anonymous function
date: 2016-12-25 14:39:16
tags: [jQuery, ready, self calling]
---
[jQuery document ready vs self calling anonymous function](http://stackoverflow.com/questions/3259496/jquery-document-ready-vs-self-calling-anonymous-function)

### $(document).ready(function(){ ... }); or short $(function(){...});

This Function is called when the DOM is ready which means, you can start to query elements for instance. .ready() will use different ways on different browsers to make sure that the DOM really IS ready.

$(document).ready() will be executed once the document is loaded.  $(function(){...}); is a shortcut for $(document).ready() and does the exact same thing.

### (function(){ ... })();

That is nothing else than a function that invokes itself as soon as possible when the browser is interpreting your ecma-/javascript. Therefor, its very unlikely that you can successfully act on DOM elements here.

(function(){...})(); will be executed as soon as it is encountered in the Javascript.


The following code will be executed when the DOM (Document object model) is ready for JavaScript code to execute.

``` bash
$(document).ready(function(){
  // Write code here
});
```
The short hand for the above code is:
``` bash
$(function(){
  // write code here
});
```
The code shown below is a self-invoking anonymous JavaScript function, and will be executed as soon as browser interprets it:
``` bash
(function(){
  //write code here
})();   // It is the parenthesis here that call the function.
```
The jQuery self-invoking function shown below, passes the global jQuery object as an argument to function($). This enables $ to be used locally within the self-invoking function without needing to traverse the global scope for a definition. jQuery is not the only library that makes use of $, so this reduces potential naming conflicts.
``` bash
(function($){
  //some code
})(jQuery);
```
