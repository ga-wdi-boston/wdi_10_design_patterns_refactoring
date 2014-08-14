# Refactoring

...is defined as making code more readable, maintainable, or robust *without* changing any of its functionality. Having a comprehensive test suite allows you to refactor with confidence &ndash; if all your tests are still green, you can be reasonably sure you haven't changed any functionality. Refactoring should *not* be something that is done separately from "regular development": It should be part of the work you do before shipping any feature (the TDD cycle is Red, Green, **Refactor** for a reason).

**Code smells** are particular patterns of "bad code" that can crop up in almost any software project. Putting names on these patterns allows us to identify and talk about them more easily, and since their consequences are well-understood, reduces arguments over whether they are worth fixing.

**Refactoring** used as a noun ("a refactoring", "many refactorings") can also refer to a particular pattern of code changes that can be applied to address specific code smells. These patterns are well-defined and well-understood, which again makes them easier to talk about and use.

**Design patterns** are techniques for writing software that are generally understood to result in more maintainable code. Unlike a refactoring, a design pattern is not a *process* geared towards resolving specific code smells; it's a *template* for solving a certain class of problems. You may also hear about **antipatterns** &ndash; these are design patterns that are known to result in "bad code".

## Basic Principles

These principles of software development are useful in refactoring Rails apps:

* [Don't Repeat Yourself](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (DRY)
* [Single Responsibility Principle](http://en.wikipedia.org/wiki/Single_responsibility_principle)
* [Tell, Don't Ask](http://robots.thoughtbot.com/tell-dont-ask)
* [Law of Demeter](http://ablogaboutcode.com/2012/02/27/understanding-the-law-of-demeter/)
* [Composition Over Inheritance](http://www.naildrivin5.com/blog/2012/12/19/re-use-in-oo-inheritance.html)

## [Code Smells](http://sourcemaking.com/refactoring/bad-smells-in-code) and [Refactoring](http://sourcemaking.com/refactoring)

The guide linked above is excellent, though not specifically written with Ruby or Rails in mind. The list of [Code Smells](https://github.com/troessner/reek/wiki/Code-Smells) on the `reek` wiki is, and for a more detailed guide, check out the book [Ruby Science](https://learn.thoughtbot.com/products/13-ruby-science). Also check out [SOLID Design Principles](http://blog.rubybestpractices.com/posts/gregory/055-issue-23-solid-design.html).

## Specific Refactorings in Rails

There are a few more specific forms of the [Extract Class](http://sourcemaking.com/refactoring/extract-class) refactoring that can be performed in Rails. Some of these are outlined in this excellent "[7 Patterns](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/)" blog post.

* **Extract Form Object:** [active_attr](https://github.com/cgriego/active_attr) makes it easy to create plain Ruby classes that behave like Rails models, including being able to pass them to `form_for` and use strong parameters.
* **Extract Decorator:** [draper](https://github.com/drapergem/draper) is a popular option. Never write a view helper again!

## SublimeLinter Plugins

These are not really refactoring-related but will help you catch syntax errors, unused variables, etc.

* `brew install node`
* `npm install -g jshint coffee-script coffeelint`
* Install packages in ST:
 * SublimeLinter
 * SublimeLinter-coffee
 * SublimeLinter-coffeelint
 * SublimeLinter-jshint
 * SublimeLinter-ruby

**Note:** SublimeLinter-rubocop might work if `rubocop` gem is installed, or it might not. Check the console (`Ctrl+Backtick`) to see if it failed to load.

## Duplication and Complexity Analysis

These can help you identify areas of duplicated code or high code complexity for potential refactoring.

* `gem install flog flay flay-js reek rails_best_practices`
* `gem uninstall rkelly` (accept the warning)
* `gem install rkelly-remix`
* Create `~/.jshintrc` and add the following:
```
{
    "maxparams": 4,
    "maxdepth": 4,
    "maxstatements": 15,
    "maxcomplexity": 6
}
```
* Create `~/coffeelint.json` and add the following:
```
{
    "cyclomatic_complexity": {
        "name": "cyclomatic_complexity",
        "value": 6,
        "level": "warn"
    }
}
```

### Usage

All of these would be executed in the root of your app. In terms of code complexity, single methods higher than 10 or classes higher than 50 are usually cause for concern.

* Ruby complexity: `flog -g app` (this gives the top 40% of complexity, add `-a` to show all)
* JavaScript complexity: `jshint app`
* CoffeeScript complexity: `coffeelint app`
* Ruby+JavaScript duplication: `flay -j app`
* Ruby code quality issues: `reek -S app`
* Rails code quality issues: `rails_best_practices`

There aren't many good tools for CoffeeScript duplication detection at the moment. If you find any, let me know!
