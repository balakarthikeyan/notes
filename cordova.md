### Add the following values to the Path system variable in Environment Variables:

> **Variable:** `ANDROID_HOME`
> **Value:** ` C:\Users\balakarthikeyan.a\AppData\Local\Android\Sdk`

> **Variable:** `ANT_HOME` 
> **Value:** `C:\apache-ant`

> **Variable:** `JAVA_HOME`
> **Value:** `C:\Program Files\Java\jdk1.8.0_131`

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
