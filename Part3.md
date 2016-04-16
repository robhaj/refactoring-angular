#Part 3

Using Angular 1.5.3 refactor into a component. Use the component router. Make sure to talk about push state. (You have to use it with the component router.) Make sure to turn HTML5 mode on. All a component is is a directive with isolated scope.

## Components!
Advantages:
- simpler configuration than plain directives
- promote sane defaults and best practices
- optimized for component-based architecture
- writing component directives will make it easier to upgrade to Angular 2

When not to use Components:

- directives that rely on DOM manipulation like adding event listeners (compile and link functions are unavailable)
- advanced directive definition options (priority, terminal, multi-element)
- when you need a directive triggered by an attribute or CSS class, rather than an element

Components take two arguments
- The name of the Component as a string.
- The Component Config Object(CCO) (as apposed to Directive Definition Object(DDO)).
  - unlike .directive(), this method does not take a factory function.

```js
app.component('heroDetail', {
  templateUrl: 'heroDetail.html',
  controller: HeroDetailController,
  bindings: {
    hero: '='
  }
});
```
## Removing the Scope object

Don't panic...we will still have the concept of scope. It'll just bind to the directive. Having different kinds of scope, like isolated and non-isolated, can easily lead to issues. It's best to keep things focused on isolated scopes.

From Angular 1.1 we've been able to attach our view properties on the controller instead of the $scope object. Given that $scope is being removed in 2.0 it's a great incentive to start doing this now. This means attaching our functions and values to this instead of $scope. This is called controller as.

Using controller as makes it obvious which controller you are accessing in the template when multiple controllers apply to an element.

As the scope concept takes shape in 2.0 we should rely more on isolated scope as that also leads to cleaner code. bindToController was introduced in 1.3 to help us use isolated scope on the controller with a directive:

Directives with a controller function may not need a link function. That’s mostly because controller means I rarely need to inject $scope and using controllers they can then be required child directives.

Before:
```js
module.controller('MessagesCtrl', function() {
  var self = this;
  self.list = [
    {text: 'Hello, World!'}
  ];
  self.clear = function() {
    self.list = [];
  };
});
```
Template:
```html
<div ng-controller="MessagesCtrl as messages">
  <ul>
    <li ng-repeat="message in messages.list">
      {{message.text}}
    </li>
  </ul>
  <button ng-click="messages.clear()">Clear</button>
</div>
```

After:

```js
module.directive('messages', function() {
  function MessagesCtrl() {
    var self = this;
    self.list = [
      {text: 'Hello, World!'}
    ];
    self.clear = function() {
      self.list = [];
    };
  }
  return {
    templateUrl: '', // Same as for controller
    scope: {}, // Isolate == Awesome
    controller: MessagesCtrl,
    controllerAs: 'messages',
    bindToController: true
  };
});
```

The template didn’t need any changes. Same for the controller function itself. We define the directive with controller:, controllerAs:, bindToController: and isolated scope:

## Link function vs Controller function

You might still need link functions because the controller function executes before the element renders. If you want to register an event handler on a child element then you’ll have to do it in the link function. Another is when you want to use require and get access to that controller.

## Component Router

In AngularJS 1.5, the Component Router was introduced. Each Component in the application can have a Router associated with it. It is recommended to develop AngularJS applications as a hierarchy of Components. Each Component is an isolated part of the application, which is responsible for its own user interface. In Angular, a Component is a special kind of directive that uses a simpler configuration which is suitable for a component-based application structure.

This makes it easier to write an app in a way that's similar to using Web Components or using Angular 2's style of application architecture.

```js
angular.module('app', ['ngNewRouter'])
  .controller('AppController', ['$router', AppController]);

AppController.$routeConfig = [
  { path: '/',           component: 'home' },
  { path: '/detail/:id', component: 'detail' }
];
function AppController ($router) {}
```
```js
angular.module('heroApp', []).controller('mainCtrl', function() {
  this.hero = {
    name: 'Spawn'
  };
});
```
