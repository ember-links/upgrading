# Upgrading Ember CLI

This is a guide I’ve written to document my experience in the hope that it could be useful to others going through the same process. It’s quite possible some of the recommendations I’ve made are not best practice (though they have worked for me) so I’d love to get any feedback and suggestions if you have them.

## First, a little background

Upgrading from Ember 1.13 to 2.x was something which for a time I was nervous about attempting. At that point in time my project had very limited (if any) tests and I was seeing a lot of deprecations logged both via the console and when I used Ember Inspector.

I really wanted to make the jump but it was after watching [this video from the Ember NYC meetup by Robert Jackson](https://youtu.be/ltzN4v-ymo4?t=1h26m20s) that gave me the confidence to do it.

In particular learning about [ember-cli-deprecation-workflow](https://github.com/mixonic/ember-cli-deprecation-workflow) and how it could rationalise the process was a real motivating factor.

Having said that though — and there is no easy was of saying this — to really have confidence I accepted that I'd need a much more comprehensive test suite. In hindsight, I'm really glad I took this approach and would *highly recommend* using this desire as the necessary motivating factor to start writing tests if you haven't already.

Once you have tests which handle the majority of the use cases this process becomes much, much less intimidating — plus, you now have tests!

I’m not going to go into resources for writing tests right now (something for another time) - but once you do have them this is the process I use to get up to date when new versions of Ember (& Ember CLI) are released.

## Next, this is my generic process to get from one version to the next

### Flushing Deprecations

Assuming you have already installed the [ember-cli-deprecation-workflow](https://github.com/mixonic/ember-cli-deprecation-workflow) run your tests and afterwards in the console flush the deprecations to see what needs to be done before updating by

`deprecationWorkflow.flushDeprecations()`

Then, simply(!) systematically work through each deprecation one by one as outlined in the documentation for the addon.

### Managing NPM Dependencies

> The caret, on the other hand, is more relaxed. It will update you to the most recent major version (the first number). ^1.2.3 will match any 1.x.x release including 1.3.0, but will hold off on 2.0.0.

I’ve found once or twice this approach has caused me some issues (at least I *think* it was the cause), so I tend to prefer to use strict versions and update each dependency myself explicitly.

I’d be interested to hear if you think this is a good/bad idea and why.

I’ve found that [Greenkeeper.io](https://greenkeeper.io/) is a great help for keeping up to date with each of the npm packages as they are updated between Ember CLI releases.

You’ll see in each individual guide the specific version numbers which I updated at the time (though if you are reading this for a very old version it may need to be double checked). One thing I can say is that the compatability was correct at the time so there is a pinned version which should still work.

### Working on a new branch

Before starting the upgrade process I always create a new branch to make it easy to get back to the previous state if something goes horribly wrong.

# Upgrading to specific versions guides

- [Upgrading to Ember 2.4.3](#upgrading-to-ember-243)
- [Upgrading to Ember 2.5.1](#upgrading-to-ember-251)
- [Upgrading to Ember 2.6.1](#upgrading-to-ember-261)
- [Upgrading to Ember 2.7.0](#upgrading-to-ember-270)

## Upgrading to Ember 2.4.3

Once Ember CLI is at v2.3.0 then it is a decision to make whether to update each dot release, or to roll them into one.

As the upgrade guide suggested there were not too many user changes to make between the dot releases I updated from 2.3.0 directly to 2.4.3

Folding together the changes listed made this:

- update ember-cli-sri in package.json to 2.1.0
- rename testem.json to testem.js

But in reality I had to make a few more updates, listed below.

### Install Ember CLI

Taken from the [official release](https://github.com/ember-cli/ember-cli/releases/tag/v2.4.3)

Install Ember CLI Globally

- `npm uninstall -g ember-cli`
- `npm cache clean`
- `bower cache clean`
- `npm install -g ember-cli@2.4.3`

Update Project

- `rm -rf node_modules bower_components dist tmp`
- `npm install ember-cli@2.4.3 --save-dev`
- `npm install`
- `bower install`

### Running `ember init`

Begin the process of updating your project files

- `ember init`

For this version the only files I hit `Y` to overwrite were

- `.travis.yml`
- `tests/helpers/module-for-acceptance.js` (destroyApp moved after conditional)

Though check that the you're `index.html`& `tests/index.html` look OK because there have been some subtle updates in the use of double quotes for `content-for` tags. For `tests/index.html` you might also want to check that you have `<script src="testem.js" integrity=""></script>` included now.

No other files appeared to have any code that needed to be updated, except for bower.json & package.json — the usual culprits — which I always prefer not to overwrite automatically and update by hand afterwards (see below).

#### bower.json

updates

- ember to `"ember": "2.4.3"`
- ember-cli-shims to `"ember-cli-shims": "0.1.1"`

check

- ember-cli-test-loader is `"ember-cli-test-loader": "0.2.2"`
- ember-qunit-notifications is `"ember-qunit-notifications": "0.1.0"`

```diff

 {
   "dependencies": {
-    "ember": "2.3.1",
-    "ember-cli-shims": "0.1.0",
+    "ember": "2.4.3",
+    "ember-cli-shims": "0.1.1",
     "ember-cli-test-loader": "0.2.2",
     "ember-qunit-notifications": "0.1.0"
   }
 }

```

#### package.json

updates

- broccoli-asset-rev to `"broccoli-asset-rev": "2.4.2"`
- ember-cli-babel to `"ember-cli-babel": "5.1.6"`
- ember-cli-htmlbars to `"ember-cli-htmlbars": "1.0.3"`
- ember-cli-inject-live-reload to `"ember-cli-inject-live-reload": "1.4.0"`
- ember-cli-qunit to `"ember-cli-qunit": "1.4.0"`
- ember-cli-sri to `"ember-cli-sri": "2.1.0"`
- ember-data to `"ember-data": "2.4.2"`
- ember-export-application-global to `"ember-export-application-global": "1.0.5"`
- ember-load-initializers to `"ember-load-initializers": "0.5.1"`
- loader.js to `"loader.js": "4.0.1"`

check correct version

- ember-ajax is `"ember-ajax": "0.7.1"`
- ember-cli is `"ember-cli": "2.4.3"`
- ember-cli-app-version is `"ember-cli-app-version": "1.0.0"`
- ember-cli-dependency-checker is `"ember-cli-dependency-checker": "1.2.0"`
- ember-cli-htmlbars-inline-precompile is `"ember-cli-htmlbars-inline-precompile": "0.3.1"`
- ember-cli-release is `"ember-cli-release": "0.2.8"`
- ember-cli-uglify is `"ember-cli-uglify": "1.2.0"`
- ember-resolver is `"ember-resolver": "2.0.3"`

remove

- [ember-disable-proxy-controllers](https://github.com/ember-cli/ember-cli/pull/5678)

now optional (can keep if already using)

ember-cli-content-security-policy is [no longer included by default](https://github.com/ember-cli/ember-cli/blob/ec81775f7f5695ea423dfb19693b5f63be1580fd/CHANGELOG.md#changes-since-11315)

```diff

 {
   "devDependencies": {
-    "broccoli-asset-rev": "2.2.0",
+    "broccoli-asset-rev": "2.4.2",
     "ember-ajax": "0.7.1",
     "ember-cli": "2.4.3",
     "ember-cli-app-version": "1.0.0",
-    "ember-cli-babel": "5.1.5",
+    "ember-cli-babel": "5.1.6",
     "ember-cli-content-security-policy": "0.4.0",
     "ember-cli-dependency-checker": "1.2.0",
-    "ember-cli-htmlbars": "1.0.1",
+    "ember-cli-htmlbars": "1.0.3",
     "ember-cli-htmlbars-inline-precompile": "0.3.1",
-    "ember-cli-inject-live-reload": "1.3.1",
+    "ember-cli-inject-live-reload": "1.4.0",
-    "ember-cli-qunit": "1.2.1",
+    "ember-cli-qunit": "1.4.0",
     "ember-cli-release": "0.2.8",
-    "ember-cli-sri": "2.0.0",
+    "ember-cli-sri": "2.1.0",
     "ember-cli-uglify": "1.2.0",
-    "ember-data": "2.3.0",
+    "ember-data": "2.4.2",
-    "ember-disable-proxy-controllers": "1.0.1"
-    "ember-export-application-global": "1.0.4",
+    "ember-export-application-global": "1.0.5",
-    "ember-load-initializers": "0.5.0",
+    "ember-load-initializers": "0.5.1",
     "ember-resolver": "2.0.3",
-    "loader.js": "4.0.0",
+    "loader.js": "4.0.1"
   }
 }

```

### Testing the new version

Time to nombom and hope that there aren't any issues.

- `rm -rf node_modules bower_components dist tmp`
- `npm cache clean`
- `bower cache clean`
- `npm i`
- `bower i`

Then run your tests and see if there are any issues building or any deprecations.

`ember test --server`

If you do find and issues, don’t panic. Often these can be caused by addons and fixed by simply updated to a newer version.

Once everything is working you may also have some deprecations, these should be handled in the usual way.

In my case from upgrading from Ember CLI 2.3.0 -> 2.4.3 it just worked™

:rocket:

## Upgrading to Ember 2.5.1

Taken from the [official release](https://github.com/ember-cli/ember-cli/releases/tag/v2.5.1)

Install Ember CLI Globally

- `npm uninstall -g ember-cli`
- `npm cache clean`
- `bower cache clean`
- `npm install -g ember-cli@2.5.1`

Update Project

- `rm -rf node_modules bower_components dist tmp`
- `npm install ember-cli@2.5.1 --save-dev`
- `npm install`
- `bower install`

### Running `ember init`

Begin the process of updating your project files

- `ember init`

#### bower.json

update

- ember to `"ember": "2.5.1"`

check

- ember-cli-shims is `"ember-cli-shims": "0.1.1"`
- ember-cli-test-loader is `"ember-cli-test-loader": "0.2.2"`
- ember-qunit-notifications is `"ember-qunit-notifications": "0.1.0"`

```diff

 {
   "dependencies": {
-    "ember": "2.4.3",
+    "ember": "2.5.1",
     "ember-cli-shims": "0.1.1",
     "ember-cli-test-loader": "0.2.2",
     "ember-qunit-notifications": "0.1.0"
   }
 }

```

#### package.json

add

- ember-cli-jshint as `"ember-cli-jshint": "1.0.0"`

update

- ember-data to `"ember-data": "2.5.0"`
- loader.js to `"loader.js": "4.0.1"`

check

- broccoli-asset-rev is `"broccoli-asset-rev": "2.4.2"`
- ember-ajax is `"ember-ajax": "0.7.1"`
- ember-cli-app-version is `"ember-cli-app-version": "1.0.0"`
- ember-cli-babel is `"ember-cli-babel": "5.1.6"`
- ember-cli-dependency-checker is `"ember-cli-dependency-checker": "1.2.0"`
- ember-cli-htmlbars is `"ember-cli-htmlbars": "1.0.3"`
- ember-cli-htmlbars-inline-precompile is `"ember-cli-htmlbars-inline-precompile": "0.3.1"`
- ember-cli-inject-live-reload is `"ember-cli-inject-live-reload": "1.4.0"`
- ember-cli-qunit is `"ember-cli-qunit": "1.4.0"`
- ember-cli-release is `"ember-cli-release": "0.2.8"`
- ember-cli-sri is `"ember-cli-sri": "2.1.0"`
- ember-cli-uglify is `"ember-cli-uglify": "1.2.0"`
- ember-export-application-global is `"ember-export-application-global": "1.0.5"`
- ember-load-initializers is `"ember-load-initializers": "0.5.1"`
- ember-resolver is `"ember-resolver": "2.0.3"`

```diff

 {
   "devDependencies": {
     "broccoli-asset-rev": "2.4.2",
     "ember-ajax": "0.7.1",
     "ember-cli-app-version": "1.0.0",
     "ember-cli-babel": "5.1.6",
     "ember-cli-dependency-checker": "1.2.0",
     "ember-cli-htmlbars": "1.0.3",
     "ember-cli-htmlbars-inline-precompile": "0.3.1",
     "ember-cli-inject-live-reload": "1.4.0",
+    "ember-cli-jshint": "1.0.0",
     "ember-cli-qunit": "1.4.0",
     "ember-cli-release": "0.2.8",
     "ember-cli-sri": "2.1.0",
     "ember-cli-uglify": "1.2.0",
-    "ember-data": "2.4.2",
+    "ember-data": "2.5.0",
     "ember-export-application-global": "1.0.5",
     "ember-load-initializers": "0.5.1",
     "ember-resolver": "2.0.3",
-    "loader.js": "4.0.0",
+    "loader.js": "4.0.1"
   }
 }

```

### Testing the new version

Time to nombom and hope that there aren't any issues.

- `rm -rf node_modules bower_components dist tmp`
- `npm cache clean`
- `bower cache clean`
- `npm i`
- `bower i`

Then run your tests and see if there are any issues building or any deprecations.

`ember test --server`

In this instance my test which included mock uploading files all broke.

After some investigation it transpired that the issue was to do with the new [native events used in testing](http://emberjs.com/blog/2016/04/11/ember-2-5-released.html#toc_native-event-test-helpers).

To help anyone else that comes across this issue when upgrading I wrote a [short blog post on how to update your tests](https://medium.com/@chrisdmasters/acceptance-testing-file-uploads-in-ember-2-5-1c9c8dbe5368)

Besides that everything else was fine.

:rocket: :rocket:

## Upgrading to Ember 2.6.2

Taken from the [official release](https://github.com/ember-cli/ember-cli/releases/tag/v2.6.2)

Install Ember CLI Globally

- `npm uninstall -g ember-cli`
- `npm cache clean`
- `bower cache clean`
- `npm install -g ember-cli@2.6.2`

Update Project

- `rm -rf node_modules bower_components dist tmp`
- `npm install ember-cli@2.6.2 --save-dev`
- `npm install`
- `bower install`

### Running `ember init`

Begin the process of updating your project files

- `ember init`

#### bower.json

update

- ember to `"ember": "2.6.0"`

check

- ember-cli-shims is `"ember-cli-shims": "0.1.1"`
- ember-cli-test-loader is `"ember-cli-test-loader": "0.2.2"`
- ember-qunit-notifications is `"ember-qunit-notifications": "0.1.0"`

```diff

 {
   "dependencies": {
-    "ember": "2.5.1",
+    "ember": "2.6.0",
     "ember-cli-shims": "0.1.1",
     "ember-cli-test-loader": "0.2.2",
     "ember-qunit-notifications": "0.1.0"
   }
 }

```

#### package.json

update

- ember-cli-jshint to `"ember-cli-jshint": "1.0.4"`
- ember-cli-qunit to `"ember-cli-qunit": "2.0.2"`
- ember-cli-release to `"ember-cli-release": "0.2.9"`
- ember-data to `"ember-data": "2.6.1"`
- loader.js to `"loader.js": "4.0.9"`

check

- broccoli-asset-rev is `"broccoli-asset-rev": "2.4.2"`
- ember-ajax is `"ember-ajax": "2.4.1"`
- ember-cli-app-version is `"ember-cli-app-version": "1.0.0"`
- ember-cli-babel is `"ember-cli-babel": "5.1.6"`
- ember-cli-dependency-checker is `"ember-cli-dependency-checker": "1.2.0"`
- ember-cli-htmlbars is `"ember-cli-htmlbars": "1.0.8"`
- ember-cli-htmlbars-inline-precompile is `"ember-cli-htmlbars-inline-precompile": "0.3.2"`
- ember-cli-inject-live-reload is `"ember-cli-inject-live-reload": "1.4.0"`
- ember-cli-sri is `"ember-cli-sri": "2.1.0"`
- ember-cli-uglify is `"ember-cli-uglify": "1.2.0"`
- ember-export-application-global is `"ember-export-application-global": "1.0.5"`
- ember-load-initializers is `"ember-load-initializers": "0.5.1"`
- ember-resolver is `"ember-resolver": "2.0.3"`


```diff

 {
   "devDependencies": {
     "broccoli-asset-rev": "2.4.2",
     "ember-ajax": "2.4.1",
     "ember-cli": "2.6.1",
     "ember-cli-app-version": "1.0.0",
     "ember-cli-babel": "5.1.6",
     "ember-cli-dependency-checker": "1.2.0",
     "ember-cli-htmlbars": "1.0.8",  
     "ember-cli-htmlbars-inline-precompile": "0.3.2",
     "ember-cli-inject-live-reload": "1.4.0",
-    "ember-cli-jshint": "1.0.3",
+    "ember-cli-jshint": "1.0.4",
-    "ember-cli-qunit": "1.4.0",
+    "ember-cli-qunit": "2.0.2",
-    "ember-cli-release": "0.2.8",
+    "ember-cli-release": "0.2.9",
     "ember-cli-sri": "2.1.0",
     "ember-cli-uglify": "1.2.0",
-    "ember-data": "2.5.3",
+    "ember-data": "2.6.1",
     "ember-export-application-global": "1.0.5",
     "ember-load-initializers": "0.5.1",
     "ember-resolver": "2.0.3",
-    "loader.js": "4.0.7",
+    "loader.js": "4.0.9"
   }
 }

```

Yes to `tests/helpers/module-for-acceptance.js` as there have been some changes to the way it works.

Finally, at the moment some addons have not yet updated to [include a call to super in the init() method](https://github.com/ember-cli/core-object/blob/master/core-object.js#L53-L57)

For me there were no deprecations to update in my code this time.

:rocket: :rocket: :rocket:

## Upgrading to Ember 2.7.0

Taken from the [official release](https://github.com/ember-cli/ember-cli/releases/tag/v2.7.0)

Install Ember CLI Globally

- `npm uninstall -g ember-cli`
- `npm cache clean`
- `bower cache clean`
- `npm install -g ember-cli@2.7.0`

Update Project

- `rm -rf node_modules bower_components dist tmp`
- `npm install ember-cli@2.7.0 --save-dev`
- `npm install`
- `bower install`

### Running `ember init`

Begin the process of updating your project files

- `ember init`

Personally I wasn’t bothered by the editor config changes

#### .jshintrc

```diff
-  "esnext": true,
+  "esversion": 6,
```

#### index.html

This is important as relates to the new approach to URLs

see http://emberjs.com/blog/2016/04/28/baseURL.html

Effectively add the rootURL to the asset URLs

```diff
-    <link rel="stylesheet" href="assets/vendor.css">
-    <link rel="stylesheet" href="assets/instatube-app.css">
+    <link rel="stylesheet" href="{{rootURL}}assets/vendor.css">
+    <link rel="stylesheet" href="{{rootURL}}assets/instatube-app.css">
```

```diff
-    <script src="assets/vendor.js"></script>
-    <script src="assets/instatube-app.js"></script>
+    <script src="{{rootURL}}assets/vendor.js"></script>
+    <script src="{{rootURL}}assets/instatube-app.js"></script>
```

#### router.js

Add rootURL to Router with

```diff
-const Router = Ember.Router.extend(googlePageview, {
-  location: config.locationType
+const Router = Ember.Router.extend({
+  location: config.locationType,
+  rootURL: config.rootURL
 });
 ```

#### bower.json

update

- ember to `"ember": "2.7.0"`

check

- ember-cli-shims is `"ember-cli-shims": "0.1.1"`
- ember-qunit-notifications is `"ember-qunit-notifications": "0.1.0"`

remove

- ember-cli-test-loader (now via npm)

```diff
-    "ember": "2.6.0",
+    "ember": "2.7.0",
     "ember-cli-shims": "0.1.1",
-    "ember-cli-test-loader": "0.2.2",
     "ember-qunit-notifications": "0.1.0"
```
#### config.js

For the new baseURL and rootURL change

in the ENV

```diff
-    baseURL: '/',
-    locationType: 'history',
+    rootURL: '/',
+    locationType: 'auto',
```

and in

```diff
   if (environment === 'test') {
     // Testem prefers this...
-    ENV.baseURL = '/';
-    ENV.locationType = 'auto';
+    ENV.locationType = 'none';
```

#### package.json

add

- ember-cli-test-loader to `"ember-cli-test-loader": "1.1.0"`

update

- broccoli-asset-rev to `"broccoli-asset-rev": "2.4.5"`
- ember-ajax to `"ember-ajax": "2.4.1"`
- ember-cli to `"ember-cli": "2.7.0"`
- ember-cli-babel to `"ember-cli-babel": "5.1.7"`
- ember-cli-dependency-checker to `"ember-cli-dependency-checker": "1.3.0"`
- ember-cli-htmlbars to `"ember-cli-htmlbars": "1.0.10"`
- ember-cli-htmlbars to `"ember-cli-htmlbars-inline-precompile": "0.3.3"`
- ember-cli-inject-live-reload to `"ember-cli-inject-live-reload": "1.4.1"`
- ember-cli-jshint to `"ember-cli-jshint": "1.0.4"`
- ember-cli-qunit to `"ember-cli-qunit": "2.1.0"`
- ember-cli-release to `"ember-cli-release": "0.2.9"`
- ember-data to `"ember-data": "2.7.0"`
- loader.js to `"loader.js": "4.0.10"`

check

- ember-cli-app-version is `"ember-cli-app-version": "1.0.0"`
- ember-cli-sri is `"ember-cli-sri": "2.1.0"`
- ember-cli-uglify is `"ember-cli-uglify": "1.2.0"`
- ember-export-application-global is `"ember-export-application-global": "1.0.5"`
- ember-load-initializers is `"ember-load-initializers": "0.5.1"`
- ember-resolver is `"ember-resolver": "2.0.3"`

```diff

 {
   "devDependencies": {
-    "broccoli-asset-rev": "2.4.2",
+    "broccoli-asset-rev": "2.4.5",
-    "ember-ajax": "0.7.1",
+    "ember-ajax": "2.4.1",
-    "ember-cli": "2.6.1",
+    "ember-cli": "2.7.0",
     "ember-cli-app-version": "1.0.0",
-    "ember-cli-babel": "5.1.6",
+    "ember-cli-babel": "5.1.7",
-    "ember-cli-dependency-checker": "1.2.0",
+    "ember-cli-dependency-checker": "1.3.0",
-    "ember-cli-htmlbars": "1.0.3",
+    "ember-cli-htmlbars": "1.0.10",
-    "ember-cli-htmlbars-inline-precompile": "0.3.1",
+    "ember-cli-htmlbars-inline-precompile": "0.3.3",
-    "ember-cli-inject-live-reload": "1.4.0",
+    "ember-cli-inject-live-reload": "1.4.1",
-    "ember-cli-jshint": "1.0.0",
+    "ember-cli-jshint": "1.0.4",
-    "ember-cli-qunit": "1.4.0",
+    "ember-cli-qunit": "2.1.0",
-    "ember-cli-release": "0.2.8",
+    "ember-cli-release": "0.2.9",
     "ember-cli-sri": "2.1.0",
+    "ember-cli-test-loader": "1.1.0",
     "ember-cli-uglify": "1.2.0",
-    "ember-data": "2.6.1",
+    "ember-data": "2.7.0",
     "ember-export-application-global": "1.0.5",
     "ember-load-initializers": "0.5.1",
     "ember-resolver": "2.0.3",
-    "loader.js": "4.0.1",
+    "loader.js": "4.0.10"
   }
 }

```


#### tests/.jshintrc

```diff
-  "esnext": true,
+  "esversion": 6,
```

#### tests/index.html

Updates once again for the new rootURL changes

```diff
-    <link rel="stylesheet" href="assets/vendor.css">
-    <link rel="stylesheet" href="assets/instatube-app.css">
-    <link rel="stylesheet" href="assets/test-support.css">
+    <link rel="stylesheet" href="{{rootURL}}assets/vendor.css">
+    <link rel="stylesheet" href="{{rootURL}}assets/instatube-app.css">
+    <link rel="stylesheet" href="{{rootURL}}assets/test-support.css">
```

```diff
-    <script src="testem.js" integrity=""></script>
-    <script src="assets/vendor.js"></script>
-    <script src="assets/test-support.js"></script>
-    <script src="assets/instatube-app.js"></script>
-    <script src="assets/tests.js"></script>
-    <script src="assets/test-loader.js"></script>
+    <script src="{{rootURL}}testem.js" integrity=""></script>
+    <script src="{{rootURL}}assets/vendor.js"></script>
+    <script src="{{rootURL}}assets/test-support.js"></script>
+    <script src="{{rootURL}}assets/instatube-app.js"></script>
+    <script src="{{rootURL}}assets/tests.js"></script>
```

### Upgrading issues

After this upgrade my tests weren’t passing immediately. It was pretty much exclusively due to the new URL system.

First issue (unrelated to URLs) was that Liquid Fire needed updating to the latest version (in my case v0.24.1).

After that I had to resolve path issues due to the new baseURL & rootURL changes.

Key changes required

- make sure (if using mirage) that this.namespace has a forward slash at the start [see this issue](https://github.com/samselikoff/ember-cli-mirage/issues/826)
- if you have custom URL endpoints in adapters I found that URLs when using prefixes, ie as used by Ember Igniter `${this.host}/${this.namespace}` no longer were correct, for now I just hardcoded the namespace, but am going to investigate further and raise an issue if necessary.
- for assets I found it necessary to include a forward slash in front of the urls to static assets in the public directory.

Beyond that there were a couple of very minor issues which were bugs in mind code that hadn't really been an issue before but for some reason now broke my tests. It was all to do with async testing and resolving promises, but were pretty trivial to fix.

:rocket: :rocket: :rocket: :rocket:

## Upgrading to Ember 2.8.0-beta.1

This time I updated to the beta version as I wanted to upgrade to Glimmer 2 (Ember 2.9.0-alpha) as soon as possible to help test the alpha version and thought that it would be a better starting point.

Taken from the [official release](https://github.com/ember-cli/ember-cli/releases/tag/v2.8.0-beta.1)

Install Ember CLI Globally

- `npm uninstall -g ember-cli`
- `npm cache clean`
- `bower cache clean`
- `npm install -g ember-cli@2.8.0-beta.1`

Update Project

- `rm -rf node_modules bower_components dist tmp`
- `npm install ember-cli@2.8.0-beta.1 --save-dev`
- `npm install`
- `bower install`

### Running `ember init`

Begin the process of updating your project files

- `ember init`

#### bower.json

update

- ember to `"ember": "2.8.0-beta.1"`

check

- ember-cli-shims is `"ember-cli-shims": "0.1.1"`

remove

- ember-qunit-notifications

```diff
   "dependencies": {
-    "ember": "2.7.0",
+    "ember": "2.8.0-beta.1",
-    "ember-qunit-notifications": "0.1.0"
```

#### package.json

update

- ember-cli to `"ember-cli": "2.8.0-beta.1"`
- ember-data to `"ember-data": "2.8.0-beta.1"`

check

- broccoli-asset-rev is `"broccoli-asset-rev": "2.4.5"`
- ember-ajax is `"ember-ajax": "2.4.1"`
- ember-cli-app-version is `"ember-cli-app-version": "1.0.0"`
- ember-cli-babel is `"ember-cli-babel": "5.1.7"`
- ember-cli-dependency-checker is `"ember-cli-dependency-checker": "1.3.0"`
- ember-cli-htmlbars is `"ember-cli-htmlbars": "1.0.10"`
- ember-cli-htmlbars is `"ember-cli-htmlbars-inline-precompile": "0.3.3"`
- ember-cli-inject-live-reload is `"ember-cli-inject-live-reload": "1.4.1",`
- ember-cli-jshint is `"ember-cli-jshint": "1.0.4"`
- ember-cli-qunit is `"ember-cli-qunit": "2.1.0"`
- ember-cli-release is `"ember-cli-release": "0.2.9"`
- ember-cli-sri is `"ember-cli-sri": "2.1.0"`
- ember-cli-test-loader is `"ember-cli-test-loader": "1.1.0"`
- ember-cli-uglify is `"ember-cli-uglify": "1.2.0"`
- ember-export-application-global is `"ember-export-application-global": "1.0.5"`
- ember-load-initializers is `"ember-load-initializers": "0.5.1"`
- ember-resolver is `"ember-resolver": "2.0.3"`
- loader.js to `"loader.js": "4.0.10"`


```diff

 {
   "devDependencies": {
     "broccoli-asset-rev": "2.4.5",
     "ember-ajax": "2.4.1",
-    "ember-cli": "2.7.0",
+    "ember-cli": "2.8.0-beta.1",
     "ember-cli-app-version": "1.0.0",
     "ember-cli-babel": "5.1.7",
     "ember-cli-dependency-checker": "1.3.0",
     "ember-cli-htmlbars": "1.0.10",
     "ember-cli-htmlbars-inline-precompile": "0.3.3",
     "ember-cli-inject-live-reload": "1.4.1",
     "ember-cli-jshint": "1.0.4",
     "ember-cli-qunit": "2.1.0",
     "ember-cli-release": "0.2.9",
     "ember-cli-sri": "2.1.0",
     "ember-cli-test-loader": "1.1.0",
     "ember-cli-uglify": "1.2.0",
-    "ember-data": "2.7.0",
+    "ember-data": "2.8.0-beta.1",
     "ember-export-application-global": "1.0.5",
     "ember-load-initializers": "0.5.1",
     "ember-resolver": "2.0.3",
     "loader.js": "4.0.10"
   }
 }

```
