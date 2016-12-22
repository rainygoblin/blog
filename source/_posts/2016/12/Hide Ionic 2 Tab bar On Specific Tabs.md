---
title: Hide Ionic 2 Tab bar On Specific Tabs
date: 2016-12-22 21:57:16
tags: [Ionic 2, JavaScript]
---
[Hide Ionic 2 Tab bar On Specific Tabs](http://pointdeveloper.com/hide-ionic-2-tab-bar-specific-tabs/)

So by default the tab bar is displayed at the top or the bottom and when any one of the tab is clicked the view is navigated to it, but the tab bar is still displayed. There can be a use case where you would want to hide the tab bar when a specific tab is clicked.

hide-ionic-2-tab-bar

Before going further let me tell you that instead of hiding the tab bar you should focus on making changes to your navigation flow so that you are showing a modal or another page instead of hiding the tab bar.

Having said that, hiding the tab bar is easy so let’s jump right into it.

## Step 1)

We will start off by creating a new project. Make sure that you are creating the app using the tabs template. To do that we will run the following command

``` bash
ionic start hideTabs tabs --v2
cd hideTabs
```
That’s the only command we will need to execute in the tutorial.





## Step 2)

The default app has three tabs in the tab bar by default, namely home, about and contact. Let’s say that we want to hide the tab bar when the user navigates to the about tab. For that, we will need to make changes to the about.ts file which you can find at
``` bash
 src\pages\about\about.ts
```
Open the file and put the following code in it.

``` bash
src \pages\about\about.ts

import { Component } from \'@angular/core\';
import { NavController } from \'ionic-angular\';

@Component({
  selector: \'page-about\',
  templateUrl: \'about.html\'
})
export class AboutPage {
  tabBarElement: any;
  constructor(public navCtrl: NavController) {
    this.tabBarElement = document.querySelector(\'.tabbar.show-tabbar\');
  }

  ionViewWillEnter() {
    this.tabBarElement.style.display = \'none\';
  }

  ionViewWillLeave() {
    this.tabBarElement.style.display = \'flex\';
  }
}
```


Let’s understand what we are doing here. First of all, we are getting the tab bar element and storing it a variable called tabBarElement. Then we are hooking into the NavVontrollers lifecycle hooks. You can read more on lifecycle events here.

The ionViewWillEnter() method will be called when the view is about to be shown so we are hiding the tab bar by doing this.tabBarElement.style.display = 'none';.

Similarly, we want to unhide the tab bar element when the view is about to leave, we so that using ionViewWillLeave() and we set the display property to flex by doing  this.tabBarElement.style.display = 'flex';

By doing just this we are hiding the tab bar effectively,you can stop right here the next step is optional.

## Step 3)

We have successfully hidden the tab bar but there is one problem we no longer have any way to go back from the about tab because we have hidden the tab bar. The hardware back button will work but what if you want to have a back button in the navbar.

We can add a back button manually that will show up next to the page title of the about page. We add the button in the about.html file as follows


``` bash
src\pages\about\about.html

<ion-header>
  <ion-navbar>
   <button ion-button icon-only menuToggle (click)="takeMeBack()">
      <ion-icon name="arrow-back"></ion-icon>
    </button>
    <ion-title>
      About
    </ion-title>
  </ion-navbar>
</ion-header>

<ion-content padding>
  This is About Page Tab bar is Hidden.
</ion-content>
```
As shown we are adding the button with arrow-back icon. Notice that the button has menuToggle attribute applied to it. This will prevent the button to have any color. Also we are hooking up the takeMeBack() function to the click event of the button.



## Step 4)

We have created the button, now it’s time to add the click functionality to it. We do that by adding the following to the about.ts file.

``` bash
import { Component } from '@angular/core';
import { NavController } from 'ionic-angular';

@Component({
  selector: 'page-about',
  templateUrl: 'about.html'
})
export class AboutPage {
  tabBarElement: any;
  constructor(public navCtrl: NavController) {
    this.tabBarElement = document.querySelector('.tabbar.show-tabbar');
  }

  ionViewWillEnter() {
    this.tabBarElement.style.display = 'none';
  }

  ionViewWillLeave() {
    this.tabBarElement.style.display = 'flex';
  }

  takeMeBack() {
    this.navCtrl.parent.select(0);
  }

}
```
As tabs have a zero-based index,  we are navigating to the first tab by passing 0 in this.navCtrl.parent.select(0);.



And just like that, we have hidden the tab bar on a specific tab.

## Conclusion

As you saw hidden tab bar for specific tabs is really simple. But as I mentioned you would be better off creating a separate page or a modal rather than hiding the tabs. Also, make sure that you are handling the history of the tabs properly because each tab under the tab bar has it’s own navigation stack that needs to be dealt with. But know how to hide tab bar is one of those tricks that is handy to have around
