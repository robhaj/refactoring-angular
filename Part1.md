# Part 1

Show a regular angular app with a:
- global controller
- ngRoute module
- factory/service
- ng-repeat directive to loop through an array of objects and add it to the DOM

# Global Controllers vs Isolated Controllers

Let's take a minute to talk about why this design pattern is very limiting.

A common problem with modern JavaScript is, while we can write large apps, the language doesn't support engineering our code in a cohesive way, keeping things adaptable with the scale.

One easy solution is to group and isolate pieces of code. Apps with loosely coupled 'components' encourage code reuse are easier to maintain.

Controllers are dying...because they aren't really needed. At least not as a separate entity. They are, however, still useful for directives. Our controller code will now live within them.
