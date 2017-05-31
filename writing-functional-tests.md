# Writing Functional Tests

WP-CLI uses [Behat](http://behat.org/en/latest/) (the official [Cucumber](https://cucumber.io/) implementation for PHP) for its functional test framework of choice. Unlike unit tests, functional tests are high-level tests focused on the behavior of the software.
These tests are written as `.feature` files in a plain and simple language called [Gherkin](https://cucumber.io/docs/reference#gherkin) and reside in the top-level `features` directory. Each feature file roughly describes a single feature (usually a sub-command), by outlining multiple "scenarios".

For WP-CLI, the goal of functional tests is to create a written specification as to how each command is expected to work. This specification is both valuable for both developers and non-developers alike as it is simply describing how it works.

## Hello World

Let's look at a very basic example

```cucumber
Feature: Test that WP-CLI loads.

  Scenario: WP-CLI loads for your tests
    Given a WP install

    When I run `wp post get 1`
    Then STDOUT should contain:
      """
      Hello world!
      """
      
```

The `Feature` keyword identifies the feature that is being described, and is only used once in each file.
The `Scenario` keyword identifies an individual "test" which is comprised of a group of Steps.

Steps are simple single-line instructions or declarations that describe the scenario. A step typically starts with `Given`, `When` or `Then`. If there are multiple `Given` or `When` steps underneath each other, you can use `And` or `But`. Behat does not differentiate between the keywords, but choosing the right one is important for the readability of the scenario as a whole.

The code above is a complete working example of a possible feature file. If we saved this into `features/hello-world.feature` we could run it like so:

```
$ vendor/bin/behat features/hello-world.feature

PHP binary: /usr/local/Cellar/php71/7.1.4_16/bin/php
PHP version:    7.1.4
php.ini used:   /usr/local/etc/php/7.1/php.ini
WP-CLI root dir:    /Users/aaemnnosttv/wp-cli
WP-CLI packages dir:    
WP-CLI global config:   
WP-CLI project config:  
WP-CLI version: 1.1.0

WordPress 4.7.5

Feature: Test that WP-CLI loads.

  Scenario: WP-CLI loads for your tests # features/hello-world.feature:3
    Given a WP install                  # features/steps/given.php:54
    When I run `wp post get 1`          # features/steps/when.php:29
    Then STDOUT should contain:         # features/steps/then.php:15
      """
      Hello world!
      """

1 scenario (1 passed)
3 steps (3 passed)
0m2.534s
```

> Testing that getting the hello-world post returns the title text "Hello World!" anywhere in the output.

## Scenarios

Each Scenario of a feature is run in a new empty directory in your system's temp directory. For this reason, each scenario should define "the world" in which it is operating in. For this reason, it is common to have some duplication between scenarios to setup the context for the test.

## Steps

As you can see above, each step in the scenario has a corresponding definition which executes a corresponding handler in PHP.
`Given a WP install` is one of many step definitions that comes with the WP-CLI functional test suite. This step creates a generic yet real, fully-installed WordPress instance in the context of the current working directory of the scenario.
`When I run ...` is another step definition that will run the given command in back-ticks and capture its output so we can make assertions about it.
`Then STDOUT should contain` is a simple definition for checking for the presence of the given text within the provided text block.


