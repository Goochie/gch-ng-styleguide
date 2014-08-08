# AngularJS styleguide

*Organic AngularJS style guide for teams by [@billgch](//twitter.com/billgch)*

The basis and underlying principles supporting these styles is that of producing *clean*, *cohesive* code that can adhere's to *single responsibility principles*.


This styleguide comprises a combination of style's defined by [@toddmotto](//twitter.com/toddmotto) and [@john_papa](//twitter.com/john_papa), whilst incorporating my own guides and styles.

Their respective guides are located here and i encourage you to view them.

[John's  guidelines](//github.com/johnpapa/angularjs-styleguide)

[Todd's  guidelines](https://github.com/toddmotto/angularjs-styleguide)

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

  - **IIFE scoping**: To avoid polluting the global scope with our function declarations that get passed into Angular,  ensure build tasks wrap the concatenated files inside an IIFE
  
    ```javascript
    (function () {

      angular
        .module('app', []);
      
      // MainCtrl.js
      function MainCtrl () {

      }
      
      angular
        .module('app')
        .controller('MainCtrl', MainCtrl);
      
      // SomeService.js
      function SomeService () {

      }
      
      angular
        .module('app')
        .service('SomeService', SomeService);
        
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
