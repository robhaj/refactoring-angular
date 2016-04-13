# Part 1

Show a regular angular app with a:
- global controller
- ngRoute module
- factory/service
- ng-repeat directive to loop through an array of objects and add it to the DOM

# Code
app.js
```js
var app = angular.module('myApp', ['ngRoute'])
```
main.html
```html
<!DOCTYPE html>
<html lang='en' ng-app='myApp'>
  <head>
    <meta charset='utf-8'>
    <title>My Angular App</title>
  </head>

  <body>
    <div ng-view></div>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.10/angular.min.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.10/angular-route.min.js"></script>
    <script src="app/app.js"></script>
  </body>
</html>
```
index.html
```html
<div> Testing the Router </div>
```
routes.js (remember to include this file in a script tag in main.html)
```js
app.config(['$routeProvider',
  function($routeProvider) {
    $routeProvider.
      when('/', {
        templateUrl: 'index.html',
        controller: 'myController'
      }).
      otherwise({
        redirectTo: '/'
      });
  }]);

```
service.js (remember to include this file in a script tag in main.html)
```js
app.service('myService', ['$http',function($http){
  return {
    getData: function() {
      return $http.get('/data')
    }
})
```
controller.js
```js
app.controller('myController', ['$scope','myService',function($scope, myService) {
$scope.data = myService.getData()
console.log($scope.data)
}])
```



# Global Controllers vs Isolated Controllers

Let's take a minute to talk about why this design pattern is very limiting.

A common problem with modern JavaScript is, while we can write large apps, the language doesn't support engineering our code in a cohesive way, keeping things adaptable with the scale.

One easy solution is to group and isolate pieces of code. Apps with loosely coupled 'components' encourage code reuse are easier to maintain.

Controllers are dying...because they aren't really needed. At least not as a separate entity. They are, however, still useful for directives. Our controller code will now live within them.
