# Part 1

Show a regular angular app with a:
- global controller
- ngRoute module
- service
- ng-repeat directive to loop through an array of objects and add it to the DOM

# Code
app.js
```js
var app = angular.module('myApp', ['ngRoute'])
```
index.html
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
main.html
```html
<div ng-repeat='person in data'>
  Name: {{person.name}} |
  Age: {{person.age}}
  <br>
</div>
```
routes.js (remember to include this file in a script tag in main.html)
```js
app.config(['$routeProvider',
  function($routeProvider) {
    $routeProvider.
      when('/', {
        templateUrl: 'main.html',
        controller: 'myController'
      }).
      otherwise({
        redirectTo: '/'
      });
  }]);

```
service.js (remember to include this file in a script tag in main.html)
```js
app.service('myService', function(){
  return {
    getData: function() {
      return [{
        "name":"Robby",
        "age":"24"
      },
      { "name":"Michael",
        "age":"33"
      },
      { "name":"Little Jimmy",
        "age":"12"
      }]
    }
  }
})
```
controller.js (remember to include this file in a script tag in main.html)
```js
app.controller('myController', ['$scope','myService',function($scope, myService) {
  $scope.data = myService.getData()
  console.log($scope.data)
}])
```

So now that we have our basic boilerplate set up, let's talk about it. We've got our app.js file that just instantiates a new angular module called myApp. We then setup our HTML template and make sure to include ng-app='myApp' in the HTML tag. Also, include angular, angular route, and our app.js in a script tag.

Next we created a routes.js file where we will setup the $routeProvider.

Then comes our service which I've called myService. We've attached a method called getData, and all that does is return an object for us to use.

Lastly we setup a global controller called myController. We have to inject our service to be able to call the getData method. We also inject $scope to talk to our view.


# Global Controllers vs Isolated Controllers

Let's take a minute to talk about why this design pattern is very limiting.

A common problem with modern JavaScript is, while we can write large apps, the language doesn't support engineering our code in a cohesive way, keeping things adaptable with the scale.

One easy solution is to group and isolate pieces of code. Apps with loosely coupled 'components' encourage code reuse are easier to maintain.

Controllers are dying...because they aren't really needed. At least not as a separate entity. They are, however, still useful for directives. Our controller code will now live within them.
