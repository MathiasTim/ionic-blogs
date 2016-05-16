### A pro's workflows for building apps with Ionic. Part 2: Mountain
Welcome to serious-app-development-mountain! In the second part of our series on developing Ionic apps with [Generator-M-Ionic](https://github.com/mwaylabs/generator-m-ionic) you'll be learning about wonderful ingredients like testing, sub-generators, plugins and ecosystem integration into platforms like the [Ionic Platform](http://ionic.io/). We are building on top of the project we created in part 1 of this series.


### Cross-platform HTML5 app development is hard
Yes, even with awesome Generator-M-Ionic, awesome Angular, awesome Cordova, awesome Ionic which supports iOS, Android, the mobile web (since [Ionic 1.2](http://blog.ionic.io/announcing-ionic-1-2/)) and even Windows (in [Ionic 2](http://blog.ionic.io/announcing-windows-support-in-ionic-2/)) cross-platform HTML5 app development can be a pain.

> and even Windows (in [Ionic 2](http://blog.ionic.io/announcing-windows-support-in-ionic-2/))
Thats not completely true, since in [this](http://blog.ionic.io/announcing-ionic-1-2/) blog post they announce support for Windows (10, with the Edge browser) with Ionic 1.2.

Maybe your app should run on phones and tablets. With a different, especially designed layout and workflow for each of them. Ah, and what the customer forgot to tell you: obviously it should also run in the browser! It's built using web technologies anyways, isn't it? Maybe even on desktops using [Electron](http://electron.atom.io/)? You'll be optimizing and testing a lot!

Even when that's taken care of there's a lot of complex topics like handling translations, offline data, persistence and data syncing between devices or the backend and your app. Things that need to be thoroughly coded while you juggle certificates and licenses when building for the different app stores, trying to integrate complicated Push services or coding custom Cordova plugins and hooks. And usually when it's the very last thing you need: along come changing customer requirements and you're back to the drawing board.

I know. We've been through all of this. That's why we built [Generator-M-Ionic](https://github.com/mwaylabs/generator-m-ionic), a trust-worthy and powerful coding companion so that you can focus on the real challenges. And development shouldn't be one of them!

Are you ready to climb that mountain?

### Quality assurance
You've set up your project, created your first commit and are now ready to start coding. Whether you are coding alone or in a team, as a developer worried about professional development you'll want to take some measures to ensure the quality of your code. We've got you covered on this one!

#### Linting
Your Generator-M-Ionic project comes with established coding guidelines and workflows already baked in using [ESLint](http://eslint.org/). On every iteration of `gulp watch` Gulp will check all your application JavaScript files for guideline violations.

![image](img/eslint.png)

To additionally get linting notifications as you develop in your editor or to learn how to configure the default set of rules, check out our [ESLint Guide](https://github.com/mwaylabs/generator-m-ionic/blob/master/docs/guides/eslint.md). If you are working with JSON files in your `app/` folder -for instance to handle translations- the generator's linting will validate those too! This keeps your development trouble free.

> What about pre-commit hooks? https://www.npmjs.com/package/husky

#### Testing
Another area where you just don't have to deal with the hassle of setting up and configuring everything yourself is unit testing with karma and end-to-end testing with protractor. It just works. Your sample app even comes with a ready-to-use test-suite that you can try out right now by running:
```sh
gulp karma
# and
gulp protractor
```
The relevant files for the test setup are these:
```
test/
  └──  karma/
  └──  protractor/
karma.conf.js
protractor.conf.js
```
Our [Testing Guide](https://github.com/mwaylabs/generator-m-ionic/blob/master/docs/guides/testing.md) can help you get started with writing your own unit and end-to-end tests for your app.

### Coding
Ok, ok, enough already! You want to finally start coding and developing your very own app? Say no more. You'll probably want to add:
- your own Angular components using our [subgenerators](https://github.com/mwaylabs/generator-m-ionic/blob/master/docs/guides/sub_generators.md)
  - controllers, templates, directives, services, filters, constants, ... or even whole modules
- some [Sass](http://sass-lang.com/) to spice up your app's styling
- [Cordova Plugins](http://ngcordova.com/docs/plugins/) to use with [ngCordova](http://ngcordova.com/) for that real app-feeling
- and maybe some additional [bower packages](http://bower.io/search/)

We will go through each of these tasks briefly to give you a general idea of how things work. For more detailed explanations visit our [Documentation](https://github.com/mwaylabs/generator-m-ionic/).

#### Adding Angular components
Our array of [subgenerators](https://github.com/mwaylabs/generator-m-ionic/blob/master/docs/guides/sub_generators.md) allows you to create the most important Angular components very easily. If applicable they also generate sample test files so you can start testing right away.

For instance we use the pair-subgenerator a lot. It creates a controller, with a test file and a template with the same name.
```sh
yo m-ionic:pair phone
```
![image](img/sub_generator.png)

Now we only need to add a state to the `main.js`:
```js
.state('main.phone', {
  url: '/phone',
  views: {
    'tab-phone': {
      templateUrl: 'main/templates/phone.html',
      controller: 'PhoneCtrl as ctrl'
    }
  }
})
```
And a navigation item in our `tabs.html` which you'll find in `app/main/templates/` to bring it all together:
```html
<!-- List Tab -->
<ion-tab title="Phone" icon-off="ion-ios-telephone-outline"
  icon-on="ion-ios-telephone"
  ui-sref="main.phone">
  <ion-nav-view name="tab-phone"></ion-nav-view>
</ion-tab>
```
That's it. A new navigation item, a new route, controller and template in about two minutes. Here's the result:

![image](img/nav_phone.png)

#### Adding Sass
This is an even easier task. Every module you generate comes with a default Sass file. For your main module this would be `main.scss` and it's located in `app/main/styles/`. Open it and add some Sass:
```scss
ion-list {
  ion-item {
    color:red;
  }
}
```
Upon saving, your `gulp watch` task will automatically compile and inject the resulting CSS, even reload your browser. Not sassy enough? As your project grows larger, you may want to split your Sass in multiple files. Find out how in our [Sass integration Guide](https://github.com/mwaylabs/generator-m-ionic/blob/master/docs/guides/sass_integration.md).

#### Adding plugins
I know. Before we don't add some nice [Cordova Plugins](http://ngcordova.com/docs/plugins/) to our app, it won't be a real hybrid app. So let's do it!

Your project comes with a local installation of the latest version of the [Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/index.html) which you can invoke through Gulp. We install it locally so you don't have to worry about which project you set up with which version. It's always the one it got set up with. The syntax is almost exactly the same as using a global CLI installation. So for instance to install the Cordova camera plugin run:

> In my opinion the point on the local cordova and why (professonal department with many customers and apps, customers which want a small CR on an old app, etc.) is not explained and stressed enough, since I think for most people it looks more like a burden than help 

```sh
gulp --cordova "plugin add org.apache.cordova.camera --save"
```
You want to develop for Windows as well? Type:
```sh
gulp --cordova "platform add windows --save"
```
Don't forget to call `--save` in order to persist new plugins and platforms in the `config.xml`! Our Development Introduction has a dedicated part on [using the Cordova CLI wrapper](https://github.com/mwaylabs/generator-m-ionic/blob/master/docs/start/development_intro.md#using-the-cordova-cli).

Now that we installed a new plugin we want to use it! [ngCordova](http://ngcordova.com/) is declared as a dependency for every module you create using the generator. Refer to the main module declaration in your `main.js` to see how it's done. So in my `PhoneCtrl` I'll just inject the `$cordovaCamera` [service](http://ngcordova.com/docs/plugins/camera/) to access the plugin:
```js
angular.module('main')
.controller('PhoneCtrl', function ($cordovaCamera) {

  var options = {
    destinationType: Camera.DestinationType.FILE_URI,
    sourceType: Camera.PictureSourceType.CAMERA,
  };

  $cordovaCamera.getPicture(options)
  .then(function(imageURI) {
    console.log(imageURI);
  });
});
```
Some plugins expose JavaScript globals and some developers prefer to access their plugins through the `cordova` global because ngCordova is not always up to date with the latest plugin versions or may not support the plugin they want to use. In that case you need to extend your globals section of your `app/.eslintrc` file:
```js
//..
"globals": {
  "angular": true,
  "ionic": true,
  "localforage": true,
  // add those
  "cordova": true,
  "Camera": true
},
//..
```
Adding new plugins has never been so simple!

#### Bower packages
And last but not least you'll probably want some more [bower packages](http://bower.io/search/) that go with your app. Maybe your app needs to support different languages? For that we use [angular-translate](https://github.com/angular-translate/angular-translate). Install it by running:
```sh
bower install angular-translate --save
```
The `--save` flag will persist that package in the `bower.json`. Sometimes for all changes to take effect it is necessary to restart `gulp watch`. Then the only thing left to do is to mark the module as a dependency in your `main.js` module declaration:
```js
'use strict';
angular.module('main', [
  'ionic',
  'ngCordova',
  'ui.router',
  // add this one
  'pascalprecht.translate'
])
```
Translations? Now you got them! Refer to their [documentation](https://angular-translate.github.io/) to learn more.

### Browser or device?
Up until now we have only used `gulp watch` for our developing purposes. At least when you are working with plugins you may want to test your app on a device. If you have your system correctly set up according to the [Cordova Platform Guides](http://cordova.apache.org/docs/en/latest/#develop-for-platforms), this should be as easy as running:
```sh
gulp --cordova "run ios"
# or
gulp --cordova "run android"
```

> Maybe we should gave a hint for all people really knew to this topic to preinstall the SDKs, certifiactes etc.
> On iOS for example it will be no problem running on the simulator without a developer account. For the device you need a provisioned device and certifiacte

Two things happen here:

1. Gulp will build your app using `gulp build`
  - this takes care of all the JavaScript, HTML and CSS and puts it into the `www/` folder
2. Gulp will call Cordova with the supplied attributes and
  - takes the contents of the `www/` folder and builds a Cordova app from it
  - the app is then pushed onto your device

![image](img/device.jpg)

Test your glorious app on your device!

The implicit run of `gulp build` and for which Cordova commands it will run is explained in more detail in our [Development Introduction](https://github.com/mwaylabs/generator-m-ionic/blob/master/docs/start/development_intro.md#cordova-build-run-emulate--under-the-hood).


#### gulp watch-build
Usually `gulp build` will build your app without any problems and everything should work just as if you run `gulp watch`. However sometimes it will be necessary to debug the web app part of your build. Since this is a little cumbersome to do when the app's already on your device there's a nice intermediate step:

```sh
gulp watch-build
```
This is just like `gulp watch` but only for the `www/` folder. It builds your app into that folder and then opens a browser windows showing the built version of your app. This allows you do quickly find out why your app doesn't build properly. Adding the `--no-build` flag enables you to watch the current version in the `www/` folder. Of course this command also works with `--no-open` and all the other flags that work with `gulp watch` as well.


### Ecosystems
I bet you start feeling like a real pro already. And you should! We're getting close to orbit. Although you're more and more becoming a seasoned astronaut there are some things you just don't want to build yourself every time. Push services, user management or other backend services for instance. Luckily there's a series of choices of platforms to handle those tasks for you: the [Ionic Platform](http://ionic.io/platform) is popular and feature rich, for our enterprise customers we offer our [Relution Enterprise Mobility Platform](http://www.mwaysolutions.com/en/mobile-device-management-mdm/) and there are many other options.

> Please be careful with these terms like "Relution Enterprise Mobility Platform" I don't think we call any part of reultion so. I think you mean MADP, mobile application development platform or the newest name (no idea if its public yet) BaaS, backend as a service. Because all this MDM stuff is not very interessting for developers.

The 'Ecosystems' section of our [guides](https://github.com/mwaylabs/generator-m-ionic#guides) can help you integrate some of these platforms with nice set up guides, generated sample code and templates. Most of the work we've done for you already, so focussing on building your app is easier than ever.

### Congratulations!
You have conquered the mountain and learned how to season your app with a lot different interesting spices: sub-generators, Sass, plugins, ecosystems, bower packages. Be proud! Ascend your development skills to out-of-this-world levels in part 3 of this series by owning environments, proxies and build tools handling app icons, splash screens, continuous integration and build variables. And at the very end, see what the future of Generator-M-Ionic has to offer for you!

### Get in touch
One thing we're particularly interested in: Live-reload for the device. How important do you think it is?

Feedback, ideas, comments regarding this blog post or any of the features discussed here are very welcome in either the comments section below, at our [Generator-M-Ionic's Github repository](https://github.com/mwaylabs/generator-m-ionic) or the [Generator-M-Ionic Gitter Chat](https://gitter.im/mwaylabs/generator-m-ionic).

> why here? and not at the end of the series?
