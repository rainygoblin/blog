---
title: Hello AxISS
date: 2016-12-28 14:42:40
tags: [AxISS
---

## AxISS Command

### Create new Project
``` bash
ionic start --v2 axiss tabs
```

### Run Project
``` bash
ionic serve
```

### Create new Page
``` bash
ionic g page login
ionic g page patient
ionic g page case_
ionic g page caseDetail
ionic g page exam
ionic g page examDetail
```

### Create new Service
``` bash
ionic g provider AccountResource
```

### Generate the app
``` bash
ionic platform add ios
ionic platform add android
ionic platform add browser
ionic platform rm ios
ionic platform rm android
ionic platform rm browser
ionic build android
ionic build ios
```

### run on the phone
``` bash
ionic run android
```

### use the crosswalk
``` bash
ionic plugin add cordova-plugin-crosswalk-webview --save
```

### add the chart.js
``` bash
npm install chart.js --save
npm install -g typings
typings search chart.js
typings install chart.js --save
```

### add the hammerjs
``` bash
npm install @types/hammerjs --save
```

### add the typescript logging
``` bash
npm install typescript-logging --save
```

### add add the [angular2-jwt](https://github.com/auth0/angular2-jwt/)
``` bash
npm install angular2-jwt

import { AuthHttp, AuthConfig } from 'angular2-jwt';
import { Http } from '@angular/http';
import { Storage } from '@ionic/storage';

let storage = new Storage();

export function getAuthHttp(http) {
  return new AuthHttp(new AuthConfig({
    headerPrefix: YOUR_HEADER_PREFIX,
    noJwtError: true,
    globalHeaders: [{'Accept': 'application/json'}],
    tokenGetter: (() => storage.get('id_token')),
  }), http);
}

@NgModule({
  imports: [
    IonicModule.forRoot(MyApp),
  ],
  providers: [
    {
      provide: AuthHttp,
      useFactory: getAuthHttp,
      deps: [Http]
    },

  ...

  bootstrap: [IonicApp],

  ...
})
```

### CORS  
uncomment the application.yml  cors  src/main/resources/application.yml
``` bash
    cors: # By default CORS is disabled. Uncomment to enable.
        allowed-origins: "*"
        allowed-methods: GET, PUT, POST, DELETE, OPTIONS
        allowed-headers: "*"
        exposed-headers:
        allow-credentials: true
        max-age: 1800
```

### RxJS Observable
``` bash
Observable.create
```

### Angular2 PDF Viewer
[Angular2 PDF Viewer](https://www.npmjs.com/package/ng2-pdf-viewer)
``` bash
npm install ng2-pdf-viewer --save
```

### Display image and videos
``` bash
https://devdactic.com/images-videos-fullscreen-ionic/
```
