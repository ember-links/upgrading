#Upgrading to Ember 2

###Managing NPM Dependencies

> The caret, on the other hand, is more relaxed. It will update you to the most recent major version (the first number). ^1.2.3 will match any 1.x.x release including 1.3.0, but will hold off on 2.0.0.

I’ve found once or twice this has caused some issues (at least I *think* it was the cause), so I tend to prefer to use strict versions and update the dependencies myself explicitly.

I’d be interested to hear if you think this is a good/bad idea and why.

### Flushing Deprecations

`deprecationWorkflow.flushDeprecations()`

## Upgrading to Ember 2.4.3

Once Ember CLI is at v2.3.0 then it is a decision to make whether to update each dot release, or to roll them into one.

As the upgrade guide suggested there were not too many user changes to make between the dot releases I updated from 2.3.0 directly to 2.4.3

Folding together the changes listed made this:

- update ember-cli-sri in package.json to 2.1.0
- rename testem.json to testem.js

But in reality I had to make a few more updates, listed below.

###Install Ember CLI

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

###Running `ember init`

Begin the process of updating your project files

- `ember init`

For this version the only files I hit `Y` to overwrite were

- `.travis.yml`
- `tests/helpers/module-for-acceptance.js` (destroyApp moved after conditional)

Though check that the you're `index.html`& `tests/index.html` look OK because there have been some subtle updates in the use of double quotes for `content-for` tags. For `tests/index.html` you might also want to check that you have `<script src="testem.js" integrity=""></script>` included now.

No other files appeared to have any code that needed to be updated, except for bower.json & package.json — the usual culprits — which I always prefer not to overwrite automatically and update by hand afterwards (see below).

####bower.json

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

####package.json

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

###Testing the new version

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

###Running `ember init`

Begin the process of updating your project files

- `ember init`

####bower.json

update

- ember to `"ember": "2.5.0"`

check

- ember-cli-shims is `"ember-cli-shims": "0.1.1"`
- ember-cli-test-loader is `"ember-cli-test-loader": "0.2.2"`
- ember-qunit-notifications is `"ember-qunit-notifications": "0.1.0"`

```diff

 {
   "dependencies": {
-    "ember": "2.4.3",
+    "ember": "2.5.0",
     "ember-cli-shims": "0.1.1",
     "ember-cli-test-loader": "0.2.2",
     "ember-qunit-notifications": "0.1.0"
   }
 }

```

####package.json

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

###Testing the new version

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
