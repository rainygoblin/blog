---
title: Issues of Cordova&Ionic
date: 2016-12-15 20:29:20
updated: 2016-12-15 20:29:20
tags: [Cordova, Ionic]
---
### 1、No Content-Security-Policy meta tag found. Please add one when using the cordova-plugin-whitelist plugin.
解决办法
index.html 中添加
《meta http-equiv="Content-Security-Policy" content="default-src *; style-src 'self' 'unsafe-inline'; script-src 'self' 'unsafe-inline' 'unsafe-eval'"》

### 2、ionic 隐藏 nav-bar
文档上来看，需要在controller中调用  $ionicNavBarDelegate
例如 .controller('DashCtrl', function ($scope, $ionicNavBarDelegate) {
    $ionicNavBarDelegate.showBar(false);
})
但没起作用。最后在模板中使用标签方法
hide-nav-bar = "true"
该方法起作用。

### 3、 ng-model 与 input 值的问题
例如   input type="text" ng-model="querystr"
ng-model绑定到$scope.querystr 时，model会根据 text输入自动变化，但在controller中将
$scope.querystr=''时 ，text input的值不会产生变化。
并且在使用ng-model时，无法使用
angular.element(document).find(selector).val("some value");方式进行赋值。
解决方法是在controller 中，设置一个结构，如
$scope.queryMode ={querystr:''};
在input 中绑定  ng-model = "queryModel.querystr"
则controller中重设   $scope.queryMode.querystr = '';
绑定生效，input 清空。

### 4、cordova插件 ,在ripple中弹出错误窗口
如果安装cordova插件，如keyboard,statusbar 等， 会在ripple中抛各种错，并且每次加载会弹出窗口，让你写JSON回调， 这个是ripple的BUG，不支持自定义插件， 去掉弹窗的办法是在app.js上方，即定义angular.module（）的上方
写

var annoyingDialog = parent.document.getElementByIdx_x_x_x_x_x_x_x_x_x_x_x_x_x_x_x('exec-dialog');
上句中的x_x 去掉，新浪自动添加的。
if (annoyingDialog) annoyingDialog.outerHTML = "";
弹窗将不再出现，但输出窗口中的ripple.js中的错误仍会，你可以过滤，也可以不管。

 
### 5、使用极光推送，按照github上的说明文档，加入插件后总是无法编译，查看
是由于 platform/android/AndroidManifest.xml  中
meta-data android:name=JPUSH_APPKEY android:value=aaabbbbmn9QITAv0Oe
重复，每次build总会生成两条键值
经查在plug/android.json 中，有两条重复生成的语句，删掉一个，即可。

### 6、ionic关于IFAME的调用
因为使用第三方接口，会造成跑到应用外的地址，ionic无法控制外部连接回到应用，因此一般使用iframe方式
打开外部连接，并加一个headbar用来返回到应用，如在modal中打开外部地址
如html中
 《script id="login.html" type="text/ng-template"》
        《ion-modal-view》
            《ion-header-bar align-title="center" class="bar-positive"》
                《div class="buttons"》
                    《button  class="button button-clear button-icon icon ion-ios-arrow-back"》《/button》
                    《button ng-click="closeModal()" class="button button-clear button-icon icon "》取消《/button》
                《/div》
                《h1 class="title"》LOGIN《/h1》
            《/ion-header-bar》
            《ion-content scroll="true" class="has-header no-padding"》
                《iframe id="ifmr2" data-tap-disabled="true" ng-src="{{chatStru.paySrc}}"》《/iframe》
            《/ion-content》
        《/ion-modal-view》
《/script》

controller中
$scope.tt = new Date().getMilliseconds();
$scope.chatStru = {
        paySrc: $sce.trustAsResourceUrl('http://xxx.om/testlogin.jsp?tt='+$scope.tt),
        token: "",
        hasToken: false
    };
$ionicModal.fromTemplateUrl('login.html', {
        scope: $scope,
        animation: 'slide-in-up'
    }).then(function (modal) {
            $scope.modal = modal;
    });

    $scope.openModal = function () {
          $scope.modal.show();
    };
     $scope.closeModal = function () {
        $scope.modal.hide();
    };

但这样的问题是在于，页面完成后，你必须通过手动点击返回，并且无法进行数据的传递，这个在html5中其实已经有了非常完美的解决方法就是postMessage()
在远端的被iframe的页面中，加入JS
window.onload=function(){
             window.parent.postMessage('logined','*');
        }

并在inoic的controller中加入
 window.addEventListener('message', function (e) {
        var data = e.data;
//这里返回的是 logined,相当于传递参数回来。
        $scope.modal.hide();
    }, false);

  你会发现，你即接收到了data，同时又关闭了该modal ，而不需要手动关闭。
当然，你可以将 addEventListener() 放到 $scope.openModal 
然后在 $scope.closeModal 中 removeEventListener()
如
 $scope.openModal = function () {
        var tt = new Date().getMilliseconds();
        $scope.chatStru.paySrc = $sce.trustAsResourceUrl('http://xxx.cn/oe/testlogin.jsp?tt=' + tt);
        $scope.modal.show();
        window.addEventListener('message', function (e) {
            var data = e.data;
            $scope.chatStru.hasToken = true;
            $scope.modal.hide();
 
        }, false);
    };
    // function to close the modal
    $scope.closeModal = function () {
        $scope.modal.hide();
        if ($scope.chatStru.hasToken) {
 
            window.removeEventListener('message', function () { }, false);
        }
    };

最终可以这样写
 var handel = function (e) {
        var data = e.data;
        if (data.id > 0) {  // 传回来的为json{id:1,msg:'aaa'}
            if (!$scope.chatStru.hasToken) {
                $scope.chatStru.hasToken = true;
            }
            $scope.modal.hide();
        } else {
            $scope.chatStru.hasToken = false;
        }
    };
 
    $scope.openModal = function () {
        var tt = new Date().getMilliseconds();
        $scope.chatStru.paySrc = $sce.trustAsResourceUrl('http://xxx.cn/oe/testlogin.jsp?tt=' + tt);
        $scope.modal.show();
        if (!$scope.chatStru.hasToken) {
            window.addEventListener('message', handel, false);
        }
    };
    
    $scope.closeModal = function () {
        $scope.modal.hide();
        if ($scope.chatStru.hasToken) {
            window.removeEventListener('message', handel, false);
        }
    };

### 7、使用android studio 运行cordova项目
直接使用platfrom目录里的gradle 可以在android studio中直接导入 cordova的项目，但运行模拟器时出现
Cannot reload AVD list: cvc-enumeration-valid: Value '280dpi' is not facet-valid with respect to enumeration '[ldpi, mdpi, tvdpi, hdpi, xhdpi, 400dpi, xxhdpi, 560dpi, xxxhdpi]'. It must be a value from the enumeration.
Error parsing D:\sdkforas\android-sdk-windows\system-images\android-22\android-wear\armeabi-v7a\devices.xml
cvc-enumeration-valid: Value '280dpi' is not facet-valid with respect to enumeration '[ldpi, mdpi, tvdpi, hdpi, xhdpi, 400dpi, xxhdpi, 560dpi, xxxhdpi]'. It must be a value from the enumeration.
Error parsing D:\sdkforas\android-sdk-windows\system-images\android-22\android-wear\x86\devices.xml
解决方法 ：
用/sdk/tools/lib/devices.xml去替换 system-images\android-22\android-wear\x86\devices.xml和system-images \android-22\android-wear\armeabi-v7a\devices.xml中的devices.xml


### 8、 android studio 中 gradle 失败
gradle project sync failed.Basic functionality(e.g.editing,debugging) will not work properly.
解决方法：android studio中，点击 tools ->Android->sync project with gradles files.

### 9、加载远程js或css 出现 Refused to load the script  或 Refused to load the stylesheet
because it violates the following ....
例如 index.html中 加载字体
《link href='http://fonts.useso.com/css?family=RobotoDraft:400,500,700,400italic' rel='stylesheet'》
可在index.html添加安全许可
《meta http-equiv="Content-Security-Policy" content="default-src *; style-src 'self' 'unsafe-inline' http://fonts.useso.com ; script-src 'self' 'unsafe-inline' 'unsafe-eval';"》
直接加根域名即可，另外不要带引号 js  需要添加在 script-src 中。

### 10 、 ionic 中 弹出键盘遮挡住输入框
在config.xml 中修改全屏为FALSE并添加  adjustPan  (adjstResize没有成功)
 《preference name="Fullscreen" value="False" /》
  《preference name="android-windowSoftInputMode" value="adjustPan"/》

### 11、ionic监听滚动
网上的示例xxx.bind('scroll',function(){.....}),很容易将页面跑死， 换个思路，使用监听touch
var targetPos = window.screen.availHeight;
 $("#scdiv").on("touchend", function () {
if (currpos > 10 && currpos < targetPos / 4) {
                $ionicScrollDelegate.$getByHandle('homescroll').scrollTo(0, 0, true);
                      return false;
            } else if (currpos >= targetPos / 4 && currpos <= targetPos - 10) {
                $ionicScrollDelegate.$getByHandle('homescroll').scrollTo(0, targetPos - 64, true);
                       return false;
            }
})
当触摸结束时，判断当前的位移，来做一些操作。这样在性能上提高了许多。
$ionicScrollDelegate.$getByHandle 操作的是在html中定义的delegate-handle
如《ion-content id="scdiv" delegate-handle="homescroll"》

### 12、 APP开启检测网络并提示开启
需要三个插件
1、https://github.com/apache/cordova-plugin-network-information
2、https://github.com/apache/cordova-plugin-dialogs
3、https://github.com/deefactorial/Cordova-open-native-settings

然后在APP.JS中，divicesReady中
$ionicPlatform.ready(function () {
if (navigator.connection) {
            var tmptypes = navigator.connection.type;
            if (tmptypes.toUpperCase().indexOf('NONE') > -1 || tmptypes.toUpperCase().indexOf('UNKNOWN') > -1) {
                if (navigator.notification) {
                    navigator.notification.confirm(
                    '您的设备未开启网络',
                       function (buttonIndex) {
                             if (buttonIndex == 1) {
                               if (cordova.plugins.settings) {
                                   cordova.plugins.settings.openSetting("wifi", function () { console.log("network setting openning"); }, function () { console.log("open network setting failed"); });
                               }
                           }
                         },            // callback to invoke with index of button pressed
                       '提示',           // title
                       ['开启', '取消']     // buttonLabels
                               );
                }
            }
        }
}）

其中openSettings中可设置以下本地设置
var settingNames = array(
    "open",
    "accessibility",
    "add_account",
    "airplane_mode",
    "apn",
    "application_details",
    "application_development",
    "application",
    "bluetooth",
    "captioning",
    "cast",
    "data_roaming",
    "date",
    "device_info",
    "display",
    "dream",
    "home",
    "input_method",
    "input_method_subtype",
    "internal_storage",
    "locale",
    "location_source",
    "manage_all_applications",
    "manage_applications",
    "memory_card",
    "network_operator",
    "nfcsharing",
    "nfc_payment",
    "nfc_settings",
    "print",
    "privacy",
    "quick_launch",
    "search",
    "security",
    "settings",
    "show_regulatory_info",
    "sound",
    "sync",
    "usage_access",
    "user_dictionary",
    "voice_input",
    "wifi_ip",
    "wifi",
    "wireless");
其中相关网络的为wifi  移动数据开启未找到。谁试出了是哪个请告诉我

### 13、$sate.go  和 $stateParams 传参及收参
在 controller1 中使用  $state.go('statename',{id:1}) ;传递参数
在 statename 相对应的  controller2 中接收参数
$scope.id = $stateParams.id;
注意，此处必须在router.js 中设置 statename 的参数形式
如.state('statename ', {
    url: '/statename ',
    params: { 'id': null },
    templateUrl: 'templates/Users/statename .html',
    controller: 'controller2 '
})
其中params: { 'id': null }, 对应$state传的参，若不设，则必为undefined

### 14、关于ionic中想底部加一长按钮，随页面滚动位置不变，例如


最初的方案是在 ion-content  外 ion-view 内添加一层
div .....style= ..... position:fixed    bottom:0px;之类，但最终发现，在不同分辨率下位置并不正确
基本可确定是因为statusbar等的缘故，仔细看了下ionic.css最终发现，太简单了
div class="tabs"  就可以了。

### 15、php中取值问题
ionic中用的$http  method:post ，params........在PHP中用$_POST取不到值， 改成 $_Request 就行了。

### 16、使用vs2015 release
会提示使用发布配置进行调试时，Android 程序包必须已签名。要配置 Android 签名，请按照 http://go.microsoft.com/fwlink/?LinkID=613579 中的说明操作（该网页很难打开）
实际需要在项目根目录的build.json中添加生成的keystore
{
     "android": {
      "release": {
        "keystore": "E:\\Projects\\android.keystore",
        "storePassword": "*******",
        "alias": "********",
        "password": "*******",
        "keystoreType": ""
      }
     }
 }
至于生成keystore 请百度，只要有jdk 运行命令就OK

### 17、ionic 的滚动优化
在app.js中的config 块中，加入
$ionicConfigProvider.scrolling.jsScrolling(false);
默认所有的滚动使用native，会比js的滚动快很多，并且很平滑，
但这样做的话，无法使用一些效果，如has-bouncing="true"（仿苹果的一种上下拉显示背景的弹性效果）
那你可以在特定的view中，ion-content中加入overflow-scroll="false"，则该view保持js滚动

### 18、微信支付
支付宝支付非常简单，文档也很丰富，微信的相对来说低了不止一个档次，当然最郁闷的一件事是
经过了数天的查错，发现微信的totalfee 居然是以分为单位，换句话来说，你传价格时，只会有整数
如果出现0.01，那必然出错。