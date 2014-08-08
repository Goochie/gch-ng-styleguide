# AngularJS styleguide

*Organic AngularJS style guide for teams by [@billgch](//twitter.com/billgch)*

The basis and underlying principles supporting these styles is that of producing *clean*, *cohesive* code that can adhere's to *single responsibility principles*.


This styleguide comprises a combination of style's defined by [@toddmotto](//twitter.com/toddmotto) and [@john_papa](//twitter.com/john_papa), whilst incorporating my own guides and styles.

Their respective guides are located here and i encourage you to view them.

*[John's  guidelines](//github.com/johnpapa/angularjs-styleguide)*

*[Todd's  guidelines](https://github.com/toddmotto/angularjs-styleguide)*

## Table of Contents

  1. [IIFE scoping](#IIFE scoping)
  1. [Modules](#modules)
  1. [Services](#services)
  1. [Factory](#factory) 
  1. [Controllers](#controllers)
  1. [Directives](#directives)
  1. [Filters](#filters)
  1. [Routing resolves](#routing-resolves)
  1. [Publish and subscribe events](#publish-and-subscribe-events)
  1. [Angular wrapper references](#angular-wrapper-references)
  1. [Comment standards](#comment-standards)
  1. [Minification and annotation](#minification-and-annotation)


## IIFE scoping

  - **IIFE scoping**: To avoid polluting the global scope with our function declarations wrap them into an IIFE.
  
    ```javascript
    (function () {

      angular
        .module('app', []);
      
      // MainCtrl.js
      function MainCtrl () {

      }
      
      // SomeService.js
      function SomeService () {

      }
      
      //----------------------
      //Configure module
      //----------------------
      
      angular
        .module('app')
        .service('SomeService', SomeService);
        .controller('MainCtrl', MainCtrl);
      // ...
        
    })();
    ```

  
## Modules

  - **Definitions**: Declare modules without a variable using the setter and getter syntax

    ```javascript
    // bad
    var app = angular.module('app', []);
    app.controller();
    app.factory();

    // good
    angular
      .module('app', [])
      .controller()
      .factory();
    ```
    
  - **Methods**: Pass functions into module methods rather than assign as a callback

    ```javascript
    // bad
    angular
      .module('app', [])
      .controller('MainCtrl', function MainCtrl () {

      })
      .service('SomeService', function SomeService () {

      });

    // good
    function MainCtrl () {

    }
    function SomeService () {

    }
    angular
      .module('app', [])
      .controller('MainCtrl', MainCtrl)
      .service('SomeService', SomeService);
    ```

  - This aids with readability and reduces the volume of code "wrapped" inside the Angular framework
  - 
  
## Services

  - Services are classes and are instantiated with the `new` keyword. Use `this` for public methods and variables

    ```javascript
    function SomeService () {
      this.someMethod = function () {

      };
    }
    ```

**[Back to top](#table-of-contents)**

## Factory

  - **Singletons**: Factories are singletons, return a host Object inside each Factory to avoid primitive binding issues

    ```javascript
    // bad
    function AnotherService () {
      var someValue = '';
      var someMethod = function () {

      };
      return {
        someValue: someValue,
        someMethod: someMethod
      };
    }
    ```
    
  - Using the module pattern means that primitive values cannot update alone.
  - ??? This way, bindings are mirrored across the host Object. Primitive values cannot update alone using the revealing module pattern ????
    
    ```javascript
    // good
    function AnotherService () {
    
      var publicAPI = {};
      publicAPI.someValue = '';
      publicAPI.someMethod = function () {

      };
      return publicAPI;
      
    }
    ```
    
    

**[Back to top](#table-of-contents)**



## Controllers

  - **controllerAs syntax**: Controllers are classes, so use the `controllerAs` syntax at all times

    ```html
    <!-- bad -->
    <div ng-controller="MainCtrl">
      {{ someObject }}
    </div>

    <!-- good -->
    <div ng-controller="MainCtrl as main">
      {{ main.someObject }}
    </div>
    ```

  - In the DOM we get a variable per controller, which aids nested controller methods, avoiding any `$parent` calls

  - The `controllerAs` syntax uses `this` inside controllers, which gets bound to `$scope`

    ```javascript
    // bad
    function MainCtrl ($scope) {
      $scope.someObject = {};
      $scope.doSomething = function () {

      };
    }

    // good
    function MainCtrl () {
      this.someObject = {};
      this.doSomething = function () {

      };
    }
    ```

  - Only use `$scope` in `controllerAs` when necessary; for example, publishing and subscribing events using `$emit`, `$broadcast`, `$on` or `$watch`. Try to limit the use of these, however, and treat `$scope` uses as special

  - **Inheritance**: Use prototypal inheritance when extending controller classes

    ```javascript
    function BaseCtrl () {
      this.doSomething = function () {

      };
    }
    BaseCtrl.prototype.someObject = {};
    BaseCtrl.prototype.sharedSomething = function () {

    };

    AnotherCtrl.prototype = Object.create(BaseCtrl.prototype);

    function AnotherCtrl () {
      this.anotherSomething = function () {

      };
    }
    ```

  - Use `Object.create` with a polyfill for browser support

  - **Zero-logic**: No logic inside a controller, delegate to services

    ```javascript
    // bad
    function MainCtrl () {
      this.doSomething = function () {

      };
    }

    // good
    function MainCtrl (SomeService) {
      this.doSomething = SomeService.doSomething;
    }
    ```

  - Think "skinny controller, fat service"

**[Back to top](#table-of-contents)**

