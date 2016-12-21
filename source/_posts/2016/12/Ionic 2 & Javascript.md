---
title: Ionic 2 & JavaScript
date: 2016-12-21 21:09:16
tags: [Ionic 2, JavaScript]
---
[如何在Ionic2项目中使用第三方JavaScript库](http://www.jianshu.com/p/5f603f593917)

Ionic的官网放出一记大招[Ionic and Typings](http://blog.ionic.io/ionic-and-typings/)，来介绍如何在Ionic2项目中使用第三方JavaScript库。

因为在前阵子正好想用一个非常有名的第三方JS库[ChartJs](http://www.chartjs.org/)来实现一些东西，但我们的项目框架使用的是Ionic2,并且使用的是TypeScript来做开发语言的，所以当时想了很多的办法也没有很好地集成进来，最后还是使用了一个开源库[ng2-charts](https://github.com/valor-software/ng2-charts)来实现的。

看了[Ionic and Typings](http://blog.ionic.io/ionic-and-typings/)的教程，来总结一下方法吧，其实特别简单啦，如下

``` bash
npm install -g typings
typings search loads
typings install lodash --save
```
以上是教程中使用lodash的方法，现在我们来看一下，如果我想在自己的Ionic2项目中使用ChartJs的步骤吧。

1, 安装ChartJs库
``` bash
cd /项目的根目录下
npm install chart.js --save
```
2, 全局安装typings
``` bash
npm install -g typings
```
3, 咱们也搜一下有多少个chart.js的源啦
``` bash
typings search chart.js
```
``` bash
Viewing 2 of 2

NAME     SOURCE HOMEPAGE                               DESCRIPTION VERSIONS UPDATED
chart.js npm    https://www.npmjs.com/package/chart.js             1        2016-06-15T17:49:20.000Z
chart    dt     https://github.com/nnnick/Chart.js                 1        2016-03-16T15:55:26.000Z
```
4, 不错，有两个源，安装chart.js，这个看起来比较新
``` bash
typings install chart.js --save
```
基本的环境配置到这里搞定了

接下来看一下如何在项目中使用
1, 参考ChartJS的官网，在xxx.html创建一个Chart.

``` bash
<ion-content padding class="about">  
    <canvas id="myChart" width="400" height="400"></canvas>
</ion-content>
```
2, 在xxx.ts中引用Chart，并创建数据。如下

``` bash
import {Component} from '@angular/core';
import {NavController} from 'ionic-angular';
import * as ChartJs from 'chart.js'; // 导入chart.js
@Component({
    templateUrl: 'build/pages/about/about.html'
})
export class AboutPage {
    constructor(private navController:NavController) {

    }

    ionViewDidEnter() {
        var canvas = <HTMLCanvasElement> document.getElementById("myChart");
        var ctx = canvas.getContext("2d");  // 这里是关键, 不能写在constructor()中
        ChartJs.Line(ctx,{
            data: {
                labels: ["Red", "Blue", "Yellow", "Green", "Purple", "Orange"],
                datasets: [{
                    label: '# of Votes',
                    data: [12, 19, 3, 5, 2, 3],
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.2)',
                        'rgba(54, 162, 235, 0.2)',
                        'rgba(255, 206, 86, 0.2)',
                        'rgba(75, 192, 192, 0.2)',
                        'rgba(153, 102, 255, 0.2)',
                        'rgba(255, 159, 64, 0.2)'
                    ],
                    borderColor: [
                        'rgba(255,99,132,1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                        'rgba(255, 159, 64, 1)'
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                scales: {
                    yAxes: [{
                        ticks: {
                            beginAtZero:true
                        }
                    }]
                }
            }
        })
    }
}
```
OK! 大功告成~ 运行一下项目看一下喽~

参考
[Ionic and Typings](http://blog.ionic.io/ionic-and-typings/)
[TypeScript: problems with type system](http://stackoverflow.com/questions/13669404/typescript-problems-with-type-system)

文／思小言（简书作者）
原文链接：http://www.jianshu.com/p/5f603f593917
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。
