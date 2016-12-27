---
title: changing locking screen orientation in ionic application
date: 2016-12-26 12:39:16
tags: [Ionic 2, orientation]
---
[Changing & Locking Screen Orientation In Ionic Application](http://www.gajotres.net/changing-locking-screen-orientation-in-ionic-application/)

PREPARATION


Before we can start make sure you have everything configured correctly.

You will need to have these:

Android Environment (or iOS if your working on a MacOS)
nodeJS
Ionic
Cordova
In case you don’t have a properly configured development environment take a look at this article: Ionic Framework | Installation Guide.
1. UPDATE IONIC & CORDOVA


Don’t forget to update Ionic and Cordova, older versions may not work with this tutorial:

1
npm update -g cordova ionic

2. CREATE A NEW PROJECT



ionic start IonicOrientationDemo blank
cd IonicOrientationDemo

Warning: Since a few of my readers never worked with Ionic CLI. From this point and further, every time I tell you to execute something, do that inside a project folder.

At this point, we need to upgrade IonicOrientationDemo project with an appropriate platform. Because I’m using Windows we’re going to create an Android application:

1
ionic platform add android

if you’re a MacOS user, create an iOS application like this:
1
ionic platform add ios

3. INSTALL SCREEN ORIENTATION PLUGIN


Next, we need to import and install screen orientation plugin into a newly created project:

1
cordova plugin add net.yoik.cordova.plugins.screenorientation



GITHUB


DEVELOPMENT


At this point, you should have everything set up and ready, we can begin.

Go to IonicOrientationDemo project directory and open main index.html file, add a controller to ion-content directive, like this:

1
2
3
4
5
6
7
<ion-pane>
    <ion-header-bar class="bar-stable">
      <h1 class="title">Ionic Blank Starter</h1>
    </ion-header-bar>
    <ion-content ng-controller="MainCtrl">
    </ion-content>
  </ion-pane>

Initialize this controller in app.js file:

1
2
3
app.controller('MainCtrl', function($scope) {

});

Now add these two buttons:

1
2
<button class="button button-full button-positive " ng-click="changeOriantationLandspace()">Change To Landspace Mode</button><br/>
<button class="button button-full button-positive " ng-click="changeOriantationPortrait ()">Change To Portrait Mode</button><br/>

And for a final touch, we’re going to change page orientation depending on a selected button:

1
2
3
4
5
6
7
8
9
10
11
12
13
app.controller('MainCtrl', function($scope) {
    document.addEventListener("deviceready", onDeviceReady, false);
    function onDeviceReady()
    {
        $scope.changeOriantationLandspace = function() {
            screen.lockOrientation('landscape');
        }

        $scope.changeOriantationPortrait = function() {
            screen.lockOrientation('portrait');
        }   
    }
});

This much code should be enough for a skeleton application, just a bare minimum. In the next chapter, I will show you how this application looks.

Our final application should look like this:

HTML

Application main page (index.html).

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8"/>
        <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no, width=device-width"/>
        <title></title>

        <link href="lib/ionic/css/ionic.css" rel="stylesheet"/>
        <link href="css/style.css" rel="stylesheet"/>

        <!-- ionic/angularjs js -->
        <script src="lib/ionic/js/ionic.bundle.js"></script>

        <!-- cordova script (this will be a 404 during development) -->
        <script src="cordova.js"></script>

        <!-- your app's js -->
        <script src="js/app.js"></script>
    </head>
    <body ng-app="starter">

        <ion-pane>
            <ion-content ng-controller="MainCtrl" padding="true">
                <button class="button button-full button-positive " ng-click="changeOriantationLandspace()">Change To Landspace Mode</button><br/>
                <button class="button button-full button-positive " ng-click="changeOriantationPortrait ()">Change To Portrait Mode</button><br/>
            </ion-content>
        </ion-pane>

    </body>
</html>

JAVASCRIPT

Application JavaScript file (app.js).

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
// Ionic Starter App

// angular.module is a global place for creating, registering and retrieving Angular modules
// 'starter' is the name of this angular module example (also set in a <body> attribute in index.html)
// the 2nd parameter is an array of 'requires'
var app = angular.module('starter', ['ionic'])

.run(function($ionicPlatform) {
  $ionicPlatform.ready(function() {
    // Hide the accessory bar by default (remove this to show the accessory bar above the keyboard
    // for form inputs)
    if(window.cordova && window.cordova.plugins.Keyboard) {
      cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
    }
    if(window.StatusBar) {
      StatusBar.styleDefault();
    }
  });
});

app.controller('MainCtrl', function($scope) {
    document.addEventListener("deviceready", onDeviceReady, false);
    function onDeviceReady()
    {
        $scope.changeOriantationLandspace = function() {
            screen.lockOrientation('landscape');
        }

        $scope.changeOriantationPortrait = function() {
            screen.lockOrientation('portrait');
        }   
    }
});

A warning, if you want to change a first page orientation (during the initial initialization), make sure to have a splash screen. Cordova device ready event may or may not trigger before Ionic application is active, so there’s a good chance application will start in portrait mode then visibly switch to landscape mode.

DEPLOYMENT

Next step, we need to build our application:



1
ionic build android

Be careful here, this step may break if you’re behind a firewall. The first execution will take a long time, so be patient.

When this step ends, look at the output log, last two lines should look something like this:

1
2
Built the following apk(s):
    D:\Development\IonicProject\platforms\android\build\outputs\apk\android-debug.apk

We’ll use the last line location to deploy our application. Make sure your smartphone is prepared to accept an incoming application. In case of Android platform, you must enable Developer Options and USB debugging.

Do this:

1
adb install -r platforms\android\build\outputs\apk\android-debug.apk
