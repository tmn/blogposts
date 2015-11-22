# Making mobile apps using Fuse
Sathurday 21 November 2015

There's a new kid in town! In October 2015 the peoples over at Fuse announced their new UX tool suite for building awesome mobile applications as an open BETA. A tool meant to bring designers and developer closer to each other.

There are of course other tools out there letting you create and deploy application on multiple platforms. But the new cool thing about Fuse is its ability to push updates to the look and feel on multiple devices in real time while designing and developing. This makes the process from the designers to the implementation by the developers work more seamlessly.

To get started the Fuse team themselves provides great tutorial videos. They also has a documentation section on their site with enough information to get around. It could of course be more comprehensive, but for now it's ok.

I tried it for the first time back in 2013, and a lot have been changed since. With the mux improved UX Markup and the integration of JavaScript you can easily get around creating great looking apps in no time. For the more advanced users, Fuse lets you access the native Android and iOS APIs directly using Uno. Unis is a native C# dialect. And if you have been using C# nearly everything you're known to will apply in Uno code. I've also created a simple app for getting departure time for the buses in Trondheim, Norway, in real time. You can see the code on [my GitHub page](https://github.com/tmn/FuseBus).

![Fuse](https://az664292.vo.msecnd.net/fusetoolscom/v1447844604954/images/fuse_hotspots_tinypng.png)
Picture by [Fuse](http://fusetools.com/)


## Trying out Fuse

In order to start explorering Fuse all you need is to download and install the tool itself from [fusetools.com](http://fusetools.com/). Their site is also the best reference for learning Fuse.

## Creating our first app!

We will start out simple by using only UX Markup. UX Markup is a XML-based format that should be immediately familiar to anyone who has worked with similar formats. The root tag of them all is the ´<App>´ tag. This one bootstraps the app and takes care of the application lifecycle.

```xml
<App Theme="Basic" Background="#fff">
  <Text>This is header!</Text>
</App>
```

### Reusable classes

The main .ux file might grow out of scale, and you might want to tighten it up by extracting some of the code to make it reusable. This can be done in many ways. On way is to declare the class inside your main file. Let's say you has some awesome ordering in the file you don't want to mess up:

```xml
<App>
  <Panel ux:Class="MyAwesomePanel">
    <Text FontSize="14">This is my awesome panel!</Text>
  </Panel>

  <!-- and use it here!-->
  <MyAwesomePanel />
  <MyAwesomePanel />
  <MyAwesomePanel />
  <MyAwesomePanel />
</App>
```

or you can create a separate file. Like MyAwesomePanel.ux:

```xml
<Panel ux:Class="MyAwesomePanel">
  <Text FontSize="14">This is my awesome panel!</Text>
</Panel>
```

And use it as the example above. No need for extra include statements.



### JavaScript

You can of course do a lot with UX Markup only. Animations, triggers, event listeners, and much more. But sometimes you need more. You need some kind of business logic. The app needs to do _something_. This is easily solved using JavaScript.

js/app.js:
```javascript
function click_handler() {
  debug_log('I just got clicked!');
}

module.exports = {
  click_handler: click_handler
}
```

MainApp.ux:
```xml
<App>
  <JavaScript File="js/app.js" />
  <Button Clicked="{click_handler}" Text="Click me!" />
</App>
```

#### Multiple JavaScript modules

Writing your whole app in one huge JavaScript module might not be the greatest idea. And you can of course split it up into different modules, and include them in other JavaScript files:

```xml
<App>
  <JavaScript File="js/MyComponent.js" ux:Global="MyComponent" />
  <JavaScript File="js/app.js" />
</App>
```

js/MyComponent.js:
```javascript
function MyComponent() {
  // .. do awesome stuff here!
}

module.exports = MyComponent;
```

js/app.js:
```javascript
var MyComponent = require('MyComponent');

// use awesome MyComponent here...
```

Notice the use of `ux:Global` when including the JavaScript file in the `.ux` file. The file is still included without the `ux:Global` attribute. But to access it from other JS files you need to give it a name.


## Exposing native APIs with Uno

Uno is a native dialect of #C where you have direct access to Android and iOS APIs.
