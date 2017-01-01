---
title: Hello Promise Observable
date: 2016-12-30 22:42:40
tags: [Javascript, Promiss, Observable]
---

[Promise VS Observable](http://stackoverflow.com/questions/37364973/angular-2-promise-vs-observable)


A Promise handles a single event when an async operation completes or fails.

Observable

An Observable is like a Stream (in many languages) and allows to pass zero or more events where the callback is called for each event.

Often Observable is preferred over Promise because it provides the features of Promise and more. With Observable it doesn't matter if you want to handle 0, 1, or multiple events. You can utilize the same API in each case.

Observable also has the advantage over Promise to be cancelable. If the result of an HTTP request to a server or some other expensive async operation isn't needed anymore, the Subscription of an Observable allows to cancel the subscription, while a Promise will eventually call the success or failed callback even when you don't need the notification or the result it provides anymore.

Observable provides operators like map, forEach, reduce, ... similar to an array

There are also powerful operators like retry(), or replay(), ... that are often quite handy.
