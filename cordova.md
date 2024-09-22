### Add the following values to the Path system variable in Environment Variables:

> **Variable:** `ANDROID_HOME` **Value:** ` C:\Users\Balakarthikeyan\AppData\Local\Android\Sdk`

> **Variable:** `ANT_HOME` **Value:** `C:\apache-ant`

> **Variable:** `JAVA_HOME` **Value:** `C:\Program Files\Java\jdk-1.8`

### Path Variable
`%JAVA_HOME%\bin;%ANT_HOME%\bin;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\build-tools;`

### via Command prompt
```
setx JAVA_HOME "C:\Program Files\Java\jdk-1.8"
setx ANT_HOME "C:\apache-ant"
setx ANDROID_HOME "C:\Users\Balakarthikeyan\AppData\Local\Android\Sdk"
setx PATH "%PATH%;%JAVA_HOME%\bin;%ANT_HOME%\bin;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\build-tools;";
setx PATH "%JAVA_HOME%\bin;%ANT_HOME%\bin;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\build-tools;"
```
### To setup project
```
npm -v
npm install -g npm@latest
npm update -g npm
npm config ls -l
npm install -g express
npm install -g cordova
npm info cordova version
npm install -g phonegap
npm info phonegap version
npm install -g grunt-cli

node .help
node .exit

phonegap -v
phonegap analytics off

npm update -g cordova
cordova create helloworld com.bala.helloworld HelloWorld
cd helloworld
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
cordova plugin add cordova-plugin-device
cordova plugin add cordova-plugin-console
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
```

### Ionic Prerequisites
```
npm install -g typescript
npm install -g cordova ionic
npm install -g ionic@latest
npm install -g ionic@3.3.0

npm install -g typescript
npm install --save @angular/material
npm install --save angular2
npm install --save mongodb

npm install --save @angular/core@4.1.0 
npm install --save @angular/compiler 
npm install --save @angular/common 
npm install --save @angular/platformbrowser 
npm install --save @angular/platform-browser-dynamic rxjs reflect-metadata zone.js

ionic start myApp blank/tabs/sidemenu
ionic start myApp
ionic info
ionic serve
ionic -version
```
### Ionic bundle script
```
<script src="http://code.ionicframework.com/nightly/js/ionic.bundle.js"></script>
```

Angular is a platform for building web applications. 
Angular is an open-source front-end framework that empowers developers to create dynamic, single-page applications using HTML and TypeScript.

Ionic, is a popular open-source framework for building cross-platform mobile apps using familiar web technologies such as HTML, CSS, and JavaScript. It leverages Angular for building the application logic, while providing a library of pre-designed components, gestures, and tools for developing fast, highly interactive apps.

Setting Up the Environment

npm install -g @ionic/cli
npm install -g @angular/cli

Creating a New Ionic and Angular Project

ionic start myApp tabs --type=angular

Understanding the Project Structure

src: This is where you’ll spend most of your development time. It contains the app’s source code, including Angular components, templates, styles, and images.
node_modules: This folder contains all the npm packages necessary for your project.
www: This folder contains the built version of your application.
app: This folder contains all the Angular components, modules, and services for your app. Each component consists of a TypeScript file (.ts), a template file (.html), and a style file (.scss).
assets: This folder is for storing images, icons, and other static files.
environments: This folder is for setting different environment variables, like API endpoints.

## Ionic Cli
npm install -g @ionic/cli
ionic -version
ionic info

## Update old Ionic Project
```
npm uninstall -g ionic --force
npm uninstall -g @ionic/cli --force

npm install -g @ionic/cli
npm install -g @angular/cli
```

## Update Angular in Ionic Project
```
ng update @angular/cli@18 @angular/core@18 --allow-dirty
ionic cordova prepare android
ionic cordova run android
npm run watch
```
## Latest Ionic setup 
```
ionic start modernApp blank --type angular
npm install @ionic/angular@latest --save
ng add @ionic/angular
```

- Go to your new project: cd .\MyIonicApp
- Run ionic serve within the app directory to see your app in the browser
- Run ionic capacitor add to add a native iOS or Android project using Capacitor
- Generate your app icon and splash screens using cordova-res --skip-config --copy

```console
ionic generate page
ionic generate component
ionic generate service
ionic generate module
ionic generate class
ionic generate directive
ionic generate guard
```

```
ionic g service services/movie
ionic g page details
```

## Create both the iOS and Android projects:

```
ionic cap add ios
ionic cap add android
```
ionic build that updates your web directory (default: www), you'll need to copy those changes into your native projects:
```
ionic cap copy
After making updates to the native portion of the code
ionic cap sync
ionic cap open android
```

## Run and Test the Ionic/Angular Apps

### Add Capacitor to the Project
```
npm install @capacitor/cli @capacitor/core
npm install @capacitor/android
npx cap init myAppName com.example.myapp
npx cap add android
npx cap open android
npx cap run android

ionic capacitor add android
ionic capacitor build android
ionic capacitor open android
ionic capacitor run android
```

### For Android:

Open your Android Studio project and select the "myAppName" target
Select the "myAppName" target and press the "Run" button to start debugging on an Android emulator or an Android device.

### For iOS:

Open your Xcode project and select the "myAppName" target
Select the "myAppName" target and go to "Signing & Capabilities"
Select your development team, then press "Run" to start debugging on an iOS simulator or an iOS device.

First Install the Intel x86 Emulator Accelerator HAXM from the Android SDK manager next create a device in genymotion, Once its done, go to settings and then go to the ADB tab and select use genymotion android tools. Once done, run this command: ionic run android, so that ionic can detect the device created using genymotion. If you use ionic emulate android then ionic will not recognise your device.

`ionic cordova run <ios or android> --device -l --debug`

For Android open Chrome and go to Web Inspector.

`Open ⠇> More tools > Remote devices`

Select your device and click Inspect.

Type `adb connect 192.168.56.101` (or whatever the IP address was that you saw earlier from the `devices list` command line).
Type `adb devices` to confirm that it is connected.
Type ionic platform add android to add Android as a platform for your app.

Finally, type ionic run android.