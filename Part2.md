#Part 2

Refactor into two custom Directives, removing the global Controller. Keep ngRoute and the Factory. Make sure to use isolate scope. Talk about why this is better code.


# Design Pattern Notes

Since Angular 1.3, controllers in the global scope are no longer 'supported'. We should stop writing controllers as separate entities. We'll be moving logic from our controllers to services so we end up with thin controllers that just handle DOM interactions. We should stop also stop associating directives as just reusable code. It’s a *building block*.

We can remove our controllers by using directives everywhere you’d use a controller. That means your app’s code is now either in a directive or in a factory/service.

You’ve heard the mantra “Don’t do DOM manipulation in controllers” a thousand times. But this isn’t a controller anymore. It’s a component.

Isolated directives are self-contained components that are easy to maintain.

Defining routes: You no longer need to supply a controller in your ui-router or ng-route configuration. Just pass a simple template such as <messages></messages>.
