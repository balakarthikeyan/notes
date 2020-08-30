### Add the following values to the Path system variable in Environment Variables:

> **Variable:** `ANDROID_HOME` **Value:** ` C:\Users\balakarthikeyan.a\AppData\Local\Android\Sdk`

> **Variable:** `ANT_HOME` **Value:** `C:\apache-ant`

> **Variable:** `JAVA_HOME` **Value:** `C:\Program Files\Java\jdk1.8.0_131`

### Path Variable
`%JAVA_HOME%\bin;%ANT_HOME%\bin;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools;`

### via Command prompt
```
setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
setx ANT_HOME "C:\apache-ant"
setx ANDROID_HOME "C:\Users\balakarthikeyan.a\AppData\Local\Android\Sdk"
setx PATH "%PATH%;%JAVA_HOME%\bin;%ANT_HOME%\bin;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools;";
setx PATH "%ANT_HOME%\bin;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools;"
```
### To setup project
```
npm -v
npm install -g npm@latest / npm update -g npm
npm config ls -l
npm install -g cordova
npm info cordova version
npm install -g phonegap
npm info phonegap version

phonegap -v
phonegap analytics off

cordova create hello
cordova platform ls
cordova platform add android --save
cordova platform update android --save
cordova build
cordova build ios
cordova emulate android
cordova plugins
cordova platform rm android
cordova platform rm ios
cordova platform rm browser
cordova platform add android
cordova platform add ios
cordova platform add browser
cordova plugin add com.telerik.plugins.nativepagetransitions
cordova plugin add cordova-plugin-android-fingerprint-auth
cordova plugin add cordova-plugin-camera
cordova plugin add cordova-plugin-compat
cordova plugin add cordova-plugin-contacts
cordova plugin add cordova-plugin-dialogs
cordova plugin add cordova-plugin-globalization
cordova plugin add cordova-plugin-inappbrowser
cordova plugin add cordova-plugin-mfp
cordova plugin add cordova-plugin-mfp-push
cordova plugin add cordova-plugin-okhttp
cordova plugin add cordova-plugin-spinnerdialog
cordova plugin add cordova-plugin-touch-id
cordova plugin add cordova-plugin-x-socialsharing
cordova plugin add es6-promise-plugin
cordova plugin add com.filfatstudios.spinnerdialog

<allow-intent href="geo:*" />
<allow-intent href="*" />

npm install -g typescript
npm install -g cordova ionic
npm install -g ionic@latest

ionic start myApp blank
ionic start myApp tabs
ionic start myApp sidemenu
ionic start myAwesomeApp
ionic info
ionic serve
```
