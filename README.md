angular-rails-assets
====================

This is a module intended for [AngularJS](http://angularjs.org) apps working with the [Ruby on Rails asset pipeline](http://guides.rubyonrails.org/asset_pipeline.html). This is useful if you plan on using HAML as a preprocessor for your templates.

**I'm currently facing problems with recompiling the file in development - right now the file serves assets without fingerprinting as a temporary measure, but I hope to find a fix soon**

Installation
------------
Add the `angular-rails-assets.js.erb` file into your assets directory, and require it in your `application.js` file.

Usage
-----
`railsAssets` is an object with filenames as keys and asset paths as values. The filenames do not have preprocessor extensions - so `path/to/template.html.haml` will be just `path/to/template.html`. Note that only html files are loaded.

```javascript
// Include the module
angular.module('your-module', ['angular-rails-assets']);

// Inject railsAssets where required
angular.module('your-module')
  .directive('your-directive', funtion (railsAssets) {
    return {
      restrict: //...
      // Don't include post-processor extensions like .haml into the filename
      templateUrl: railsAssets['your-template.html'],
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

Setting up the rails asset pipeline to use HAML
-----------------------------------------------
First ensure you have the haml gem in your Gemfile:
```ruby
gem 'haml'
```

To enable haml for assets, add this in `config/initializers/haml_assets.rb` (or some other initializer):
```ruby
Rails.application.assets.register_engine '.haml', Tilt::HamlTemplate
```

And you're set. I personally put all my angularJS templates in `app/assets/templates`. I can get the path of the file `app/assets/templates/killer-template.html.haml` with:
```javascript
railsAssets('killer-template.html')
```