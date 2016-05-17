## Upgrading to Ember 2.4.3

After running ember init I needed to update my bower.json with these updates

- ember to `"ember": "2.4.3"`
- ember-cli-shims to `"ember-cli-shims": "0.1.1"`

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

package.json required

updates

- broccoli-asset-rev to `"broccoli-asset-rev": "2.4.2"`
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
- ember-cli-babel is `"ember-cli-babel": "5.1.6"`
- ember-cli-dependency-checker is `"ember-cli-dependency-checker": "1.2.0"`
- ember-cli-htmlbars-inline-precompile is `"ember-cli-htmlbars-inline-precompile": "0.3.1"`
- ember-cli-release is `"ember-cli-release": "0.2.8"`
- ember-cli-uglify is `"ember-cli-uglify": "1.2.0"`
- ember-resolver is `"ember-resolver": "2.0.3"`

notes

ember-cli-content-security-policy is now optional due to developer ergonomics

```diff

   "devDependencies": {
-    "broccoli-asset-rev": "2.2.0",
+    "broccoli-asset-rev": "2.4.2",
     "ember-ajax": "0.7.1",
     "ember-cli": "2.4.3",
     "ember-cli-app-version": "1.0.0",
     "ember-cli-babel": "5.1.6",
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