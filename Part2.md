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
app.directive('myDirective', function (myService) {
  return {
    restrict: 'EA',
    scope: {},
    controller: function (myService) {
      this.people = 'people'
    },
    controllerAs: 'ctrl1',
    bindToController: true,
    template: '<div> {{people}} </div>'
  };
});

app.directive('myOtherDirective', function (myService) {
  return {
    restrict: 'EA',
    scope: {},
    controller: function (myService) {
      this.people = myService.getData()
    },
    controllerAs: 'ctrl2',
    bindToController: true,
    template: '<div ng-repeat="person in people">{{person.name}}</div>'
  };
});
```
Now we have declared two directives, one called myDirective, the other called myOtherDirective. We return a Directive Definition Object, or a DDO, from the callback function passed two the directive method. We've added the restrict property and set it to 'EA' which restricts the directive to an element or an attribute. We set scope to an empty object to create an isolated scope.


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
      when('/other', {
        template: '<my-other-directive></my-other-directive>'
      }).
      otherwise({
        redirectTo: '/'
      });
  }]);
```

Now, at the root url, you should see "My directive".
When you browse to /other you should see "My other directive!"

## Bind to controller
Angular 1.3 introduces a new property to the directive definition object called bindToController, which does exactly what it says. When set to true in a directive with isolated scope that uses controllerAs, the component’s properties are bound to the controller rather than to the scope.
That means, Angular makes sure that, when the controller is instantiated, the initial values of the isolated scope bindings are available on this, and future changes are also automatically available.
Let’s apply bindToController to our directive and see how the code becomes cleaner.

Improvements in 1.4
In version 1.4, bindToController gets even more powerful. When having an isolated scope with properties to be bound to a controller, we always define those properties on the scope definition and bindToController is set to true. In 1.4 however, we can move all our property binding definitions to bindToController and make it an object literal.
Here’s an example with a component directive that uses bindToController. Instead of defining the scope properties on scope, we declaratively define what properties are bound to the component’s controller:
