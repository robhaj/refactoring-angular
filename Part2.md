# Part 2

Refactor into two custom Directives, removing the global Controller. Keep ngRoute and the Service. Make sure to use isolate scope. Talk about why this is better code.


# Design Pattern Notes

Since Angular 1.3, controllers in the global scope are no longer 'supported'. We should stop writing controllers as separate entities. We'll be moving logic from our controllers to services so we end up with thin controllers that just handle DOM interactions. We should stop also stop associating directives as just reusable code. It’s a *building block*.

We can remove our controllers by using directives everywhere you’d use a controller. That means your app’s code is now either in a directive or in a factory/service.

You’ve probably been told not to do DOM manipulation in the controller many times. However, this isn’t a controller anymore. It’s a *component*.

Isolated directives are self-contained components that are easy to maintain.

## Code
We can keep our boilerplate from Part1. We'll delete the controller.js to get rid of our global controller, also remember to remove the script tag linking that file in your main.html.

Next, we'll add a
directives.js
```js
app.directive('myDirective', function() {
  return {
    restrict: 'EA',
    templateUrl: 'myDirective.html',
    scope: {}
  }
})

app.directive('myOtherDirective', function() {
  return {
    restrict: 'EA',
    templateUrl: 'myOtherDirective.html',
    scope: {}
  }
})
```
myDirective.html
```html
<div>My directive!</div>
```
myOtherDirective.html
```html
<div>My other directive!</div>
```
Now we have declared two directives, one called myDirective, the other called myOtherDirective. We return a Directive Definition Object, or a DDO, from the callback function passed two the directive method. We've added the restrict property and set it to 'EA' which restricts the directive to an element or an attribute. We also add a templateUrl property which is a path to the html file for the directive. We set scope to an empty object to create an isolated scope.


## Defining routes

You no longer have to specify a controller in the ui-router or ng-route configuration. Just pass a template of the directive to the template property. So instead of passing an HTML file path we pass the actual element. We also remove the controller property because next we will add a controller property to the DDO.

routes.js
```js
app.config(['$routeProvider',
  function($routeProvider) {
    $routeProvider.
      when('/', {
        template: '<my-directive></my-directive>'
      }).
      when('/', {
        template: '<my-other-directive></my-other-directive>'
      })
      otherwise({
        redirectTo: '/'
      });
  }]);
```

Now, at the root url, you should see "My directive".
When you browse to /other you should see "My other directive!"
