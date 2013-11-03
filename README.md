angular-rails-assets
====================

This is a module intended for [AngularJS](http://angularjs.org) apps working with the [Ruby on Rails asset pipeline](http://guides.rubyonrails.org/asset_pipeline.html). This is useful if you plan on using HAML as a preprocessor for your templates.

Installation
------------
Add the `angular-rails-assets.js.erb` file into your assets directory, and require it in your `application.js` file.

Usage
-----
`railsAssets` is an object with filenames as keys and asset paths as values. The filenames do not have preprocessor extensions - so `path/to/template.html.haml` will be just `path/to/template.html`.

```javascript
// Include the module
angular.module('your-module', ['angular-rails-assets']);

// Inject railsAssets where required
angular.module('your-module')
  .directive('your-directive', funtion (railsAssets) {
    return {
      restrict: //...
      templateUrl: railsAssets['your-template.html'],
      // Don't include post-processor extensions like .haml into the filename
      scope: {
        // ...
      },
      /*
       * directive stuff...
       *
       */
    };
  });

/* You can inject the assets into a config block for your routes,
 * since it is loaded as a constant
 */
angular.module('your-module')
  .config(function ($routeProvider, railsAssets) {

    $routeProvider.when('/some-route', {
      templateUrl: railsAssets['template-name.html'],
      controller: 'YourController'
    })
  });
```

Mocking for tests
-----------------
If you include `angular-rails-assets-mock.js` in your tests, it merely replaces `railsAssets` with an empty object